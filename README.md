# vscode debugger 的一些配置

## 启动

```shell
# 1. 使用chrome打开
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --user-data-dir=/Users/eternallycyf/chrome-debugger
# 2.使用chrome canary打开
/Applications/Google\ Chrome\ Canary.app/Contents/MacOS/Google\ Chrome\ Canary --remote-debugging-port=9222
# ./.vscode/launch.jsn
```

### 通用

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "针对 localhost 启动 Chrome",
      "type": "chrome",
      "request": "attach",
      "port": 9222,
      "userDataDir": false,
      "runtimeExecutable": "canary",
      "sourceMaps": true,
      "sourceMapPathOverrides": {
        "meteor://💻app/*": "${workspaceFolder}/*",
        "webpack:///./~/*": "${workspaceFolder}/node_modules/*",
        "webpack://?:*/*": "${workspaceFolder}/*"
      },
      "runtimeArgs": [
        "--auto-open-devtools-for-tabs",
        "--user-data-dir=${workspaceFolder}/.vscode/chrome"
      ],
      "skipFiles": [
        "${workspaceFolder}/node_modules/**/*.js",
        "<node_internals>/**/*.js"
      ]
    }
  ]
}
```

### nest

```json
{
  "name": "Launch via NPM",
  "request": "launch",
  "runtimeArgs": ["run-script", "start"],
  "console": "integratedTerminal",
  "runtimeExecutable": "pnpm",
  "skipFiles": ["<node_internals>/**"],
  "type": "node"
}
```

### umi

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Chrome against localhost",
      "url": "http://localhost:8000",
      "webRoot": "${workspaceFolder}",
      "skipFiles": [
        "${workspaceFolder}/<node_internals>/**",
        "${workspaceFolder}/node_modules/**",
        "${workspaceFolder}/webpack/**",
        "${workspaceFolder}/.umi/**",
        "${workspaceFolder}/umi\\.js$",
        "${workspaceFolder}/@@/devScripts.js$",
        "${workspaceFolder}/^webpack://.*/react refresh$",
        "${workspaceFolder}/react_devtools_backend.js\\.js$"
      ],
      "smartStep": true,
      "sourceMaps": true
    }
  ]
}
```

### vue

```js
// vue.config.js
const { defineConfig } = require('@vue/cli-service');
module.exports = defineConfig({
  configureWebpack: {
    output: {
      devtoolModuleFilenameTemplate: '文件地址://[resource-path]',
    },
  },
});
```

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "pwa-chrome",
      "request": "launch",
      "name": "Launch Chrome against localhost",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}",
      "sourceMapPathOverrides": {
        "文件地址://*": "*"
      }
    }
  ]
}
```
