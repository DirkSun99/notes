## Vue 路由配置

1. 安装 `npm install vue-router --save`
2. 引入并 Vue.use(VueRouter) `main.js`
    ```javascript
    // 引入
    import VueRouter from 'vue-router'

    // 使用
    Vue.use(VueRouter)
    ```
3. 配置路由

    3.1 创建组件 引入组件

    3.2 定义路由

        const routes = [
            {path: '/foo', component: Foo},
            {path: '/bar', component: Bar},

            // 默认跳转路由
            {path: '*', redirect: '/foo'}
        ]

    3.3 实例化VueRouter

        const router = new VueRouter({
            // 缩写，相当于 routes: routes
            routes
        })

    3.4 挂载

        new Vue({
            el: '#app',
            router,
            render: h => h(App)
        })

    3.5 根组件(App.vue)的模板里面加上 `<router-view />` 以及路由链接

        ```html
        <template>
            <div id='app'>
                <!-- 添加路由链接 -->
                <router-link to='/foo'>Go To Foo</router-link>
                <router-link to='/bar'>Go To Bar</router-link>

                <hr>

                <router-view />
            </div>
        </template>
        ```