template

- 模板语法(HTML拓展)
- 数据绑定使用Mustache语法(双大括号)

```html
<span>Message:: {{ msg }}</span>
```



jsx

- javascript的语法拓展
- 数据绑定使用单引号

```html
<span>Message: {this.msg}</span>
```

可以在jsx中写各种数据逻辑



比较:

template: 学习成本低, 大量内置指令简化开发, 组件作用域CSS, 灵活性低

jsx: 灵活

当我们的逻辑越来越复杂的时候推荐使用jsx

template:

```html
<template>
  <h1 v-if="level === 1">
    <slot></slot>
  </h1>
  <h2 v-else-if="level === 2">
    <slot></slot>
  </h2>
  <h3 v-else-if="level === 3">
    <slot></slot>
  </h3>
  <h4 v-else-if="level === 4">
    <slot></slot>
  </h4>
  <h5 v-else-if="level === 5">
    <slot></slot>
  </h5>
  <h6 v-else-if="level === 6">
    <slot></slot>
  </h6>
</template>
<script>
export default {
  props: {
    level: {
      type: Number,
      default: 1
    }
  }
};
</script>

```

jsx:

```html
export default {
  props: {
    level: {
      type: Number,
      default: 1
    }
  },
  render: function(h) {
    const Tag = `h${this.level}`;
    return <Tag>{this.$slots.default}</Tag>;
  }
};

```



当逻辑越来越复杂,  可以使用jsx与template混合开发

```html
<template>
  <div>
    <span>Message: {{ msg }}</span>
    <br />
    <VNodes :vnodes="getJSXSpan()" />
    <anchored-heading1 :level="1">Hello world!</anchored-heading1>
    <anchored-heading2 :level="2">Hello world!</anchored-heading2>
    <anchored-heading3 :level="3">Hello world!</anchored-heading3>
    <VNodes :vnodes="getAnchoredHeading(4)" />
  </div>
</template>
<script>
import AnchoredHeading1 from "./AnchoredHeading.vue";
import AnchoredHeading2 from "./AnchoredHeading.js";
import AnchoredHeading3 from "./AnchoredHeading.jsx";

export default {
  components: {
    AnchoredHeading1,
    AnchoredHeading2,
    AnchoredHeading3,
    VNodes: {
      functional: true,
      render: (h, ctx) => ctx.props.vnodes
    }
  },
  data() {
    return {
      msg: "hello vue"
    };
  },
  methods: {
    getJSXSpan() {
      return <span>Message: {this.msg}</span>;
    },
    getAnchoredHeading(level) {
      const Tag = `h${level}`;
      return <Tag>Hello world!</Tag>;
    }
  }
};
</script>

```

上面的效果都是输出一串helloworld

![1571314968864](D:\mydir\vue笔记\jsx_template.png)

其实他们都是语法糖而已, 最终都会编译成下面的形式