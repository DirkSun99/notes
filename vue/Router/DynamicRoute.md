## Vue动态路由 (url/:param)

1. 配置动态路由
    
    ```javascript
    routes: [
        // 动态路径参数，以冒号开头
        {path: '/user/:id', component: User}
    ]
    ```

2. 使用 `<router-link>` 绑定参数

    ```html
    <template>
        <div>
            <ul>
                <li v-for="item in list">
                    <router-link :to="'/user/'+item.id">{{item.name}}</router-link>
                </li>
            </ul>
        </div>
    </template>

    <script>
        export default {
            data() {
                return {
                    list: [
                        {'id': 1, 'name':'Dirk'},
                        {'id': 2, 'name': 'Nash'}
                    ]
                }
            }       
        }
    </script>
    ```

3. 在对应页面使用 `this.$route.params` 获取动态路由的值
    
    ```javascript
    <script>
        export default {
            mounted() {
                // 获取动态路由传值
                console.log(this.$route.params)
            }
        }
    </script>
    ```
