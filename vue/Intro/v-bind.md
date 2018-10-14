## v-bind绑定

* **绑定属性**

	```html
	<div id='app'>
		<div v-bind:title="title">鼠标瞄上去看一下</div>
		<div :title="title">绑定属性的简写形式</div>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				title: '我是一个title'
			}
		}
	}
	```

* **绑定Class**

	```html
	<div id='app'>
		<!-- 根据flag值显示颜色 -->
		<div v-bind:class="{'red':flag}">我是一个div</div>
		
		<!-- 根据flag值显示颜色，flag值为true显示为红色，flag值为false显示为蓝色-->
		<div :class="{'red':flag, 'blue':!flag}">我是另一个div</div>

		<!-- 循环数据，第一个红色高亮，第二个蓝色高亮 -->
		<ul>
			<li v-for="(item,key) in list" :class="{'red':key===0, 'blue':key===1}">
				{{key}} ---> {{item}}
			</li>
		</ul>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				flag: false,
				list: ['111','222','333']
			}
		}
	}
	```

	```css
	.red {
		color: red;
	}

	.blue {
		color: blue;
	}
	```

* **绑定Style**
	```html
	<div id='app'>
		<!-- v-bind:style :style的使用 -->
		<div class="box" v-bind:style="{width: boxWidth+'px'}">
			我是一个DIV
		</div>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				boxWidth: 300
			}
		}
	}
	```

	```css
	.box {
		height: 100px;
		weight: 100px;
		background-color: red;
	}
	```