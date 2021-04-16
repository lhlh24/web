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







### 说一下路由钩子在Vue生命周期的体现？

**一、Vue-Router导航守卫**

有的时候，我们需要通过路由来进行一些操作，比如最常见的登录权限验证，当用户满足条件时，才让其进入导航，否则就取消跳转，并跳到登录页面让其登录。

为此我们有很多种方法可以植入路由的导航过程：全局的，单个路由独享的，或者组件级的

**1.全局路由钩子**

vue-router全局有三个路由钩子：

- router.beforeEach 全局前置守卫 进入路由之前
- router.beforeResolve 全局解析守卫（2.5.0+）在beforeRouterEnter调用之后调用
- router.afterEach 全局后置钩子 进入路由之后

具体使用：

- beforeEach（判断是否登录了，没登录就跳转到登录页）

```JavaScript
router.beforeEach((to, from, next) => {  
    let ifInfo = Vue.prototype.$common.getSession('userData');  // 判断是否登录的存储信息
    if (!ifInfo) { 
        // sessionStorage里没有储存user信息    
        if (to.path == '/') { 
            //如果是登录页面路径，就直接next()      
            next();    
        } else { 
            //不然就跳转到登录      
            Message.warning("请重新登录！");     
            window.location.href = Vue.prototype.$loginUrl;    
        }  
    } else {    
        return next();  
    }
})
```

- afterEach（跳转之后滚动条返回顶部）

```JavaScript
router.afterEach((to, from) => {  
    // 跳转之后滚动条回到顶部  
    window.scrollTo(0,0);
});
```

**2.单个路由独享钩子**

**beforeEnter**

如果不想全局配置守卫的话，可以为某些路由单独配置守卫

有三个参数：to、from、next

```javascript
export default [    
    {        
        path: '/',        
        name: 'login',        
        component: login,        
        beforeEnter: (to, from, next) => {          
            console.log('即将进入登录页面')          
            next()        
        }    
    }
]
```

**3.组件内钩子**

**beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave**

这三个钩子都有三个参数：to、from、next

- beforeRouteEnter：进入组件前触发

- beforeRouteUpdate：当前地址改变并且改组件被复用时触发，举例来说，带有动态参数的路径foo/:id，在/foo/1和/foo/2之间跳转的时候，**由于会渲染同样的foo组件，这个钩子在这种情况下就会被调用**

- beforeRouteLeave：离开组件被调用

注：beforeRouteEnter组件内还访问不到this，因为该守卫执行前组件实例还没有被创建，需要传一个回调给next来访问，例如

```JavaScript
beforeRouteEnter(to, from, next) {      
    next(target => {        
        if (from.path == '/classProcess') {          
            target.isFromProcess = true        
        }      
    })    
}
```

beforeRouteUpdate和beforeRouteLeave可以访问组件实例this

**二、Vue路由钩子在生命周期函数的体现**

**1.完整的路由导航解析流程（不包括其他生命周期）**

- 触发进入其他路由	

- 调用要离开路由的组件守卫 beforeRouteLeave

- 调用局前置守卫：beforeEach

- 在重用的组件调用 beforeRouteUpdate

- 调用路由独享守卫 beforeEnter

- 解析异步路由组件

- 在将要进入的路由组件中调用 beforeRouteEnter

- 调用全局解析守卫 beforeResolve

- 导航被确认

- 调用全局后置钩子的afterEach钩子

- 触发DOM更新（mounted）

- 执行beforeRouteEnter守卫中传给next的回调函数

**2.触发钩子的完整顺序**

路由导航、keep-alive、和组件生命周期钩子结合起来的，触发顺序，假设是从a组件离开，第一次进入b组件：

- beforeRouteLeave：路由组件的组件离开路由前钩子，可取消路由离开

- beforeEach：路由全局前置守卫，可用于登录验证、全局路由loading等

- beforeEnter：路由独享守卫

- beforeRouteEnter：路由组件的组件进入路由前钩子

- beforeResolve：路由全局解析守卫

- afterEach：路由全局后置钩子

- beforeCreate：组件生命周期，不能访问this

- created：组件生命周期，可以访问this，不能访问DOM

- beforeMount：组件生命周期

- deactivated：离开缓存组件a，或者触发a的beforeDestroy和destroyed组件销毁钩子

- mounted：访问/操作DOM

- actived：进入缓存组件，进入a的嵌套子组件（如果有的话）。

- 执行beforeRouteEnter回调函数next

  

### 计算属性与普通的区别

区别

computed属性是vue的计算属性，是数据层到视图层的数据转化映射；

计算属性是基于它们的依赖进行缓存的，只有在相关依赖发生改变时，它们才会重新求值，也就是说，只要它的依赖没有发生变化，那么每次访问的时候计算属性都会立即返回之前的计算结果，不再执行函数；

- computed是响应式的，methods并非响应式。
- 调用方式不一样，computed定义的成员像属性一样访问，methods定义的成员必须以函数形式调用
- computed是带缓存的，只有依赖数据发生改变，才会重新进行计算，而methods里的函数在每次调用时都要执行
- computed中的成员可以只定义一个函数作为只读属性，也可以定义get/set变成可读写属性，这点是methods中的成员做不到的
- computed不支持异步，当computed内有异步操作时无效，无法监听数据的变化

如果声明的计算属性计算量非常大的时候，而且访问量次数非常多，改变的时机却很小，那就需要用到computed；缓存会让我们减少很多计算量



### 描述下自定义指令（你是怎么用自定义指令的）

自定义指令

在vue2.0中，代码复用和抽象的主要形式是组件。然而，有的情况下，仍然需要对普通DOM元素进行底层操作，这时候就会用到自定义指令。

一般需要对DOM元素进行底层操作时使用，尽量只用来操作DOM展示，不修改内部的值。**当使用自定义指令直接修改value值时绑定v-model的值也不会同步更新**；如必须修改可以在自定义指令中使用keydown事件，在vue组件中使用change事件，回调中修改vue数据；

1.自定义指令基本内容

- 全局定义：Vue.directive("focus",{})
- 局部定义：directives:{focus:{}}
- 钩子函数：指令定义对象提供钩子函数
  - bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
  - inserted：被绑定元素插入父节点时调用（仅保证父节点存在，但不一定已被插入文档中）
  - update：所在组件的VNode更新时调用，但是可能发生在其子VNode更新之前调用。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新
  - componentUpdate：指令所在组件的VNode及其子VNode全部更新后调用
  - unbind：只调用一次，指令与元素解绑时调用

- 钩子函数参数

  - el：绑定元素

  - bing：指令核心对象，描述指令全部信息属性

    - name

    - value

    - oldValue

    - expression

    - arg

    - modifers
    
  - vnode：虚拟节点
  
  - oldVnode：上一个虚拟节点（更新钩子函数中才有用）
  
    

2.使用场景

- 普通DOM元素进行底层操作的时候，可以使用自定义指令
- 自定义指令是用来操作DOM的。尽管Vue推崇数据驱动视图的概念，但并非所有情况都适合数据驱动。自定义指令就是一种有效的补充和扩展，不仅可用于自定义任何的DOM操作，并且是可复用的
- 

3.使用案例

初级应用：

- 鼠标聚焦
- 下拉菜单
- 相对时间转换
- 滚动动画

高级应用：

- 自定义指令实现图片懒加载
- 自定义指令集成第三方插件



### 说一下Vue中所有带\$的方法





### Vue template到render的过程

**过程分析**

vue的模板编译过程主要如下：**template -> ast -> render函数**

vue在模板编译版本的源码中会执行compileToFunctions将template转化为render函数

```JavaScript
// 将模板编译为render函数
const { render, staticRenderFns } = compileToFunctions(template,optinos//省略}, this)
```

compileToFunctions中的主要逻辑如下：

**1.调用parse方法将template转化为ast（抽象语法树）**

```javascript
const ast = parse(template.trim(),options)
```

**parse的目标**：是把template**转换为AST树**，它是一种用JavaScript对象的形式来描述整个模板

**解析过程**：利用正则表达式顺序解析模板，当解析到开始标签、闭合标签、文本的时候都会分别执行对应的回调函数，来达到构造AST树的目的

AST元素节点总共三种类型：type为1表示普通元素，2为表达式，3为纯文本

**2.对静态节点做优化**

```JavaScript
optimize(ast,options)
```

这个过程主要**分析出哪些是静态节点**，给其打一个标记，为后序更新渲染可以直接跳过静态节点做优化

深度遍历AST，查看每个子树的节点元素是否为静态节点或者静态节点根。**如果为静态节点，它们生成的DOM永远不会改变**，这对运行时模板更新起到了极大的优化作用。

3.生成代码

```javascript
const code = generate(ast, options)
```

generate将ast抽象语法树编译成render字符串，并将静态部分放到staticRenderFns中，最后通过new Function(render)生成render函数。



### 怎么定义 vue-route 的动态路由？怎么获取传过来的动态参数？

实现方式

**1.param方式**

- 配置路由格式：/route/:id
- 传递的方式：在path后面跟上对应的值
- 传递后形成的路径：/route/123

**1）路由定义**

```vue
//在APP.vue中
<router-link :to="'/user/'+userId" replace>用户</router-link>    

//在index.js
{
   path: '/user/:userid',
   component: User,
},
```

**2）路由跳转**

```JavaScript
// 方法1：
<router-link :to="{ name: 'users', params: { uname: wade }}">按钮</router-link>

// 方法2：
this.$router.push({name:'users', params:{uname:wade}})

// 方法3：
this.$router.push('/user/' + wade)
```

**3）参数获取**

通过 $route.params.userid 获取传递的值



**2.query方式**

- 配置路由格式：/route，也就是普通配置
- 传递的方式：对象中使用query的key作为传递方式
- 传递后形成的路径：/route?id=123

**1）路由定义**

```JavaScript
//方式1：直接在router-link 标签上以对象的形式
<router-link :to="{path:'/profile',query:{name:'why',age:28,height:188}}">档案</router-link>

// 方式2：写成按钮以点击事件形式

<button @click='profileClick'>我的</button>    

profileClick(){
  this.$router.push({
    path: "/profile",
    query: {
        name: "kobi",
        age: "28",
        height: 198
    }
  });
}
```

**2）跳转方法**

```JavaScript
// 方法1：
<router-link :to="{ name: 'users', query: { uname: james }}">按钮</router-link>

// 方法2：
this.$router.push({ name: 'users', query:{ uname:james }})

// 方法3：
<router-link :to="{ path: '/user', query: { uname:james }}">按钮</router-link>

// 方法4：
this.$router.push({ path: '/user', query:{ uname:james }})

// 方法5：
this.$router.push('/user?uname=' + jsmes)
```

**3）获取参数**

```JavaScript
// 通过$route.query 获取传递的值
```



### 为什么要用Vuex或者Redux

由于传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致代码无法维护。

所以我们需要**把组件的共享状态抽取出来，以一个全局单例模式管理**。在这种模式下，我们的组件构成了一个巨大的“视图“，不管在树的哪个位置，任何组件都能获取状态或者触发行为！

另外，通过定义和隔离状态管理中的各种概念并强制遵守一定的规则，我们的代码将会变得更结构化且易于维护。



### vue data  中某一个属性的值发生改变后，视图会立即同步执行重新渲染吗？

**一、答案解析**

**不会**

Vue实现响应式并不是数据发生变化之后DOM立即变化，而是按一定的策略进行DOM更新。

Vue在更新DOM时是**异步执行**的。只要侦听到数据变化，Vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。

如果同一个watcher被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和DOM操作是非常重要的。

然后，在下一个的事件循环“tick”中，Vue刷新队列并执行实际（已去重的）工作。

**二、异步执行的运行机制**

1.所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）

2.主线程之外，还存在一个“任务队列”（task queue）。只要异步任务有了运行结果，就在“任务队列”之中放置一个事件。

3.一旦“执行栈”中的所有同步任务执行完毕，系统就会读取“任务队列”，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行

4.主线程不断重复上面的第三步

![image-20210402104752685](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20210402104752685.png)

事件循环说明

简单来说，Vue在修改数据后，视图不会立刻更新，**而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新**。



















