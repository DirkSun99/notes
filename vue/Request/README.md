## 请求数据

* `vue-resource`：官方提供的一个vue插件
    - 安装 `npm install vue-resource --save`
    - 在main.js中引入 `import VueResource from 'vue-resource'`
    - 在main.js中使用 `Vue.use(VueResource)`
    - 在组件中直接使用 `this.$http.get(地址).then(function() {})`
    - 示例
    
```html
<template>
    <div>
        <button>vue-resource请求数据</button>

        <hr>
        <br>
        <ul>
            <li v-for="item in list">
                {{item.title}}
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                list:[]
            }
        },
        methods: {
            // 使用vue-resource请求数据
            getData() {
                let api = 'http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1'

                this.$http.get(api).then((response) => {
                    console.log(response)

                    // 注意this指向
                    this.list = response.body.result
                },(err => {
                    console.log(err)
                }))
            }
        },
        // 生命周期函数
        mounted() {
            // vue-resource请求数据
            this.getData()
        }
    }
</script>
```

* `axios`
    - 安装 `npm install axios --save`
    - 哪里使用哪里引用 `import axios from 'axios'`
    - 示例

```html
<template>
    <div>
        <button @click="getDataAxios()">axios请求数据</button>
        <hr>
        <br>

        <ul>
            <li v-for="item in list">
                {{item.title}}
            </li>
        </ul>
    </div>
</template>

<script>
    <!-- 引入axios -->
    import axios from 'axios'

    export default {
        data() {
            return {
                list: []
            }
        },
        methods: {
            // 使用axios请求数据
            getDataAxios() {
                let api = 'http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1'

                axios.get(api).then((response) => {
                    conosle.log(response)
                    this.list = response.data.result
                }).catch((error) => {
                    console.log(error)
                })
            }
        },
        mounted() {
            // axios请求数据
            this.getDataAxios()
        }
    }
</script>
```