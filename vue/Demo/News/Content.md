## Content.vue(新闻详情组件)

```html
<template>
    <div id="content">
        <h2>{{list.title}}</h2>

        <div v-html="list.content"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            msg: '数据',
            list: []
        }
    },
    mounted() {
        // 获取动态路由传值
        // console.log(this.$route.params)
        let aid = this.$route.params.aid

        // 调用请求的方法
        this.requestData(aid)
    },
    methods: {
        requestData(aid) {

            let api = 'http://www.phonegap100.com/appapi.php?a=getPortalArticle&aid='+aid
        
            // get请求如果跨域的话，后台 php java 里面允许跨域请求

            this.$http.get(api).then((response) => {
                // console.log(response)
                this.list = response.body.result[0]
            },(err) => {
                console.log(error)
            })
        }
    }
}
</script>

<style lang="scss">
    #content {
        padding: 1rem;
        
        line-height: 2;

        img {
            max-width: 100%;
        }
    }
</style>
```