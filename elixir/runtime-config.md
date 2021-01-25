# Runtime Configuration

Runtime configuration for Elixir applications can be specified in `config/releases.exs` (introduced in Elixir 1.9) or `config/runtime.exs` (introduced in Elixir 1.11). However, `config/releases.exs` is executed at runtime only in a release environment, while `config/runtime.exs` is executed at runtime in a release environment and also in all `Mix` environments (`dev`,`prod`,`test`). When both `config/releases.exs` and `config/runtime.exs` exist, `config/runtime.exs` will be executed, and `config/releases.exs` will be ignored.

Runtime configuration is combined with and overrides the build-time configuration defined using the "traditional" Elixir approach of specifying configuration in `config/config.exs`, `config/dev.exs`, `config/prod.exs`, and `config/test.exs` Note that the use of `import_config/1` is explicitly disallowed in `config/runtime.exs`, so hybrid schemes in which `config/runtime.exs` attempts to import `config/releases.exs` will not work.

## Considerations for RentPath Elixir Applications

Runtime configuration of RentPath production applications is often accomplished by reading environment variables set via `Consul`. Each RentPath Elixir application is deployed to production in an Elixir release built in the `Mix` `prod` environment. Thus, either `config/releases.exs` or `config/runtime.exs` could be used for application configuration at runtime in production. However, since `config/runtime.exs` is always executed at runtime in any environment, internal conditional logic must be used to select the desired configuration. In the absence of internal conditional logic, required environment variables would also need to be set during development and testing, at the cost of some inconvenience.

```
# config/runtime.exs, defining only runtime configuration

import Config

if config_env() == :prod do
  config :sentry, :environment_name, System.fetch_env!("ENVIRONMENT")
end
```

In contrast, since development and testing workflows are typically executed outside a release, using `config/releases.exs` does not require internal conditional logic, since a release and the `Mix` `prod` environment are implied.

```
# config/releases.exs

import Config

config :sentry, :environment_name, System.fetch_env!("ENVIRONMENT")
```

A significant subset of the configuration required by an application can often be defined at build-time, using the "traditional" Elixir approach. However, it would be possible to specify _all_ configuration in `config/runtime.exs`, using conditional logic to dedicate sections of the file to each of the Boolean combinations of the `Mix` environments of interest. This approach has the (arguable) advantage of consolidating all configuration in one file, but this approach loses the convenience of the "configuration by difference" enabled by the several configuration files used in the "traditional" approach and also implies either duplication of configuration or more complicated conditional logic.

```
# config/runtime.exs, consolidated configuration

import Config

if config_env() == :prod do
  config :sentry, :environment_name, System.fetch_env!("ENVIRONMENT")
else
  config :sentry, :environment_name: to_string(config_env())
end
```

## Configuration Recommendations for RentPath Elixir Applications

Migrating the existing RentPath Elixir applications created prior to Elixir 1.11 to each use a consolidated configuration would require significant effort, and the arguable advantage(s) do not justify the effort. It is recommended that Elixir applications continue to use build-time configuration and "configuration by difference" when appropriate.

Runtime configuration should be defined in `config/releases.exs`. However, `config/runtime.exs` can be used locally to make some scenarios more convenient. Since `config/runtime.exs` effectively "blocks" `config/releases.exs`, the former should not be committed to GitHub or even exist locally outside of these scenarios. It is recommended that `config/runtime.exs` be added to `.gitignore`.

### Example

A detailed example will illustrate the overall approach and some scenarios where `config/runtime.exs` may be convenient. Consider a service, such as the SEO Metadata Service (SEOMD), that uses a `PostgreSQL` database. Development and testing will use an appropriately seeded local database, while access to `PostgreSQL` in a release deployed to production will be configured via `Consul` environment variables.

The following `PostgreSQL` connection parameters must be configured in all environments.

  * `database`
  * `hostname`
  * `password`
  * `username`

The `PostgreSQL` configuration for development and testing is defined using the "traditional" approach and "configuration by difference". Thus, environment variables for `PostgreSQL` are _not_ needed for local development and testing.

```
# config/config.exs

import Config

config :seo_metadata_service, SeoMetadataService.DataStore.Postgres.Repo,
  hostname: "localhost",
  password: "postgres",
  username: "postgres"
```

```
# config/dev.exs

import Config

config :seo_metadata_service, SeoMetadataService.DataStore.Postgres.Repo,
  database: "seo_metadata_service_dev"
```

```
# config/test.exs

import Config

config :seo_metadata_service, SeoMetadataService.DataStore.Postgres.Repo,
  database: "seo_metadata_service_test"
```

Runtime configuration is defined in terms of respective environment variables for `PostgreSQL` that _must_ exist in `Consul`. Recall that the release configuration will be combined with and override the build-time configuration. Thus, it is ensured that `PostgreSQL` access in a production deployment is configured solely via `Consul`.

```
# config/releases.exs

import Config

config :seo_metadata_service, SeoMetadataService.DataStore.Postgres.Repo,
  database: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_DATABASE"),
  hostname: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_HOSTNAME"),
  password: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_PASSWORD"),
  username: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_USERNAME")
```

### Local Use of `config/runtime.exs`

The use of `config/runtime.exs` can eliminate some code that was required prior to Elixir 1.11 and also make some scenarios more convenient.

#### Scenario: Local Debugging using Non-Local Data

Assume an issue with a deployed production release of SEOMD is noted in one or more of the `CI`, `QA`, or live production environments. The issue can be explored locally by copying `config/releases.exs` to `config/runtime.exs` and doing the appropriate edits. Then, SEOMD can be started locally, with the environment variables of interest provided by `envconsul`, and the issue can be explored with the relevant data. Of course, in scenarios where the data is not read-only, *extreme caution must be used*.

```
# config/runtime.exs

import Config

config :seo_metadata_service, SeoMetadataService.DataStore.Postgres.Repo,
  database: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_DATABASE"),
  hostname: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_HOSTNAME"),
  password: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_PASSWORD"),
  username: System.fetch_env!("SERVICES_SEO_METADATA_POSTGRES_USERNAME")
```

Once debugging is complete, simply delete or rename `config/runtime.exs`.

#### Scenario: Migrating a Non-Local Database with Local Commands

Assume it is necessary to migrate a `PostgreSQL` database in one or more of the `CI`, `QA`, or live production environments. Again, an appropriate copy and edit of `config/runtime.exs` and use of `envconsul` will enable a local `mix ecto.migrate` command to migrate a non-local database. This capability is particularly useful, given restricted access to containers deployed via `Kubernetes`.

#### Code Elimination

Prior to Elixir 1.11, [code](https://github.com/rentpath/seo_metadata_service/blob/20210120-141-1e5a6be/lib/seo_metadata_service/data_store/postgres/repo.ex#L4-L18) was written to enable partial or full override of the build-time configuration with environment variables at runtime. Using `config/releases.exs` and `config/runtime.exs` as explained above can eliminate the need for such code. While such code is not unduly complex, some development, testing, and maintenance effort can be saved.
