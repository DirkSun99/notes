## Vuex

Vuex 是一个专为 vue.js 应用程序开发的状态管理模式
    
    1. vuex 解决了组件之间同一状态的共享问题（解决了不同组件之间的数据共享）
    2. 组件里面数据的持久化

#### Vuex 的定义

---

1. 在src目录下新建一个vuex的文件夹
2. 在 vuex 文件夹里面新建一个store.js
3. 安装vuex `npm install vuex --save`
4. 在stroe.js中引入vuex并且使用
    ```javascript
    import Vue from 'vue'
    import Vuex from 'vuex'

    Vue.use(Vuex)
    ```
5. 定义数据(state)

    ```javascript
    /* state在vuex中用于存储数据 */
    const state = {
        count: 1
    }
    ```
6. 定义方法(mutations)
    
    ```javascript
    /* mutations里面放的是方法，主要用于改变state里面的数据 */
    const mutations = {
        incCount() {
            ++state.count
        }
    }
    ```
7. getters

    ```javascript
    /* 有点类似计算属性: 改变state里面的数据时会触发 getters 里面的方法 获取新的值（基本不用） */
    const getters = {
        computedCount: (state) => {
            return state.count * 2
        }
    }
    ```
8. actions

    ```javascript
    /**
     * Action 类似于 mutation,不同在于
     *  1 Action 提交的是 mutation，而不是直接变更状态
     *  2 Action 可以包含任意异步操作
     */
    const actions = {
        incMutationsCount(context) {
            // context.commit 提交一个mutation
            context.commit('incCount') /* 执行 mutations 里面的 incCount 方法 改变 state 里面的数据 */
        }
    }
    ```
9. 暴露

    ```javascript
    // vuex 实例化 Vuex.Store
    const store = new Vuex.Store({
        state,
        mutations,
        getters,
        actions
    })

    export default store`
    ```

#### 在组件中使用Vuex

---

1. 引入store  建议store的名字不要变
    
    ```javascript
    import store from '../vuex/store.js'
    ```
2. 注册

    ```javascript
    export default {
        data() {

        },
        // 注册
        store,
        methods: {

        }
    }
    ```
3. 获取state里面的数据
    
    ```javascript
    this.$store.state.数据
    ```
4. 触发 mutations 改变 state 里面的数据

    ```javascript
    export default {
        methods: {
            incCount() {
                this.$store.commit('incCount')
            }
        }
    }
    ```
5. 触发 actions 里面的方法
    
    ```javascript
    export default {
        methods: {
            incCount() {
                this.$store.dispatch('incMutationsCount')
            }
        }
    }
    ```
6. 获取getters里面方法返回的数据 

`{{this.$store.getters.computedCount}}`

#### 示例

---

动态获取新闻列表将结果保存在vuex中

* `store.js`

    ```javascript
    import Vue from 'vue'
    import Vuex from 'vuex'

    Vue.use(Vuex)

    // state存储新闻列表数据
    const state = {
        list: []
    }

    // 将新闻列表数据保存在state中
    const mutations = {
        addList(state, data) {
            state.list = data
        }
    }

    // vuex 实例化 Vuex.Store
    const store = new Vuex.Store({
        state,
        mutations
    })

    // 暴露
    export default store
    ```

* `News.vue`

    ```html
    <template>
        <div id='news'>
            <ul>
                <li v-for="item in list">
                    <span v-text="item.title"/>
                </li>
            </ul>
        </div>
    </template>

    <script>
        import store from '../vuex/store.js'

        export default {
            data() {
                list:[]
            },
            store,
            methods: {
                requestData() {
                    // jsonp请求，需要后台api接口支持
                    let api = 'http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1'
            
                    this.$http.jsonp(api).then((response) => {
                        // 注意： 用到this，注意this指向
                        this.list = response.body.result

                        // 数据放在store里面
                        this.$store.commit('addList',response.body.result)
                    },(error) => {
                        console.log(error)
                    })
                }
            },
            mounted() {
                // 判断store里面有没有数据
                let listData = this.$store.state.list

                if (listData.length > 0) {
                    this.list = listData
                } else {
                    this.requestData()
                }
            }
        }
    </script>
    ```
