# Prelude

> In general, our main asset is the Erlang VM. We wouldn't have Elixir if it
> was not for the Erlang VM. Put that together with our meta-programming and
> extensibility mechanisms and you get a solid look of what are our goals and
> what we want you to be able to achieve with the language.
> -- José Valim

# Style Guide

RentPath uses Christopher Adams's Elixir Style Guide,
which can be found [here](https://github.com/christopheradams/elixir_style_guide).

# Code Analysis

RentPath uses [Credo](https://github.com/rrrene/credo) for code analysis. See
[`.credo.exs`](.credo.exs) for an example `Credo` config file.

# Elixir Installation and Version Management

The team has standardized on [asdf-elixir][1] for installing and managing
multiple versions of Elixir. Each Elixir repository contains an asdf
`.tool-versions` file in the top level directory that specifies the version to
be used for Erlang, Elixir and any other languages used by the application.
Using asdf is not required, but you'll likely need to manually specify the
Elixir version when using another tool.

Setting up asdf for our Elixir projects involves several steps but is easy to
do:

1. Follow the [installation instructions for asdf][2] for your OS.
1. Follow the [instructions for installing the Erlang plugin][3].
1. Follow the [instructions for installing the Elixir plugin][4].

After completing these steps you'll need to build versions of Erlang and Elixir
for each Elixir project you work with. The easiest way to do this is:

    # Navigate to the project directory
    $ cd <elixir project cloned from GitHub>

    # Run the asdf command to install Erlang and Elixir versions specified
    # in the project's .tool-version file
    $ asdf install

Note that the Erlang build will take some time, but in a few minutes the
command should finish and you'll have Erlang and Elixir all set up and ready
to be used. Run `asdf reshim` for good measure after `asdf install`.

If you've already built the correct version of Erlang and Elixir the `asdf
install` command will print something like the following:

    $ asdf install
    elixir 1.12.3-otp-24 is already installed
    erlang 24.3.4.5 is already installed

[1]:https://github.com/asdf-vm/asdf-elixir
[2]:https://asdf-vm.com/guide/getting-started.html
[3]:https://github.com/asdf-vm/asdf-erlang#use
[4]:https://github.com/asdf-vm/asdf-elixir#install
