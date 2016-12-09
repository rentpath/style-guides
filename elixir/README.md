# Prelude

> In general, our main asset is the Erlang VM. We wouldn't have Elixir if it
> was not for the Erlang VM. Put that together with our meta-programming and
> extensibility mechanisms and you get a solid look of what are our goals and
> what we want you to be able to achieve with the language.
> -- José Valim

# Style Guide

RentPath uses [René Föhring's](https://github.com/rrrene) Elixir Style Guide,
which can be found [here](https://github.com/rrrene/elixir-style-guide).

# Code Analysis

RentPath uses [Credo](https://github.com/rrrene/credo) for code analysis. See
`.credo.exs` for an example `Credo` config file.

# Elixir Installation and Version Management

A popular, though not required, option for installing and managing multiple
versions of Elixir is to use [exenv][1] and [elixir-build][2].

Follow the installation instructions for those two applications. Then install
the elixir version in use for the application available in the
`.exenv-version` file at the root of the project:

    $ exenv install <a.b.c>

After installing the version, tell `exenv` to rehash.

    $ exenv rehash

If you are not in a directory that contains a `.exenv-version` file and do not
have a system elixir installed, `elixir -v` will not report anything. You must
either set `exenv`s local or global version or create a `.exenv-version` file.

    $ exenv local a.b.c
    # OR
    $ exenv global a.b.c
    # OR
    $ echo 'a.b.c' >> ./.exenv-version

Going forward `exenv` will detect and automatically change your `elixir`
version based on the `.exenv-version` file in the project you are working on.

[1]:https://github.com/mururu/exenv
[2]:https://github.com/mururu/elixir-build
