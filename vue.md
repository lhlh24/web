### Redux 和 Vuex 有什么区别，说下一它们的共同思想

公司：快手

分类：React、Vue

解析：

1）Redux和Vuex的区别

- Vuex改进了Redux中的Action和Reducer函数，以mutations变化函数取代Reducer，无需switch，只需在对应的mutation函数里改变state值即可
- Vuex由于Vue自动重新渲染的特性，无需订阅重新渲染函数，只要生成新的State即可
- Vuex数据流的顺序是：View调用store.commit提交对应的请求到Store中对应的mutation函数 ->store改变（vue检测到数据变化自动渲染）

通俗点理解就是，Vuex弱化dispatch，通过commit进行store状态的一次更变；取消了action的概念，不必传入特定的action形式进行指定变更；弱化Reducer，基于commit参数直接对数据进行转变，使得框架更加简易。

2）共同思想

- 单一的数据源

- 变化可以预测

  本质上：Redux和Vuex都是对MVVM思想的服务，将数据从视图中抽离的一种方案；

  形式上：Vuex借鉴了Redux，将store作为全局的数据中心，进行mode管理；



### 说一下对 React 和 Vue 的理解，它们的异同

公司：网易、脉脉、快手

分类：React、Vue

解析：





### 第2题.Vuex和localStorage的区别

1.**最重要的区别**

- vuex存储在内存

- localstorage则以文件的方式存储在本地，localstorage只能存储

  localstorage只能存储字符串类型的数据，存储对象需要JSON的stringify和parse方法进行处理。读取内存比读取硬盘速度要快。

2.**应用场景**

- Vuex是一个专为Vue.js应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex用于组件之间的传值
- localstorage是本地存储，是将数据存储到浏览器的方法，一般是在跨页面传递数据时使用。
- Vuex能做到数据的响应式，localstorage不能

3.**永久性**

- 刷新页面时Vuex存储的值会丢失，localstorage不会





### 第3题.Vue双向绑定原理

**1.原理**

![image-20210401100452296](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20210401100452296.png)

View的变化能实时让Model发生变化，而Model的变化也能实时更新View。

Vue采用数据劫持&发布-订阅模式的方式，通过ES5提供的Object.defineProperty()方法来劫持（监控）各属性的getter、setter，并在数据（对象）发生变动时通知订阅者，触发相应的监听回调。并且，由于是在不同的数据上触发同步，可以精确的将变更发送给绑定的视图，而不是对所有的视图都执行一次检测。要实现Vue中的双向数据绑定，大致可以划分三个模块：Observer、Compile、Watcher，如图：

![image-20210401101012394](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20210401101012394.png)

- **Observer数据监听器**，负责对数据对象的所有属性进行监听（数据劫持），监听到数据发生变化后通知订阅者。

- **Compiler指令解析器**，扫描模板，并对指令进行解析，然后绑定指定事件。

- **Watcher订阅者**，关联Observer和Compiler，能够订阅并收到属性变动的通知，执行指定绑定的操作，更新视图。Update()是它自身的一个方法，用于执行Compiler中绑定的回调，更新视图。



**2.版本比较**

vue是基于依赖收集的双向绑定；

3.0之前的版本使用Object.defineProperty，3.0新版使用Proxy

**1）基于 数据劫持/依赖收集 的双向绑定的优点**

- 不需要显示的调用，Vue利用数据劫持+发布订阅，可以直接通知变化并且驱动视图

- 直接得到精确的变化数据，劫持了属性setter，当属性值改变我们可以精确的获取变化的内容newVal，不需要额外的diff操作

**2）object.defineProperty的缺点**

- 不能监听数组；因为数组没有getter和setter，因为数组长度不确定，如果太长性能负担太大

- 只能监听属性，而不是整个对象；需要遍历属性；

- 只能监听属性变化，不能监听属性的删减；

**3）Proxy好处**

- 可以监听数组；

- 监听整个对象不是属性；

- 13种拦截方法，强大很多；

- 返回新对象而不是直接修改原对象，更符合immutable；

**4）Proxy缺点**

- 兼容性不好，且无法用polyfill磨平；







说一下路由钩子在Vue生命周期的体现？













