# vue中的虚拟dom-vnode

> vnode即virtual dom node，用js对象来描述一个dom节点

- 为什么要有虚拟dom？

  ​	为了减少真实dom操作次数，操作真实dom耗费性能

- 虚拟dom的作用

  在视图渲染之前，把template编译成vnode并且缓存下来，当数据变化视图需要重新渲染的时候，把数据发生变化后的vnode与前一次缓存下来的vnode作对比，找出差异，然后有差异的VNode对应的真实DOM节点就是需要重新渲染的节点，最后根据有差异的VNode创建出真实的DOM节点再插入到视图中，最终完成一次视图更新。
  
  

