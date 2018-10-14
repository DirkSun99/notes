## 生命周期函数

生命周期函数：组件挂载、组件更新以及组件销毁的时候触发的一系列方法。

**生命周期图示**

![生命周期](./../pic/Lifecycle/lifecycle.png)

**生命周期示例**

Life.vue

```html
<template>
    <div id='life'>
        生命周期函数演示 -- {{msg}}

        <br>
        <button @click="setMsg()">执行方法改变msg</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                msg: '我是一个生命周期组件'
            }
        },
        methods: {
            setMsg() {
                this.msg = '我是改变后的数据'
            }
        },
        beforeCreate() {
            console.log('实例刚刚被创建1')
        },
        created() {
            console.log('实例已经创建完成2')
        },
        beforeMount() {
            console.log('模板编译之前3')
        },
        /**
        * 请求数据，操作Dom,放在这个里面
        */
        mounted() {
            console.log('模板编译完成4')
        },
        beforeUpdate() {
            console.log('数据更新之前')
        },
        updated() {
            console.log('数据更新完毕')
        },
        /**
        * 页面销毁的时候要保存一些数据，就可以监听这个销毁的生命周期函数
        */
        beforeDestroy() {
            console.log('实例销毁之前')
        },
        destroyed() {
            console.log('实例销毁完成')
        }
    }
</script>
```

Home.vue

```html
<template>
    <div id='home'>
        <!-- 生命周期组件 -->
        <v-life v-if="flag" />

        <br>
        <br>
        <button @click="flag=!flag">挂载以及卸载Life组件</button>
    </div>
</template>

<script>
    <!-- 引入生命周期组件 -->
    import Life from './Life.vue'
    
    export default {
        data() {
            flag: true
        },
        components； {
            'v-life': Life
        }
    }
</script>
```