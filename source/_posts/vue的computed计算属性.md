---
title: vue的computed计算属性
date: 2019-11-08 23:40:12
tags:
- vue
- computed
---

## 工作项目里用到vue的计算属性比较多，但是自己对这个东西还不是很了解。今天通过探讨之后得出一点结论。
### 1. 如果是在get当中处理过，那么就是通过处理的方式，得到return出去的值，根据其他值的变化而变化。
```javascript
<template>
  <div>
    <div>
      <input type="text" v-model="message" />
      <div>{{changeMessage}}</div>
    </div>
  </div>
</template>

<script>
   export default {
    data () {
       return {
         message: 'hello'
       }
     },
    computed: {
       changeMessage: {
        // 计算属性：依赖message变化而变化  依赖没变化就不会重新渲染；
        get () {
           return this.message + 'world'
        },
        set () {
        }
      }
     }
  }
</script>
```
### 2. 而set指的是因为我computed的值改变了，所以我可以在方法里面处理一些东西，其他的数值根据他的改变而改变。
```javascript
<template>
  <div>
    <div>
      {{didi}}
      {{family}}
    </div>
    <div>
      {{didiFamily}}
    </div>
  </div>

</template>

<script>
   export default {
    data () {
       return {
        didi: 'didi',
        family: 'family'
       }
     },
    computed: {
      didiFamily:{
        //getter
        get:function(){
          return this.didi + ' ' + this.family
        },
        //setter
        set:function(newValue){
          // 这里由于该计算属性被赋值，将被调用
          console.log(newValue)
          this.didi = 123
          this.family = 456
        }
      }
    },
    mounted () {
      // 赋值，调用setter函数
      this.didiFamily = 'John Doe'
    }
  }
</script>
```

### 计算属性是基于它们的响应式依赖进行缓存的。
这个是它区别于vue methods的一个地方，举一个简单的例子
```javascript
// 计算属性
computed: {
  reversedMessage () {
    return this.message.split('').reverse().join('')
  }
}
// 方法
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。而方法却会执行。这就是computed会产生缓存，减少性能开销。

判断computed的值是否发生改变，可以看return返回的是不是响应式的依赖。

根据computed的缓存特点，我们在项目过程中，就会产生一个疑问，什么时候需要用到它的依赖缓存特性。

**答案是：父组件给子组件传值，值的类型为引用类型**

父组件
```html
<template>
  <div>
    <child :user="user"></child>
    <label for="user">parent：</label>
    <input id="user" type="text" v-model="user.name">
  </div>
</template>
<script>
import Child from './child.vue'
export default {
  data () {
    return {
      user: { name: 'ligang' }
    }
  },
  components: { Child }
}
</script>
```
子组件中需要同时显示改变前和改变后的值。

子组件
```html
<template>
  <div>
    <div>child:</div>
    <div>修改前：{{oldUser}} 修改后：{{user}}</div>
  </div>
</template>
<script>
export default {
  name: 'child',
  props: ['user'],
  data () {
    return {
      oldUser: {}
    }
  },
  // 缓存 userInfo   
  computed: {
    userInfo () {
      return { ...this.user }
    }
  },
  watch: {
    userInfo: {
      handler (val, oldVal) {
        this.oldUser = oldVal || val
      },
      deep: true,
      immediate: true
    }
  }
}
</script>
```
**需要使用 `{ ...this.user }` 或者使用 `Object.assign({}, this.user)` 来创建新的引用，不然会报错，不允许修改父组件的值！**