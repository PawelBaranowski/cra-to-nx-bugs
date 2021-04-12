# Environment
| Feature | Version |
|-|-|
| Operating system | Windows 10 19041.264 |
| Shell | PowerShell Core (pwsh) 7.1.1 |
| Node JS | v14.16.0 |
| npm | 6.14.11 |
| yarn | 1.22.10 |

# Steps to reproduce

> shell commands are included in commit messages

### 1. Initialize project using Create React App
```shell
$ npx create-react-app cra-to-nx-bugs --template typescript
$ yarn start
```
:white_check_mark: it works

### 2. Convert project to nx workspace
```shell
$ npx cra-to-nx
$ yarn start
```
:x: it doesn't work:
```shell
yarn run v1.22.10
$ nx serve

> nx run cra-to-nx-bugs:serve
C:\Repos\cra-to-nx-bugs\node_modules\.bin\react-app-rewired:2
basedir=$(dirname "$(echo "$0" | sed -e 's,\\,/,g')")
          ^^^^^^^

SyntaxError: missing ) after argument list
    at wrapSafe (internal/modules/cjs/loader.js:979:16)
    at Module._compile (internal/modules/cjs/loader.js:1027:27)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:72:12)
    at internal/main/run_main_module.js:17:47
ERROR: Something went wrong in @nrwl/run-commands - Command failed: node ../../node_modules/.bin/react-app-rewired start

>  NX   CLOUD  See run details at https://nx.app/runs/Wwdx1bn5Sxj

error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```