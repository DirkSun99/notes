## App.vue

```html
<template>
  <!-- vue的模板里面，所有内容要被一个根节点包含起来 -->
  <div id="app">
    <header class="header">
      <router-link to='/home'>首页</router-link>
      <router-link to='/news'>新闻</router-link>
    </header>

    <hr>
    
    <router-view />
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      msg: 'Hello Vue'
    }
  }
}
</script>

<style lang="scss">

.header {
  height: 4.4rem;

  background: #000;

  color: #fff;

  line-height: 4.4rem;

  text-align: center;

  a {
    color: #fff;

    padding: 0 2rem;
  }
}
</style>
```