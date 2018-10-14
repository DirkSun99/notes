## News.vue(新闻组件)

```html
<template>
    <div id='news'>
        新闻页面

        <br>

        <ul class="list">
            <li v-for="(item,key) in list">
                <router-link :to="'/content/'+item.aid">{{key}} --- {{item.title}}</router-link>
            </li>
        </ul>
    </div>
</template>

<script>

export default {
    data() {
        return {
            msg: '我是一个新闻组件',
            list: []
        }
    },

    methods: {
        requestData() {
            // jsonp请求，需要后台api接口支持
            let api = 'http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1'
            
            this.$http.jsonp(api).then((response) => {
                // 注意： 用到this，注意this指向
                this.list = response.body.result
            },(error) => {
                console.log(error)
            })
        }
    },

    mounted() {
        this.requestData()
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

            a {
                color: #666;
            }
        }
    }
</style>
```