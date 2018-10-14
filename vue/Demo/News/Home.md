## Home.vue(Home组件)

```html
<template>
    <!-- 所有的内容要被根节点包含起来 -->
    <div id='home'>
        首页面
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
    }
}
</script>

<!-- 方法二：使用 css 局布作用域 scoped -->
<style lang="scss" scoped>
</style>
```