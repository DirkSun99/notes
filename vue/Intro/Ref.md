## Ref获取Dom节点

`ref`被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的`$refs`对象上。如果在普通的DOM元素上使用，引用指向的就是DOM元素；如果用在子组件上，引用就指向组件实例。

```html
<div id='app'>
	<input type="text" ref="userInfo" />

	<div ref="box">我是一个box</div>

	<button v-on:click="getInputValue()">获取表单里面的数据</button>
</div>
```

```javascript
export default {
	name: 'app',
	methods: {
		getInputValue() {
			// 获取dom节点
			console.log(this.$refs.userInfo)

			// 打印DOM元素数据
			alert(this.$refs.userInfo.value)

			// 设置box背景色
			this.$refs.box.style.background='red'
		}
	}
}
```