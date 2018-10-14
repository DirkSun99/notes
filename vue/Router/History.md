## HTML5 History 模式

> https://router.vuejs.org/zh/guide/essentials/history-mode.html

`vue-router` 默认 hash 模式 --- 使用 URL 的 hash 来模拟一个完整的 URL, 当 URL 改变时，页面不会重新加载。

如果不想使用 hash，可以用路由的 **history模式**,这种模式充分利用  `history.pushState` API来完成 URL 跳转而无须重新加载页面。

```javascript
const router = new VueRouter({
    mode: 'history',
    routes: [...]
})
```

当使用 history 模式时， URL 就像正常的url，例如：https://yoursite.com/user/id。

不过在这种模式下，需要后台配置支持。因为应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问URL时就会返回404。

需要在服务器端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，刚应该返回同一个 `index.html`页面，这个页面就是 app 依赖的页面。