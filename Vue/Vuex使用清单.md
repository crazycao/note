# Vuex使用清单


> Vuex 是一个专为 Vue.js 应用程序开发的 **状态管理模式**。它采用 集中式存储 管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
> 
> 主要用于解决 **多个组件共享状态** 的问题：
> 
> - 多个视图依赖于同一状态。
> - 来自不同视图的行为需要变更同一状态。
> 

## 核心概念
- **State**：唯一数据源。（每个应用将仅仅包含一个 store 实例。）
- **Getter**：用于从 store 中的 state 中派生出一些状态。（相当于 store 的计算属性。）
- **Mutation**：更改 store 中的状态的唯一方法是 提交 mutation。Mutation中对状态的操作，必须是同步操作。
	- 触发方法：`this.$store.commit('xxx') `
- **Action**：相当于对 mutation 的封装，可以包含任意异步操作，最后提交的是 mutation，而不是直接变更状态。
	- 触发方法：`this.$store.dispatch('xxx')` 
- **Module**：当应用变得复杂时，允许将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块。


## 使用 Vuex
1. 安装 Vuex。

	```
	npm install vuex --save
	```
2. 在 _src_ 目录中，创建 _store_ 文件夹，并按照上述核心概念创建文件，每一个都单独拆分开，便于管理。

	```
	└──src
	   ├── components
	   │   └── ...
	   └── store
	       ├── index.js          # 我们组装模块并导出 store 的地方
	       ├── actions.js        # 异步操作
	       ├── getters.js        # 状态获取
	       ├── mutations.js      # 状态变更     
	       ├── state.js          # 状态值
	       └── modules
	       	 ├── user.js         # 子模块，模块中也包含上述核心要素
	       	 └── ...
	```

3. _index.js_ 相当于 vuex 的主目录，将拆分开的文件都在 _index.js_ 文件引入
 
	```
	import Vue from 'vue'
	import Vuex from 'vuex'

	Vue.use(Vuex)

	import state from './state'
	import getters from './getters'
	import mutations from './mutations'
	import actions from './actions'
	import user from "./modules/user"
	
	export default new Vuex.Store({
	  state,
	  getters,
	  mutations,
	  actions
	})
	```
4. 在 _state.js_ 文件中定义所需要的所有状态

	```
	export default {
    	auth: {} // 权限信息
	}
	```
5. 在 _mutaions.js_ 文件中添加更改状态的方法

	```
	export default {
   		setAuth(state, auth) {
   			if (auth) {
   				state.auth = auth;
        	} else {
            	state.auth = {};
        	}
    	}
	}
	```
6. 在 _getters.js_ 文件中添加获取状态的方法

	```
	export default {
    	authLevel: state => {
        	return state.auth ? state.auth.level : {};
    	}
	}
	```
7. 如果需要异步更改状态，在 _actions.js_ 文件中添加方法，最后提交相应的 mutation 方法。

	```
	export default {
   		setAuthAsync ({ commit }, auth) {
    		setTimeout(() => {
      			commit('setAuth', auth)
    		}, 1000)
  		}
	}
	```

8. 在 _main.js_ 文件中引入 store。通过在根实例中注册 store 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。

	```
	import Vue from 'vue'
	import App from './App.vue'
	import router from './router'
	import store from './store' // 导入，会自动检索 store 下的 index.js 文件

	Vue.config.productionTip = false

	new Vue({
  		router,
  		store,   // 引入 store
  		render: h => h(App)
	}).$mount('#app')
	```

9. 在组件中使用。

	```
	// 获取状态值：一般会在计算属性中返回某个状态
	computed: {
  		dataAuth () {
    		return this.$store.getters.authLevel; 
    		// 若不使用getters，则是 this.$store.state.auth.level 。
  		}
	}
	
	// 修改状态值：
	this.$store.commit('setAuth', { 
		level: 10
	}) // 设置权限等级为 10
	
	// 异步修改状态值
	this.$store.dispatch('setAuthAsync', { 
		level: 10
	}) // 设置权限等级为 10
	```

10. modules 的使用：
	
	(1) 构建一个module文件，如 _user.js_

	```
	// state
	const state = {
		userInfo: {}
	};

	// getters
	const getters = {};

	// actions
	const actions = {};

	// mutations
	const mutations = {
		setUserInfo(state, info) {
   			state.userInfo = info;
   		}
	};

	export default {
  		state,
  		getters,
  		actions,
  		mutations
	};

	```
	
	(2) 在 _index.js_ 中导入
	
	```
	import user from "./modules/user"

	export default new Vuex.Store({
  		modules: {
    		user
  		}
	})
	```

	(3) 使用时，取值要先获取到 module 的状态；但修改值可以直接提交 mutation 的事件类型即可。
	
	```
	computed: {
  		userInfo () {
    		return this.$store.state.user.userInfo;
  		}
	}
	this.$store.commit('setUserInfo', userInfo);
	```