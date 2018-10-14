## v-text绑定数据

```html
<div id='app'>
	<div v-text="msg" />
	<!-- 等价于下面的写法 -->
	<div>{{msg}}</div>
</div>
```

```javascript
export default {
	name: 'app',
	data() {
		return {
			msg: 'Hello Vue!'
		}
	}
}
```