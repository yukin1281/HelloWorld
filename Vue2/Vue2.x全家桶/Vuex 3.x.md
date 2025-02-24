# Vuex  3.x

## 安装配置

### 安装vuex

```shell
npm i vuex@3 --save 
yarn add vuex@3 --save
```

Vuex依赖Promise。如果你支持的浏览器并没有实现 Promise (比如 IE)，那么你可以使用一个 polyfill 的库，例如 [es6-promise (opens new window)](https://github.com/stefanpenner/es6-promise)。

```shell
npm install es6-promise --save # npm
yarn add es6-promise # Yarn
```

```js
import 'es6-promise/auto'
```

### 结构目录

```shell
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```

### 初始化仓库

创建仓库 store/index.js

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
// 创建仓库 store
const store = new Vuex.Store({
    state: {
        
    },
    mutations: {
        
    },
    getters: {
        
    },
    actions: {
        
    }
})
// 导出仓库
export default store
```

导入挂载 main.js

```javascript
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false //阻止vue在启动时生成生产提示

new Vue({
  render: h => h(App),
  store
}).$mount('#app')
```



## 案例

### 单模块

store/index.js

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        // 定义初始状态
        count: 0, 
        user: {
            name: '张三',
            age: 25
        },
        todos: [
            { id: 1, text: '学习 Vuex', completed: false },
            { id: 2, text: '阅读 Vue 文档', completed: true }
        ]
    },
    mutations: {
        // 1. 修改 count
        increment(state) {
            state.count++
        },
        decrement(state) {
            state.count--
        },
        // 2. 更新用户信息
        updateUser(state, payload) {
            state.user = { ...state.user, ...payload }  ///后者属性覆盖前者属性
        },
        // 3. 添加待办事项
        addTodo(state, newTodo) {
            state.todos.push(newTodo)
        }
    },
    actions: {
        // 异步增加 count
        asyncIncrement({ commit }) {
            setTimeout(() => {
                commit('increment')
            }, 1000)
        },
        // 异步更新用户信息
        asyncUpdateUser({ commit }, userData) {
            return new Promise((resolve) => {
                setTimeout(() => {
                    commit('updateUser', userData)
                    resolve()
                }, 1000)
            })
        }
    },
    getters: {
        // 获取未完成的 todos
        incompleteTodos: (state) => {
            return state.todos.filter(todo => !todo.completed)
        },
        // 获取用户信息
        userInfo: (state) => {
            return `姓名：${state.user.name}, 年龄：${state.user.age}`
        }
    }
})

export default store
```

组件中使用vuex，读取状态

```vue
<template>
  <div>
    <p>计数器: {{ count }}</p>
    <p>{{ userInfo }}</p>
    <button @click="increment">增加</button>
    <button @click="asyncIncrement">异步增加</button>
  </div>
</template>

<script>
import { mapState, mapMutations, mapActions, mapGetters } from 'vuex'

export default {
  computed: {
    ...mapState(['count', 'user']),
    ...mapGetters(['userInfo'])
  },
  methods: {
    ...mapMutations(['increment']),
    ...mapActions(['asyncIncrement'])
  }
}
</script>
```





## 参考

https://v3.vuex.vuejs.org/zh/

https://vuex.vuejs.org/zh/

https://zhoubichuan.github.io/web-vue2x/base/vue2.x/4.vuex.html#_1-%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE