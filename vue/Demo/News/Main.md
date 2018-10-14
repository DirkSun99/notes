## main.js

```javascript
import Vue from 'vue'
import App from './App.vue'

// 引入公共的scss
// 注意：创建项目的时候必须用scss
import './assets/css/basic.scss'

import VueResource from 'vue-resource'
import VueRouter from 'vue-router'

Vue.use(VueResource)
Vue.use(VueRouter)

// 引入组件
import Home from './components/Home'
import News from './components/News'
import Content from './components/Content'

// 配置路由
const routes = [
  {path: '/home', component: Home},
  {path: '/news', component: News},

  // 动态路由
  {path: '/content/:aid',component: Content},

  // 默认跳转路由
  {path: '*', redirect: '/home'}
]

// 实例化VueRouter
const router = new VueRouter({
  // 缩写，相当于 routes: routes
  routes
})

// 挂载路由

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})

// 把<router-view /> 放在 App.vue中
```