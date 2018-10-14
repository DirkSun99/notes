## 非父子组件传值

* 步骤
    - 新建一个js文件，引入vue，实例化vue，暴露这个实例
    - 在要广播的地方引入定义的实例
    - 通过 `VueEvent.$emit('名称','数据')` 发送广播
    - 在接收数据的地方通过 `VueEvent.$on('名称',(data) => {})` 接收广播数据

* 示例
    1. 新建js文件 `VueEvent.js`
    
    ```javascript
    // 引入 Vue
    import Vue from 'vue'

    // 实例化Vue
    var VueEvent = new Vue()
    
    // 暴露实例
    export default VueEvent
    ```

    2. 在广播的地方引入vue实例及发送广播数据 `Component1.vue`
    
    ```html
    <template>
        <div id='com1'>
            组件1

            <br>

            <button @click="emitCom2()">给com2组件广播数据</button>
        </div>
    </template>

    <script>
        // 引入vue实例
        import VueEvent from '../model/VueEvent.js'

        export default {
            name: 'com1',
            data() {
                return {
                    msg: 'com1组件'
                }
            },
            methods: {
                emitCom2() {
                    // 发送广播数据
                    VueEvent.$emit('to-com2',this.msg)
                }
            }
        }
    </script>
    ```

    3. 在接收数据的地方引入vue实例及接收广播数据 `Component2`
    
    ```html
    <template>
        <div id='com2'>
            组件2
        </div>
    </template>

    <script>
        // 引入vue实例
        import VueEvent from '../model/VueEvent.js'

        export default {
            name: 'com2',
            data() {
                return {
                    msg: 'com2组件'
                }
            },
            mounted() {
                VueEvent.$on('to-com2',(data) => {
                    console.log(data)
                })
            }
        }
    </script>
    ```

    4. 在 `App.vue` 中引入两个组件

    ```html
    <template>
        <div id='app'>
            <!-- 引入组件1 -->
            <v-com1 />
            <br>
            <hr>

            <!-- 引入组件2 -->
            <v-com2 />
        </div>
    </template>

    <script>
        // 引入子组件
        import Component1 from './components/Component1.vue'
        import Component2 from './components/Component2.vue'
    
        export default {
            name: 'app',
            data {
                return {
                    msg: 'Hello Vue'
                }
            },
            components: {
                'v-com1': Component1,
                'v-com2': Component2
            }
        }
    </script>
    ```