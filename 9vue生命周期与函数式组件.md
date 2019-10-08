## vue生命周期和函数式组件

![1569379349114](https://github.com/1322756558/gitbook_vue_note/blob/master/声明周期.png)

在整个声明周期中, 创建阶段以及销毁阶段只会调用一次, 而更新阶段会调用多次

![1569379439045](https://github.com/1322756558/gitbook_vue_note/blob/master/创建阶段.png)

render阶段生成虚拟DOM对结点进行挂载

![1569380128181](https://github.com/1322756558/gitbook_vue_note/blob/master/更新阶段.png)

不可以在更新阶段更改依赖数据, 因为一旦更改就会重新调用更新阶段, 导致死循环

![1569380293811](https://github.com/1322756558/gitbook_vue_note/blob/master/销毁阶段.png)

### 函数式组件

- function: true

- 无状态, 无实例, 没有this上下文, 没有生命周期

多用于临时变量