## 编程式导航 -- Programmatic Navigation

除了使用 `<router-link>` 创建`a`标签来定义导航链接，还可以借助router的实例方法，通过编写代码来实现。

### `router.push(location, onComplete?, onAbort?)`

---

注意：在Vue实例内部，可以通过`$router`访问路由实例。`this.$router.push`。

想要导航到不同的URL，则使用 `router.push` 方法。这个方法会向history栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的URL。

当点击`<router-link>`时，这个方法会在内部调用，所以说，点击`<router-link :to="...">`等同于调用`router.push(...)`

| 声明式 | 编程式 |
| ----- | ----- |
|`<router-link :to="...">` | `router.push(...)` |

该方法的参数可以是一个字符串路径，或者一个描述地址的对象。

```javascript
// 字符串
this.$router.push('home')

// 对象
this.$router.push({path: 'home'})

// 命名的路由
this.$router.push({ name: 'user', parmas: {userId: 123}})

// 带查询参数，变成 /register?plan=private
this.$router.push({path: 'register', query: {plan: 'private'}})
```

**注意:** 如果提供了 `path`， `params`会被忽略。需要提供路由的 `name` 或手写完整的带有参数的 `path`

```javascript
const userId = 123

this.$router.push({name: 'user', params: {userId}}) // -> /user/123

this.$router.push({path: `/user/${userId}`}) // -> /user/123

// 这里的params不生效
this.$router.push({path: '/user', params: {userId}}) // -> /user
```

同样的规则也适用于 `router-link` 组件的 `to` 属性。

在2.2.0+，可选的在`router.push` 或 `router.replace` 中提供的 `onComplete` 和 `onAbort` 回调作为第二个和第三个参数。这些回调将会在导航成功完成（在所有的异步钩子被解析之后）或终止（导航到相同的路由、或在当前导航完成之前导航到另一个不同的路由）的时候进行相应的调用。

**注意：**如果目的地和当前路由相同，只有参数发生了改变(比如从一个用户资料到另一个 `/users/` -> `/users/2`)，需要使用 `beforeRouteUpdate` 来响应这个变化（比如抓取用户信息）

### `router.replace(location, onComplete?, onAbort?)`

---

跟 `router.push` 很像，唯一的不同是它不会向history添加新记录，而是跟它的方法名一样 ----替换掉当前的history记录。

| 声明式 | 编程式 |
| ----- | ----- | 
| `<router-link :to="..." replace>` | `router.replace(...)`

### `router.go(n)`

---

这个方法的参数是一个整数，，意思是在history记录中向前或者后退多少步，类似`window.history.go(n)`

```javascript
// 在浏览器记录中前进一步，等同于 history.forward()
this.$router.go(1)

// 后退一步记录，等同于 history.back()
this.$router.go(-1)

// 前进 3 步记录
this.$router.go(3)

// 如果 history 记录不够用，那就默默地失败
this.$router.go(-100)
```

Vue Router的导航方法(`push`、`replace`、`go`)在各类路由模式(`history`、`hash`、`abstract`)下表现一致