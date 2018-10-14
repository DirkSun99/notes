## 父组件主动获取子组件的数据和方法

* 步骤
    - 调用子组件的时候定义一个ref `<v-child ref="child" />`
    - 在父组件里面通过如下语法调用
        + this.$refs.child.属性
        + this.$refs.child.方法

* 实例
    - 父组件 `Parent.vue`
    
        ```html
        <template>
            <div id='parent'>
                <h2>父组件</h2>
                <button @click="getChildData()">获取子组件的数据和方法</button>

                <hr>
                <v-child ref="child" />
            </div>
        </template>

        <script>
            import Child from './Child.vue'

            export default {
                name: 'parent',
                data() {
                    return {
                        msg: '父组件msg',
                        title: '父组件title'
                    }
                }，
                methods: {
                    run(val) {
                        alert(`我是父组件run方法${val}`)
                    },

                    getChildData() {
                        // 获取子组件的数据
                        alert(this.$refs.child.msg)

                        // 执行子组件的方法
                        this.$refs.child.run()
                    }
                },
                components: {
                    'v-child': Child
                }
            }
        </script>
        ```

    - 子组件 `Child.vue`

        ```html
        <template>
            <div id='child'>
                <h3>子组件</h3>
            </div>
        <template>

        <script>
            export default {
                name: 'child',
                data() {
                    return {
                        msg: '子组件msg'
                    }
                },
                methods: {
                    run() {
                        alert('子组件的run方法')
                    }
                }
            }
        </script>
        ```