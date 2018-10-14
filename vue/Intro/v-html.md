## v-html绑定HTML

```html
<div id='app'>
	<!-- 绑定HTML -->
	<div v-html="h" />
</div>
```

```javascript
export default {
	name: 'app',
	data() {
		return {
			h: '<h2>Hello Vue</h2>'
		}
	}
}
```