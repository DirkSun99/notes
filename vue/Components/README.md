## 组件

* 编写组件

    编写Home.vue组件
    
    ```html
    <template>
        <div id='home'>
            <!-- 首页组件 -->
            <h2>这是一个首页组件</h2>
            <button @click="run()">执行run方法</button>
        </div>
    </template>

    <script>
        export default {
            data() {
                return {
                    msg: '我是一个首页组件'
                }
            },
            methods: {
                run() {
                    alert(this.msg)
                }
            }
        }
    </script>
    ```

* 引入组件

    在`APP.vue`中引用组件

    ```javascript
    <script>
        import Home from './components/Home.vue'
    </script>
    ```

* 挂载组件
    
    在`APP.vue`中挂载组件
    
    ```javascript
    <script>
        exports default {
            // 前面的名称不能和Html标签一致
            components: {
                'v-home': Home
            }
        }
    </script>
    ```

* 在模板中使用

    在`APP.vue`中使用

    ```html
    <template>
        <div id='app'>
            <!-- 使用组件 -->
            <v-home />
        </div>
    </template>
    ```

* 在组件中使用样式

    ```html
    <template>
        <div id='home'>
            <h2>这是一个首页组件</h2>
        </div>
    </template>
    
    <!-- 样式只在此组件中起作用 -->
    <!-- 方法一 -->
    <style lang="scss">
        #home {
            h2 {
                color: red;
            }
        }
    </style>

    <!-- 方法二： 使用 css 布局作用域 scoped-->
    <style lang="scss" scoped>
        h2 {
            color: red;
        }
    </style>
    ```