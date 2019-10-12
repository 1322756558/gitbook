## 高级特性 provide/inject

provide/inject为解决组件间通讯而存在

![数据的组件间传递](D:\mydir\vue笔记\数据的组件间传递.png)

一般来说直接调用组件的时候这个功能并不常用, 但是如果在写底层组件的时候这个功能是很好用的一个功能

当我们需要跨组件传递数据的时候如果我们如果我们常规方法使用属性层层传递的时候是非常繁杂而且几乎没有什么健壮性可言的, 

如图中结构, A结点分发数据, 后面的结点接收数据

```html
--------------------父节点--------------------
<template>
  <div class="border">
    <h1>A 结点</h1>
    <button @click="() => changeColor()">改变color</button>
    <ChildrenB />
    <ChildrenC />
    <ChildrenD />
  </div>
</template>
<script>
import ChildrenB from "./ChildrenB";
import ChildrenC from "./ChildrenC";
import ChildrenD from "./ChildrenD";
export default {
  components: {
    ChildrenB,
    ChildrenC,
    ChildrenD
  },
------------这样无法改变, color非响应式--------
  provide() {
    return {
      theme: {
        color: this.color
      }
    };
  },
------------这样是对的-------------
  provide() {
    return {
      theme: this
    };
  },
-----------------------------------
  data() {
    return {
      color: "blue"
    };
  },
  methods: {
    changeColor(color) {
      if (color) {
        this.color = color;
      } else {
        this.color = this.color === "blue" ? "red" : "blue";
      }
    }
  }
};
</script>
```

可以看到父节点通过provide向下级分发数据-color

```html
--------------------E结点-------------------
<template>
  <div class="border2">
    <h3 :style="{ color: theme.color }">E 结点</h3>
    <button @click="handleClick">改变color为green</button>
  </div>
</template>
<script>
export default {
  components: {},
  inject: {
    theme: {
      default: () => ({})
    }
  },
  methods: {
    handleClick() {
      if (this.theme.changeColor) {
        this.theme.changeColor("green");
      }
    }
  }
};
</script>

```

对于子节点E数据通过inject 注入 theme 的方式, 插入子节点

```html
<template>
  <div class="border2">
    <h3 :style="{ color: theme1.color }">F 结点</h3>
  </div>
</template>
<script>
export default {
  components: {},
  inject: {
    theme1: {
      from: "theme",
      default: () => ({})
    }
  }
};
</script>

```

这里与上面E结点的区别是为了方式重名, 利用了from的方式使用了别名theme1

```html
<template functional>
  <div class="border2">
    <h3 :style="{ color: injections.theme.color }">I 结点</h3>
  </div>
</template>
<script>
export default {
  inject: {
    theme: {
      default: () => ({})
    }
  }
};
</script>

```

函数式组件通过injections.theme.color取得数据



并且要注意, inject向上寻找provide的时候是逐级查找, 查找最近的一个, 比如如果c结点有一个provide 那么E, F是不能接收到A结点的数据的