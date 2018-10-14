## Element UI

* 全部引入 步骤
    - 找官网 ： http://element-cn.eleme.io/#/zh-CN/component/quickstart
    - 安装 `npm install element-ui -S`
    - 引入 element UI 的 css 和 插件
        
        ```javascript
        import ElementUI from 'element-ui'
        import 'element-ui/lib/theme-chalk/index.css'
        Vue.use(ElementUI)
        ```
    - webpack.config.js文件中配置file_loader

        ```javascript
        {
            test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
            loader: 'file-loader'
        }
        ```
    - 看文档直接使用

* 按需引入(方法一，**推荐**) 步骤
    - 安装 `npm install babel-plugin-component -D`
    - 找到.babelrc配置文件把配置文件修改为
        
        ```json
        {
          "presets": [["env", { "modules": false }]],
          "plugins": [
            [
              "component",
              {
                "libraryName": "element-ui",
                "styleLibraryName": "theme-chalk"
              }
            ]
          ]
        }
        ```
    - 引入对应的组件

        ```javascript
        import {Button, Select} from 'element-ui'

        Vue.use(Button)
        Vue.use(Select)
        ```

    * 按需要引入(方法二) 步骤
        - 引入对应的组件
            
            ```javascript
            import {Button, Select} from 'element-ui'

            Vue.use(Button)
            Vue.use(Select)
            ```
        - 引入对应的css

            ```javascript
            import 'element-ui/lib/theme-chalk/index.css'
            ```
        - webpack.config.js文件中配置file_loader
            ```javascript
            {
                test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
                loader: 'file-loader'
            }
            ```

