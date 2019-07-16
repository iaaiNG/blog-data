---
title: Vue技巧
date: 2019-06-20 23:31:17
categories: 前端技术
---

时隔快两月，终于想起写博客纪录，整里一些最近项目中使用 Vue 技巧的东西。
<!-- more -->


# 一、在组件上使用 v-model
## (1) 传统 v-model 原理
```js
<input v-model="searchText">
```
等价于
```js
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```

## (2) 当用在组件上时，v-model 则会这样：
```js
<custom-input v-model="searchText"></custom-input>
```
等价于
```js
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```

## (3) 实现 custom-input
```js
<template>
  <input 
    v-bind:value="value" 
    v-on:input="$emit('input', $event.target.value)" 
  >
</template>
<scritp>
  export default {
    props: ['value'], 
  }
</scritp>
```
现在，这个组件上的 v-model 可以完美地工作起来了。

## (4) 改变 v-model 的默认行为
组件上的 v-model 默认会利用名为 `value` 的 `prop` 和名为 `input` 的事件。  
但像 `<checkbox>` `<select>` 等类型的输入控件可能会将 value 特性用于不同的目的。
**`model` 选项可以用来避免这样的冲突：**
```js
<scritp>
  export default {
    model: {
      prop: 'checked',
      event: 'change'
    },
    props: ['value'], 
  }
</scritp>
```

```js
<base-checkbox v-model="lovingVue"></base-checkbox>
```
这里的 lovingVue 的值将会传入这个名为 checked 的 prop。并且可以触发 change 事件更新 checked。


# 二、.sync 修饰符

## (1) 原理
```html
<comp :foo.sync="bar"></comp>
```
会被扩展为：
```html
<comp :foo="bar" @update:foo="val => bar = val"></comp>
```
当子组件需要更新foo的值时，它需要显式地触发一个更新事件：
```js
this.$emit('update:foo', newValue)
```

## (2) 运用
当子组件需要改变父组件的值的时候，结合 computed 的 get, set 设置，尤其有效。
```html
<child :data.sync="data"></child>
```

再 computed 定义 myData，便可以在子组件灵活使用 myData。
```js
// in child components
<scritp>
export default {
  prop:['data'],
  computed:{
    myData:{
      get(){
        return this.data
      },
      set(val){
        this.$emit('update:data', val)
      }
    }
  }
}
</scritp>
```
