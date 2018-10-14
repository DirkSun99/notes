## 子组件主动获取父组件的数据和方法

* 步骤
    - 调用子组件的时候定义一个ref `<v-child ref="child" />`
    - 在子组件里通过如下语法调用
        + this.$parent.数据
        + this.$parent.方法

* 实例
    - 父组件 `Parent.vue`
        
        ```html
        <template>
            <div id='parent'>
                <h2>父组件</h2>

                <hr>

                <v-child ref="child" />
            </div>
        </template>

        <script>
            // 引入子组件
            import Child from './Child.vue'

            export default {
                name: 'parent',
                data() {
                    return {
                        msg: '父组件msg',
                        title: '父组件title'
                    }
                },

                methods: {
                    run() {
                        alert(`父组件run方法`)
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

                <button @click="getParentData()">获取父组件数据及方法</button>
            </div>
        </template>

        <script>
            name: 'child',
            data() {
                return {
                    msg: '子组件msg'
                }
            },

            methods: {
                getParentData() {
                    // 子组件主动获取父组件数据
                    alert(this.$parent.msg)

                    // 子组件主动执行父组件方法
                    this.$parent.run()
                }
            }
        </script>
        ```
