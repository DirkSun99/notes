## 循环

* **循环数组**

	```html
	<div id='app'>
		<ul>
	      <li v-for="item in list">
	        {{item}}
	      </li>
	    </ul>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				list: ['111','222','333']
			}
		}
	}
	```

* **循环数组对象**

	```html
	<div id='app'>
		<ul>
	      <li v-for="item in list">
	        {{item.title}}
	      </li>
	    </ul>
	</div> 
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				list: [
					{"title":'111'},
			        {"title":'222'},
			        {"title":'333'},
			        {"title":'444'}
				]
			}
		}
	}
	```

* **数组嵌套循环**

	```html
	<div id='app'>
		<ul>
	      <li v-for="item in list">
	        {{item.cate}}
	        <ol>
	          <li v-for="news in item.list">
	            {{news.title}}
	          </li>
	        </ol>
	      </li>
	    </ul>
	</div>
	```

	```javascript
	export default {
		name: 'app',
		data() {
			return {
				list: [
		        { 
		          'cate':'国内新闻',
		          'list':[
		            {'title':'国内新闻111'},
		            {'title':'国内新闻222'},
		          ]
		        },
		        { 
		          'cate':'国际新闻',
		          'list':[
		            {'title':'国际新闻111'},
		            {'title':'国际新闻222'},
		          ]
		        }
		      ]
			}
		}
	}
	```