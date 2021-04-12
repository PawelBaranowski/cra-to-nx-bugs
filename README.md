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
```
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

### 3. Fix issues
First, calls to react-app-rewired binaries must be changed in `workspace.json`:
```diff
diff --git a/workspace.json b/workspace.json
index 21e1346..94384c5 100644
--- a/workspace.json
+++ b/workspace.json
@@ -32,7 +32,7 @@
             "{options.outputPath}"
           ],
           "options": {
-            "command": "node ../../node_modules/.bin/react-app-rewired build",
+            "command": "node ../../node_modules/react-app-rewired/bin/index.js build",
             "cwd": "apps/cra-to-nx-bugs",
             "outputPath": "dist/apps/cra-to-nx-bugs"
           }
@@ -41,7 +41,7 @@
           "executor": "@nrwl/workspace:run-commands",
           "outputs": [],
           "options": {
-            "command": "node ../../node_modules/.bin/react-app-rewired start",
+            "command": "node ../../node_modules/react-app-rewired/bin/index.js start",
             "cwd": "apps/cra-to-nx-bugs"
           }
         },
@@ -57,7 +57,7 @@
           "executor": "@nrwl/workspace:run-commands",
           "outputs": [],
           "options": {
-            "command": "node ../../node_modules/.bin/react-app-rewired test --watchAll=false",
+            "command": "node ../../node_modules/react-app-rewired/bin/index.js test --watchAll=false",
             "cwd": "apps/cra-to-nx-bugs"
           }
         }
```
Second, `.env` contains unnecessary double quotes.
```diff
diff --git a/.env b/.env
index 55996b3..6f809cc 100644
--- a/.env
+++ b/.env
@@ -1 +1 @@
-"SKIP_PREFLIGHT_CHECK=true" 
+SKIP_PREFLIGHT_CHECK=true
```
Now try to run it:
```shell
$ yarn start
```
:white_check_mark: it works again

Additionaly nx is making a change to tsconfig.json:
```
The following changes are being made to your tsconfig.json file:
  - compilerOptions.jsx must be react-jsx (to support the new JSX transform in React 17)
  - compilerOptions.paths must not be set (aliased imports are not supported)
```
