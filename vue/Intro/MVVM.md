## MVVM

Vue是一个MVVM的框架。

M： Model

V: View

MVVM: Model改变会影响视图View，View视图改变反过来影响Model

双向数据绑定必须在表单里面使用。

```html
<div id='app'>
	{{msg}}

	<input type="text" v-model="msg">

	<button v-on:click="getMsg()">获取表单里面的数据get</button>
	<button v-on:click="setMsg()">设置表单里面的数据set</button>

</div>
```

```javascript
export default {
	name: 'app',
	data() {
		return {
			msg: '你好，Vue'
		}
	},
	// 存放方法的地方
	methods: {
		getMsg() {
			// 方法里面获取data里面的数据
			alert(this.msg)
		},
		setMsg() {
			this.msg = "我是改变后的数据"
		}
	}
}
```