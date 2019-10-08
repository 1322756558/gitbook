## vue数据,属性,计算属性,监听器

**data**: 

作用于当前页面的数据 



**pops**: 

属性-理论上应该是由父组件传递过来的, 所以不应该去修改pops, 要么使用data 要么写个方法进行回调, 用父方法修改pops的值



**computed**:计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例, 如果他的返回值有变化, 就会重新对结点进行渲染, 但是前提是数据已经注册了, 比如下面的代码中message注册了, 那么当message改变的时候就会重新渲染结点, 更新数据, 但是Data.now没有被注册所以不会被监听并重新渲染

相比watch, computed拥有缓存机制所以在绝大多数时候能用computed还是用computed, 还是会有一些性能的提升

与methods比computed只有在返回值变动的时候才会重新渲染, 如果用methods来实现的话, 使用forceUpdate来强制刷新的时候一看到methods会重新渲染, 而computed不会

```js
<p>Reversed message1: "{{ reversedMessage1 }}"</p>

data() {
    return {
      message: "hello vue"
    };
  },
computed: {
    // 计算属性的 getter
    reversedMessage1: function() {
      console.log("执行reversedMessage1");
      return this.message
        .split("")
        .reverse()
        .join("");
    },
    now: function() {
      return Date.now();
    }
  },
```



**监听器 watch**

优点

- 灵活, 通用
- watch中可以执行任何逻辑, 函数节流, Ajax异步获取数据, 甚至DOM操作(不要这么干)

与computed的区别

- computed能做的watch都能, 反之则不一定
- 能用computed的尽量用computed 原因上面说了



watch

```vue
<template>
  <div>
    {{ fullName }}

    <div>firstName: <input v-model="firstName" /></div>
    <div>lastName: <input v-model="lastName" /></div>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      firstName: "Foo",
      lastName: "Bar",
      fullName: "Foo Bar"
    };
  },
  watch: {
    firstName: function(val) {
      this.fullName = val + " " + this.lastName;
    },
    lastName: function(val) {
      this.fullName = this.firstName + " " + val;
    }
  }
};
</script>

```



computed

```vue
<template>
  <div>
    {{ fullName }}

    <div>firstName: <input v-model="firstName" /></div>
    <div>lastName: <input v-model="lastName" /></div>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      firstName: "Foo",
      lastName: "Bar"
    };
  },
  computed: {
    fullName: function() {
      return this.firstName + " " + this.lastName;
    }
  },
  watch: {
    fullName: function(val, oldVal) {
      console.log("new: %s, old: %s", val, oldVal);
    }
  }
};
</script>
```



