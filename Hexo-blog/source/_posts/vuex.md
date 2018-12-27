---
title: 关于Vuex的应用
categories: 前端技术
tags:
- Vue.js
- Vuex
---

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
<!-- more -->

## 安装
```
npm i vuex -S
```
## 使用

```javascript
// store.js 声明
import Vuex from 'vuex'
import defaultState from './state/state'
import mutations from './mutations/mutations'
import getters from './getters/getters'
export default ()=>{
	return new Vuex.Store({
		strict: true, 
		// 使通过 this.$store.state.count=3 修改值，会报错，开发模式下使用
		state:defaultState,  // 数据状态
		mutation,	// 保存数据同步方法
		getters,  // 类似vue中的computed
		actions, // 保存数据异步方法
	})
}
```
```javascript
// 引用
import createStore from './store/store'
Vue.use(Vuex)
const store = createStore()
...
// 使用
this.$store.state.count   //state
this.$store.commit('方法名', param)  //mutations
this.$store.dispatch('方法名', param) //actions
```
同时vuex，提供了便捷的方法，可在vue中定义
```javascript
import { mapState, mapGetters, mapActions, mapMutations} from 'vuex'
...
computed:{
	...mapState(['count']),  //同名
	...mapState({
		counter:'count',
		counter:(state) => state.count 
	}),
	...mapGetters(['同名方法名'])
}
methods:{
	...mapActions(['同名方法名'])
	...mapMutations(['同名方法名'])
}
```
## Vuex 模块化

```javascript
// store.js
...{
	modules:{
		a:{
			namespaced: true,  // 默认false，会把mutations下方法定义到全局 
			state:{ text: 1 },
			mutations:{   
				updateText(state,text){
					state.text = text
				}
			},
			getters:{
				textPlus(state, getter, rootState){  
					// 通过rootState可以获取全局的数据
					return state.text + rootState.count + rootState.b.text
				}
			}，
			actions:{
				add({ state, commit, rootState}){
					commit('updateText', rootState.count)
					commit('updateCount', rootState, {root:true})
					// 通过 root:true 使模块可以调用全局的方法
				}
			} 
		},
		b:{ state:{ text: 2 } }
	}
}
...
vue中调用
this.$store.state.a.text  //1
this.$store.state.b.text  //2...
...mapMutations(['a/updateText'])
...mapMutations({
	updataText: state => state.a.updateText
})
```


## 扩展
使支持ES最新的语法
```
cnpm i babel-preset-stage-1 -D
```
