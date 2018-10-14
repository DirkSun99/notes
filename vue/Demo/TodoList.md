## TodoList 待办事项

开发一个[待办事项](http://www.todolist.cn/)的功能，并将数据保存到浏览器缓存中。用到的技术点有：双向数据绑定、封装Storage模块持久化数据。

* 封装Storage模块持久化数据

```javascript
// storage.js
/**
+ 封装操作localStorage本地存储的方法
+ 模块化的文件
*/
var storage = {
    /**
    +   保存数据到浏览器缓存
    +   key: 待保存的数据key值
    +   value: 待保存的数据
    */
    set(key, value) {
        localStorage.setItem(key, JSON.stringify(value))
    },
    
    /**
    + 根据key值获取浏览器缓存数据
    + key: 待获取的数据key值
    + @return 返回从浏览器缓存中查找到数据
    */
    get(key) {
        return JSON.parse(localStorage.getItem(key))
    },

    /**
    + 移除浏览器缓存数据
    + key: 待移除的数据key值
    */
    remove(key) {
        localStorage.removeItem(key)
    }
}

export default storage
```

* 开发TodoList

```html
<template>
    <div id='app'>
        <input type="text" v-model="todo" @keydown="doAdd($event)" />
        <hr>

        <h2>进行中</h2>
        <ul>
            <li v-for="(item,key) in list" v-if="!item.checked">
                <input type="checkbox" v-model="item.checked" @change="saveList()" /> -- <button @click="removeData(key)">删除</button>
            </li>
        </ul>

        <br>

        <h2>已完成</h2>
        <ul>
            <li v-for="(item,key) in list" v-if="item.checked">
                <input type="checkbox" v-model="item.checked" @change="saveList()" /> -- <button @click="removeData(key)">删除</button>
            </li>
        </ul>
    </div>
</template>

<script>
    // 引入storage.js
    import storage from './model/storage.js'

    export default {
        name: 'app',
        data() {
            // 输入框中的值，默认为空
            todo: '',
            /**
            * 待办及已办事项集合，默认为空
            * 数据格式为
            * [
            *  {title: '录制NodeJS',checked: true},
            *  {title: '录制Ionic', checked: false}
            * ]
            */
            list:[]
        },
        methods: {
            /**
            * 添加待办事项
            */
            doAdd(e) {
                // 按回车保存
                if (e.keyCode === 13 && this.todo != '') {
                    // 1. 保存数据到list中
                    this.list.push({
                        title: this.todo,
                        checked: false
                    })

                    // 2. 保存浏览器缓存
                    storage.set('list',JSON.stringify(this.list))

                    // 3. 清空输入框
                    this.todo = ''
                }
            },

            /**
            * 删除待办或已办事项
            */
            removeData(key) {
                // 1. 从list中删除
                this.list.splice(key, 1)

                // 2. 更新浏览器缓存
                storage.set('list',JSON.stringify(this.list))
            },

            /**
            * 保存数据到浏览器缓存
            */
            saveList() {
                storage.set('list',JSON.stringify(this.list))
            }
        },

        /**
        * 生命周期函数
        * vue页面刷新会触发此方法
        */
        mounted() {
            // 从浏览器缓存中获取待办或已办事项集合
            let list = JSON.parse(storage.get('list'))
            
            // 判断非空
            if (list) {
                this.list = list
            }
        }
    }
</script>
```