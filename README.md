#    Get start cypress

##    Step1: environment setting

於專案底下安裝cypress
```
    yarn add cypress --dev
```

如有需要用TS寫測試，需於專案底下安裝cypress webpack-preprocessor
```
    yarn add @cypress/webpack-preprocessor --dev
```

於package.json新增啟動指令
```js
    {
        "test": "cypress open"
    }
```
設定cypres.json
```js
    {
        "viewportWidth": 1280,  //測試預覽寬度
        "viewportHeight": 660,    //測試預覽高度
        "baseUrl": "http://localhost:9000", //需測試網址
        "supportFile": "./cypress/support/index.ts", //用TS寫要重新指向TS檔案路徑 
        "pluginsFile": "./cypress/plugins/index.ts", //用TS寫要重新指向TS檔案路徑 
        "watchForFileChanges": false // 開關是否存檔後自動run test
    }
```
預設資料夾結構
```
/cypress
    /fixtures
        - 測試用靜態資料放置區
    /integration
        - 測試程式碼集中管理區
        - yourTest.spec.js
    /plugins
        - 在每個測試 sepc 執行前自動將 plugins/index 程式碼引用進來
        - index.js
    /support
        - 在每個測試 sepc 執行前自動將 support/index 引用並執行
        - commands.js
        - index.js
```
TS版資料夾結構與設定
```js
/cypress
    /fixtures
        - 測試用靜態資料放置區
    /integration
        - 測試程式碼集中管理區
        - yourTest.spec.ts
    /plugins
        - 在每個測試 sepc 執行前自動將 plugins/index 程式碼引用進來
        - index.ts
            新增以下程式碼，使用cy方法才不會undefined
                /// <reference types="cypress" />
            新增以下程式碼，才不會因ts無法編譯
                const webpack = require('@cypress/webpack-preprocessor');
                module.exports = (on, config) => {
                    // `on` is used to hook into various events Cypress emits
                    // `config` is the resolved Cypress config

                    on('file:preprocessor', webpack());
                };
            啟動預設會引用index.js，但因改成ts檔案，所以要在cypress.pluginsFile value
            {
                "pluginsFile": "./cypress/plugins/index.ts",
            }
    /support
        - 在每個測試 sepc 執行前自動將 support/index 引用並執行
        - commands.ts
        - index.d.ts 
            所有在command新增的function要在此declare，否則用 cy. 會找不到自訂義的function
        - index.ts 
            啟動預設會引用index.js，但因改成ts檔案，所以要在cypress.json設定supportFile value
            {
                "supportFile": "./cypress/support/index.ts",
            }
            並且將裡面 import './commands.ts'
```


啟動cypress
```
    yarn run test
```
