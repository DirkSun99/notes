## Get传值 (url?key=value)

1. 配置路由
    
    ```javascript
    routes: [
        {path: '/product', component: Product}
    ]
    ```

2. 使用 `<router-link>` 绑定参数

    ```html
    <template>
        <div>
            <ul>
                <li v-for="item in list">
                    <router-link :to="'/product/?id='+item.id">{{item.name}}</router-link>
                </li>
            </ul>
        </div>
    </template>

    <script>
        export default {
            data() {
                return {
                    list: [
                        {'id': 1, 'name':'Apple'},
                        {'id': 2, 'name': 'Xiaomi'}
                    ]
                }
            }       
        }
    </script>
    ```

3. 在对应页面使用 `this.$route.query` 获取Get参数的值
    
    ```javascript
    <script>
        export default {
            mounted() {
                // Get传值
                console.log(this.$route.query)
            }
        }
    </script>
    ```