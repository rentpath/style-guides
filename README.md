# Style Guides
A collection of style guides used at RentPath.

# Editor Setup

## Atom

1. `apm install linter-rubocop`
2. Configure linter-rubocop by editing ~/.atom/config.cson (choose Open Your Config in Atom menu)
```shell
'linter-rubocop':
      'executablePath': null # rubocop path.
```
  - Run `which rubocop` to find the path, if you using rbenv run `rbenv which rubocop`

**Note**: This plugin finds the nearest .rubocop.yml file and uses the --config command line argument to use that file, so you may not use the --config argument in the linter settings.

(*Source Credit*: [https://atom.io/packages/linter-rubocop](https://atom.io/packages/linter-rubocop))
