## v-if 条件渲染

```html
<div id='app'>
    <h2 v-if="ok">这是一个OK</h2>
    <h2 v-if="!ok">这是一个NO</h2>
</div>
```

```javascript
export default {
    name: 'app',
    data() {
        ok: true
    }
}
```