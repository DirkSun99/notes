## 数据绑定

Vue.js的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进DOM系统。

* **绑定基础数据**

	```html
	<div id='app'>
		{{msg}}
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				msg: 'Hello Vue'
			}
		}
	}
	```

* **绑定对象**

	```html
	<div id='app'>
		{{obj.name}}
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				obj: {
		        	name: '张三'
		      	}
			}
		}
	}
	```