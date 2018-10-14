## Vue事件

* **事件执行方法1**

	```html
	<div id='app'>
		{{msg}}

		<button v-on:click="run()">执行方法的第一种写法</button>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				msg: 'Hello Vue'
			}
		},
		methods: {
			run: () => {
				alert('这是一个方法')
			}
		}
	}
	```

* **事件执行方法2**

	```html
	<div id='app'>
		<button @click="run()">执行方法的第二种写法</button>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		methods: {
			run() {
				alert('这是另一个方法')
			}
		}
	}
	```

* **获取data中的数据**

	```html
	<div id='app'>
		<button @click="getMsg()">获取data里面的msg</button>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				msg: 'Hello Vue'
			}
		},
		methods: {
			getMsg() {
				alert(this.msg)
			}
		}
	}
	```

* **改变data中的数据**

	```html
	<div id='app'>
		<button @click="setMsg()">改变data里面的msg</button>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				msg: 'Hello Vue'
			}
		},
		methods: {
			setMsg() {
				this.msg = '我是改变后的数据'
			}
		}
	}
	```

* **请求数据**

	```html
	<div id='app'>
		<button @click="requestData()">请求数据</button>
		<hr>
		<ul>
			<li v-for="(item,key) in list" key="">
				{{key}} --> {{item}}
			</li>
		</ul>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				list: []
			}
		}
		methods: {
			requestData() {
				for (let i=0; i<10; i++) {
					this.list.push(`我是第${i}条数据`)
				}
			}
		}
	}
	```
* **方法传值**
	
	```html
	<div id='app'>
		<button @click="deleteData('111')">方法传值</button>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		methods: {
			deleteData(val) {
				alert(val)
			}
		}
	}
	```

* **事件对象**

	```html
	<div id='app'>
		<button data-aid='123' @click="eventFn(event)">事件对象</button>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		eventFn(e) {
			console.log(e)

			// e.srcElement 获取Dom节点
			e.srcElement.style.background='red'

			// 获取自定义属性的值
			console.log(e.srcElement.dataset.aid)
		}
	}
	```