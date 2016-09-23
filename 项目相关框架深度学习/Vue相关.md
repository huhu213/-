* Vue.js不使用脏检查，基于依赖追踪的观察系统并异步队列更新，所有数据变化都是独立的，除非有明确的依赖关系————深入研究下
* Angular基于脏检查，watcher，视图更新，digest cycle（脏检查循环）。
* React基于Virtual DOM，将DOM数映射到内存中，通过更新Virtual DOM来对现有的DOM进行修改。
* Vue.js的模板基于真实的DOM，而不是字符串
* Vue.js在基于数据进行DOM更新时，通过实现一些启发算法以求最大化复用DOM元素
* 