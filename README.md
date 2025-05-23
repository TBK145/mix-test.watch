# mix test.watch

[![Build Status](https://travis-ci.org/lpil/mix-test.watch.svg?branch=master)](https://travis-ci.org/lpil/mix-test.watch)
[![Module Version](https://img.shields.io/hexpm/v/mix_test_watch.svg)](https://hex.pm/packages/mix_test_watch)
[![Hex Docs](https://img.shields.io/badge/hex-docs-lightgreen.svg)](https://hexdocs.pm/mix_test_watch/)
[![Total Download](https://img.shields.io/hexpm/dt/mix_test_watch.svg)](https://hex.pm/packages/mix_test_watch)
[![License](https://img.shields.io/hexpm/l/mix_test_watch.svg)](https://github.com/lpil/mix-test.watch/blob/master/LICENSE)
[![Last Updated](https://img.shields.io/github/last-commit/lpil/mix-test.watch.svg)](https://github.com/lpil/mix-test.watch/commits/master)

Automatically run your Elixir project's tests each time you save a file.

Because [Test-driven development (TDD)](https://en.wikipedia.org/wiki/Test-driven_development)
is awesome.

## Usage

Add it to your dependencies:

```elixir
# mix.exs
def deps do
  [
    {:mix_test_watch, "~> 1.0", only: [:dev, :test], runtime: false}
  ]
end
```

Optionally, add `preferred_cli_env: ["test.watch": :test]` for running `mix test.watch` in `:test` env by default.
It prevents the task from running `:dev` env config scripts.

```elixir
# mix.exs
def project do
  [
    ...
    preferred_cli_env: [
      "test.watch": :test
    ],
    ...
  ]
end
```

Run the mix task:

```
mix test.watch
```

Start hacking :)

## Running Additional Mix Tasks

Through the mix config it is possible to run other mix tasks as well as the
test task. For example, if I wished to run the [Credo][credo] code analysis
tool after my tests I would do so like this.

[credo]: https://github.com/rrrene/credo

```elixir
# config/config.exs
use Mix.Config

if Mix.env == :dev do
  config :mix_test_watch,
    tasks: [
      "test",
      "credo",
    ]
end
```

Tasks are run in the order they appear in the list, and the progression will
stop if any command returns a non-zero exit code.

All tasks are run with `MIX_ENV` set to `test`.

## Passing Arguments To Tasks

Any command line arguments passed to the `test.watch` task will be passed
through to the tasks being run. If I only want to run the tests from one file
every time I save a file I could do so with this command:

```
mix test.watch test/file/to_test.exs
```

Note that if you have configured more than one task to be run these arguments
will be passed to all the tasks run, not just the test command.

## Running tests of modules that changed

Elixir's `mix test` provides a `--stale` option that will run only those test files which reference modules that have changed since the last run. You can pass it to test.watch:

```
mix test.watch --stale
```

## Clearing The Console Before Each Run

If you want mix test.watch to clear the console before each run, you can
enable this option in your `config/dev.exs` as follows:

```elixir
# config/config.exs
use Mix.Config

if Mix.env == :dev do
  config :mix_test_watch,
    clear: true
end
```

## Excluding files or directories

To ignore changes from specific files or directories just add `exclude:` regexp
patterns to your config in `config/config.exs`:

```elixir
# config/config.exs
use Mix.Config

if Mix.env == :dev do
  config :mix_test_watch,
    exclude: [~r/db_migration\/.*/,
              ~r/useless_.*\.exs/]
end
```

The default is `exclude: [~r/\.#/, ~r{priv/repo/migrations}]`.

## Watching files with other extensions

To watch files with extensions other than the default (`.erl`, `.ex`, `.exs`, `.eex`, `.leex`, `.heex`, `.xrl`, `.yrl`, `.hrl`) add them to `extra_extensions:` in your config:

```elixir
# config/config.exs
use Mix.Config

if Mix.env == :dev do
  config :mix_test_watch,
    extra_extensions: [".rs"]
end
```

## Compatibility Notes

On Linux you may need to install `inotify-tools`.

## Desktop Notifications

You can enable desktop notifications with
[ex_unit_notifier](https://github.com/navinpeiris/ex_unit_notifier).

## Alternatives

[mix_test_interactive](https://github.com/influxdata/mix_test_interactive) is based on `mix test.watch` but adds an interactive mode that allows you to dynamically change which tests to run.

## Copyright and Licence

mix test.watch

Copyright © 2015 Louis Pilfold

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the "Software"),
to deal in the Software without restriction, including without limitation
the rights to use, copy, modify, merge, publish, distribute, sublicense,
and/or sell copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM,
DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE
OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
