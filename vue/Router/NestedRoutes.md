## 嵌套路由 -- Nested Routes

* 步骤
    - 嵌套路由配置
        
        ```javascript
        {
            path: '/user', 
            component:User,
            // 子路由
            children: [
              {path: 'useradd', component: UserAdd},
              {path: 'userlist', component: UserList}
            ]
        },
        ```
    - 父路由里面配置子路由显示的地方

        ```html
        <div id='user'>
        <div class="user">
            <div class="left">
                <ul>
                    <li><router-link to="useradd">增加用户</router-link></li>
                    <li><router-link to="userlist">用户列表</router-link></li>
                </ul>
            </div>
            <div class="right">
                <!-- 显示子路由 -->
                <router-view />
            </div>
        </div>
    </div>
    ```

* 示例
    - `App.vue`

    ```html
    <template>
        <div id='app'>
            <router-link to='/user/userlist'>用户</router-link>

            <hr>

            <router-view />
        </div>
    </template>
    ```
    
    - `main.js`

    ```javascript
    import Vue from 'vue'
    import App from './App.vue'

    import VueRouter from 'vue-router'
    Vue.use(VueRouter)

    import User from './components/User'
    import UserAdd from './components/User/UserAdd'
    import UserList from './components/User/UserList'

    const routes = [
        {
            path: '/user',
            component: User,
            children: [
                {path: 'useradd', component: UserAdd},
                {path: 'userlist', component: UserList}
            ]
        }
    ]

    const router = new VueRouter({
        routes
    })

    new Vue({
        el: '#app',
        router,
        render: h => h(App)
    })
    ```

    - `User.vue`

    ```html
    <template>
        <div class='user'>
            <div class='left'>
                <ul>
                    <li><router-link to='useradd'>用户增加</router-link></li>
                    <li><router-link to='userlist'>用户列表</router-link></li>
                </ul>
            </div>
            <div class='right'>
                <router-view />
            </div>
        </div>
    </template>

    <style lang="scss" scoped>
        .user {
            display: flex;

            .left {
                width； 200px;

                min-height: 400px;

                border-right: 1px solid #eee;

                li {
                    line-height: 2;
                }
            }

            .right {
                flex: 1;
            }
        }
    </style>
    ```

    - `UserAdd.vue`

    ```html
    <template>
        <div id="adduser">增加用户</div>
    </template>
    ```

    - `UserList.vue`

    ```html
    <template>
        <div id='listuser'>用户列表</div>
    </template>
    ```