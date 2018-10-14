## Mint UI

* 步骤
    - 找官网 ：http://mint-ui.github.io/#!/zh-cn
    - 安装 `npm install mint-ui -S`
    - 引入Mint UI 的 css 和插件
    
        ```javascript
        import Mint from 'mint-ui'
        Vue.use(Mint)

        import 'mint-ui/lib/style.css'
        ```
    - 看文档直接使用

* 注意事项
    - 在MintUI组件上执行事件的写法 `@click.native`
        ```html
        <mt-button @click.native='sheetVisible = true' size='large'>点击上拉 action sheet</mt-button>
        ```

* 示例
    - Mint Ui  infinite-scroll结合api接口实现真实上拉分页加载更多
    
    ```html
    <template>
        <div id='news'>
            新闻页面

            <br>

            <ul 
                v-infinite-scroll="loadMore"
                infinite-scroll-disabled="loading"
                infinite-scroll-distance="10"
                class="list"
            >
                <li v-for="item in list"><router-link :to="'/content/'+item.aid">{{item.title}}</router-link></li>
            </ul>

            <div>loading...</div>
        </div>
    </template>

    <script>

    export default {
        data() {
            return {
                msg: '我是一个新闻组件',
                list: [],
                page: 1,
                loading: false
            }
        },

        methods: {
            requestData() {
                this.loading=true /* 请求数据的开头 */
                // jsonp请求，需要后台api接口支持
                let api = `http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=${this.page}`
                
                this.$http.get(api).then((response) => {
                    // 注意： 用到this，注意this指向
                    // 拼接list
                    this.list = this.list.concat(response.body.result)

                    ++this.page // 每次请求完成page++
                    
                    // 判断最后一页是否有数据
                    if (response.body.result.length < 20 ) {
                        this.loading = true /* 终止请求 */
                    } else {
                        this.loading=false /* 继续请求 */
                    }
                },(error) => {
                    console.log(error)
                })
            },
            loadMore() {
                this.requestData()
            }
        },

        mounted() {
            // this.requestData()
        }
    }
    </script>

    <style lang="scss" scoped>
        .list {
            li {
                height: 3.4rem;

                line-height: 3.4rem;

                border-bottom: 1px solid #eee;

                font-size: 1.6rem;

                cursor: pointer;

                a {
                    color: #666;
                }
            }
        }
    </style>

    ```