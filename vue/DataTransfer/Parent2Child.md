## 父组件传值给子组件

父组件可以给子组件传值、传方法以及整个实例

* 步骤
    - 父组件调用子组件的时候，绑定动态属性 `<v-child :title="title" />`
    - 在子组件里面通过 props 接收父组件传过来的数据
        + 方式一：props: ['title']
        + 方式二：props: {'title':String}，带检验功能
    - 直接在子组件中使用

* 实例
    - 父组件 `Parent.vue`

    ```html
    <template>
        <div id='parent'>
            <h2>父组件</h2>

            <!-- 子组件 -->
            <!-- 传值、 传方法、 传整个实例 给子组件-->
            <v-child :title="title" :homeMsg="msg" :run="run" :parent="this"/>
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
            },
            methods: {
                run(val) {
                    alert(`父组件run方法${val}`)
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
            <!-- 父组件给子组件传值 -->
            <h3>子组件 --- {{title}} --- {{homeMsg}}</h3>

            <button @click="run('123')">执行父组件的方法</button>

            <br>
            <br>
            <br>

            <button @click="getParent()">获取父组件的数据和方法</button>
        </div>
    </template>

    <script>
        export default {
            name: 'child',
            data() {
                return {
                    msg: '子组件msg'
                }
            },
            methods: {
                // 子组件使用父组件的值
                getParent() {
                    // 获取传值
                    alert(this.title)

                    // 使用父实例获取传值
                    alert(this.parent.title)

                    // 使用父实例调用方法
                    this.parent.run('Vue')
                }
            },
            
            // 子组件接收父组件传参，方式一
            props: ['title','homeMsg','run','parent']
            
            // 子组件接收父组件传参，方式二
            // 参数校验。如果校验失败，控制台打印警告信息，一般不用
            props: {
                'title': String,
                'homeMsg': String,
                'run': Function
                'parent': Object
            }
        }
    </script>
    ```
