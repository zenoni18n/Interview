#  Vue2

### 简单

#### 1 MVC 和 MVVM 区别

##### MVC

MVC 全名是 Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范

- Model（模型）：是应用程序中用于处理应用程序数据逻辑的部分。通常模型对象负责在数据库中存取数据
- View（视图）：是应用程序中处理数据显示的部分。通常视图是依据模型数据创建的
- Controller（控制器）：是应用程序中处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据

![mvc.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e4d22e916014ee7abb10e4b350e5583~tplv-k3u1fbpfcp-watermark.image)

MVC 的思想：一句话描述就是 Controller 负责将 Model 的数据用 View 显示出来，换句话说就是在 Controller 里面把 Model 的数据赋值给 View。

##### MVVM

MVVM 新增了 VM 类

- ViewModel 层：做了两件事达到了数据的双向绑定 一是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的页面。实现的方式是：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转化成后端的数据。实现的方式是：DOM 事件监听。

![mvvm.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/32c0545686454396a7e39b86b644fe73~tplv-k3u1fbpfcp-watermark.image)

MVVM 与 MVC 最大的区别就是：它实现了 View 和 Model 的自动同步，也就是当 Model 的属性改变时，我们不用再自己手动操作 Dom 元素，来改变 View 的显示，而是改变属性后该属性对应 View 层显示会自动改变（对应Vue数据驱动的思想）

整体看来，MVVM 比 MVC 精简很多，不仅简化了业务与界面的依赖，还解决了数据频繁更新的问题，不用再用选择器操作 DOM 元素。因为在 MVVM 中，View 不知道 Model 的存在，Model 和 ViewModel 也观察不到 View，这种低耦合模式提高代码的可重用性

> 注意：Vue 并没有完全遵循 MVVM 的思想 这一点官网自己也有说明

![vue-mvvm.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0290c7c656b449718db920a5456d0f45~tplv-k3u1fbpfcp-watermark.image)

那么问题来了 为什么官方要说 Vue 没有完全遵循 MVVM 思想呢？

> - 严格的 MVVM 要求 View 不能和 Model 直接通信，而 Vue 提供了$refs 这个属性，让 Model 可以直接操作 View，违反了这一规定，所以说 Vue 没有完全遵循 MVVM。

##### 说说你对MVVM的理解

- Model-View-ViewModel的缩写，Model代表数据模型，View代表UI组件,ViewModel将Model和View关联起来
- 数据会绑定到viewModel层并自动将数据渲染到页面中，视图变化的时候会通知**viewModel**层更新数据

#### 2 为什么 data 是一个函数

组件中的 data 写成一个函数，数据以函数返回值形式定义，这样每复用一次组件，就会返回一份新的 data，类似于给每个组件实例创建一个私有的数据空间，让各个组件实例维护各自的数据。而单纯的写成对象形式，就使得所有组件实例共用了一份 data，就会造成一个变了全都会变的结果

- 一个组件被复用多次的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数。
- 如果data是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间data不冲突，data必须是一个函数。

#### 子组件为什么不可以修改父组件传递的Prop？/怎么理解vue的单向数据流？

- Vue提倡单向数据流,即父级props的更新会流向子组件,但是反过来则不行。
- 这是为了防止意外的改变父组件状态，使得应用的数据流变得难以理解。
- 如果破坏了单向数据流，当应用复杂时，debug 的成本会非常高。

#### 3 Vue 组件通讯有哪几种方式

1. props 和$emit 父组件向子组件传递数据是通过 prop 传递的，子组件传递数据给父组件是通过$emit 触发事件来做到的

```javascript
.sync
<comp :foo.sync="bar"></comp>
会被扩展为：
<comp :foo="bar" @update:foo="val => bar = val"></comp>
当子组件需要更新 foo 的值时，它需要显式地触发一个更新事件：
this.$emit('update:foo', newValue)

Vue3  v-model:app="..."
<!--父组件--!>
 <HelloWorld v-model:foo="modelValue" v-model:message="msg"></HelloWorld>
<!--子组件--!>
  <input type="text" :value="foo" @input="$emit('update:foo', $event.target.value)" />

 <input type="text" :value="message" @input="$emit('update:message', $event.target.value)" />
     自定义修饰符的用法，

根据官方

//父组件

<HelloWorld v-model.capitalize="myText"></HelloWorld>
{{myText}}
setup(){
	return {  myText : ref(22); } //不加ref无法做到响应
}

//子组件
//自定义修饰符 capitalize  通过modelModifiers prop 提供给组件 忽视any
 <input type="text" :value="modelValue" @input="emitValue" />
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({ capitalize: false })
    }
  },
  created() {
    console.log(this.modelModifiers); // { capitalize: true }
  },
  methods: {
    emitValue(e: any) {
      let value: any = e.target.value;
      if (this.modelModifiers.capitalize) {
        value = value.charAt(0).toUpperCase() + value.slice(1);
      }
      this.$emit("update:modelValue", value);
    }
  }  

//对于带参数的 v-model 绑定

 <vModel v-model:foo.capitalize="bar"></vModel>
   props: {
    foo: {
      type: String
    },
    fooModifiers: {
      type: String
    }
  }
  created() {
    console.log(this.fooModifiers); // { capitalize: true }
  }
  <input type="text" :value="foo" @input="$emit('update:foo', $event.target.value)" />


```



1. $parent,$children 获取当前组件的父组件和当前组件的子组件

2. $attrs 和$listeners A->B->C。Vue 2.4 开始提供了$attrs 和$listeners 来解决这个问题 (跨级)

   inheritAttrs：false 元素不会设置在父dom上，自定义给子元素

3. 父组件中通过 provide 来提供变量，然后在子组件中通过 inject 来注入变量。(官方不推荐在实际业务中使用，但是写组件库时很常用) (跨级)

4. $refs 获取组件实例

5. envetBus 兄弟组件数据传递 这种情况下可以使用事件总线的方式， 这种方法通过**一个空的 Vue实例作为中央事件总线（事件中心**），用它来触发事件和监听事件，从而实现任何组件间的通信，包括父子、隔代、兄弟组件

6. vuex 状态管理 (跨级)

#### 4 Vue 的生命周期方法有哪些 一般在哪一步发请求

**beforeCreate** 在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。在当前阶段 data、methods、computed 以及 watch 上的数据和方法都不能被访问

**created** 实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。这里没有$el,如果非要想与 Dom 进行交互，可以通过 vm.$nextTick 来访问 Dom

**beforeMount** 在挂载开始之前被调用：相关的 render 函数首次被调用。

**mounted** 在挂载完成后发生，在当前阶段，真实的 Dom 挂载完毕，数据完成双向绑定，可以访问到 Dom 节点

bug : 有时候挂载!==渲染完，需要使用nextTick 

**beforeUpdate** 数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁（patch）之前。可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程

**updated** 发生在更新完成之后，当前阶段组件 Dom 已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新，该钩子在服务器端渲染期间不被调用。

**beforeDestroy** 实例销毁之前调用。在这一步，实例仍然完全可用。我们可以在这时进行善后收尾工作，比如清除计时器。

**destroyed** Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

**activated** keep-alive 专属，组件被激活时调用

**deactivated** keep-alive 专属，组件被销毁时调用

> 异步请求在哪一步发起？

可以在钩子函数 created、beforeMount、mounted 中进行异步请求，因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。

如果异步请求不需要依赖 Dom 推荐在 created 钩子函数中调用异步请求，因为在 created 钩子函数中调用异步请求有以下优点：

- 能更快获取到服务端数据，减少页面 loading 时间；
- ssr 不支持 beforeMount 、mounted 钩子函数，所以放在 created 中有助于一致性；

#### Vue中组件生命周期调用顺序是什么样的？

- 组件的调用顺序都是先父后子,渲染完成的顺序是先子后父。
- 组件的销毁操作是先父后子，销毁完成的顺序是先子后父。

#### 5 v-if 和 v-show 的区别

v-if 在编译过程中会被转化成三元表达式,条件不满足时不渲染此节点。

v-show 会被编译成指令，条件不满足时控制样式将对应节点隐藏 （display:none）

**使用场景**

v-if 适用于在运行时很少改变条件，不需要频繁切换条件的场景

v-show 适用于需要非常频繁切换条件的场景

> 扩展补充：display:none、visibility:hidden 和 opacity:0 之间的区别？

![display.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/618888ae9baa4c3096479b1f61bb37f3~tplv-k3u1fbpfcp-watermark.image)

#### 6 说说 vue 内置指令

![内置指令.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b46ec8b051246858211c4c7ec129fb3~tplv-k3u1fbpfcp-watermark.image)

#### 7 怎样理解 Vue 的单向数据流

数据总是从父组件传到子组件，子组件没有权利修改父组件传过来的数据，只能请求父组件对原始数据进行修改。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。

> 注意：在子组件直接用 v-model 绑定父组件传过来的 prop 这样是不规范的写法 开发环境会报警告

如果实在要改变父组件的 prop 值 可以再 data 里面定义一个变量 并用 prop 的值初始化它 之后用$emit 通知父组件去修改

#### 8 computed 和 watch 的区别和运用的场景

computed 是计算属性，依赖其他属性计算值，并且 computed 的值有缓存，只有当计算值变化才会返回内容，它可以设置 getter 和 setter。

watch 监听到值的变化就会执行回调，在回调中可以进行一些逻辑操作。

计算属性一般用在模板渲染中，某个值是依赖了其它的响应式对象甚至是计算属性计算而来；而侦听属性适用于观测某个值的变化去完成一段复杂的业务逻辑

计算属性原理详解 [传送门](https://juejin.cn/post/6956407362085191717)

侦听属性原理详解 [传送门](https://juejin.cn/post/6954925963226382367)

#### 9 v-if 与 v-for 为什么不建议一起使用

v-for 和 v-if 不要在同一个标签中使用,因为解析时先解析 v-for 再解析 v-if。如果遇到需要同时使用时可以考虑写成计算属性的方式。

------

### 中等

#### 10 Vue2.0 响应式数据的原理/双向绑定原理

- Vue 数据双向绑定主要是指：**数据变化更新视图，视图变化更新数据**。其中，View变化更新Data，可以通过事件监听的方式来实现，所以 Vue数据双向绑定的工作主要是如何**根据Data变化更新View**。

- **简述**：

- - 当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property，并使用 Object.defineProperty 把这些 property 全部转为 getter/setter。
  - 这些 getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 能够追踪依赖，在 property 被访问和修改时通知变更。
  - 每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。

![图片](https://mmbiz.qpic.cn/mmbiz/1NOXMW586utTA28SC6pr4rnWUmBia9uUUQjDNOjGNv5IyUWRdTSmvmticWA5mDTSdcAn7TakwLLkziagrCVMqISXw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 深入理解：

- - **监听器 Observer**：对数据对象进行遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。
  - **解析器 Compile**：解析 Vue 模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。
  - **订阅者 Watcher**：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁 ，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新——这是一个典型的观察者模式
  - **订阅器 Dep**：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。



整体思路是数据劫持+观察者模式

对象内部通过 defineReactive 方法，使用 Object.defineProperty 将属性进行劫持（只会劫持已经存在的属性），数组则是通过重写数组方法来实现。当页面使用对应属性时，每个属性都拥有自己的 dep 属性，存放他所依赖的 watcher（依赖收集），当属性变化后会通知自己对应的 watcher 去更新(派发更新)。

相关代码如下

```javascript
class Observer {
  // 观测值
  constructor(value) {
    this.walk(value);
  }
  walk(data) {
    // 对象上的所有属性依次进行观测
    let keys = Object.keys(data);
    for (let i = 0; i < keys.length; i++) {
      let key = keys[i];
      let value = data[key];
      defineReactive(data, key, value);
    }
  }
}
// Object.defineProperty数据劫持核心 兼容性在ie9以及以上
function defineReactive(data, key, value) {
  observe(value); // 递归关键
  // --如果value还是一个对象会继续走一遍odefineReactive 层层遍历一直到value不是对象才停止
  //   思考？如果Vue数据嵌套层级过深 >>性能会受影响
  Object.defineProperty(data, key, {
    get() {
      console.log("获取值");

      //需要做依赖收集过程 这里代码没写出来
      return value;
    },
    set(newValue) {
      if (newValue === value) return;
      console.log("设置值");
      //需要做派发更新过程 这里代码没写出来
      value = newValue;
    },
  });
}
export function observe(value) {
  // 如果传过来的是对象或者数组 进行属性劫持
  if (
    Object.prototype.toString.call(value) === "[object Object]" ||
    Array.isArray(value)
  ) {
    return new Observer(value);
  }
}
复制代码
```

响应式数据原理详解 [传送门](https://juejin.cn/post/6935344605424517128)

#### 11 Vue 如何检测数组变化

数组考虑性能原因没有用 defineProperty 对数组的每一项进行拦截，而是选择对 7 种数组（push,shift,pop,splice,unshift,sort,reverse）方法进行重写(AOP 切片思想)

所以在 Vue 中修改数组的索引和长度是无法监控到的。需要通过以上 7 种变异方法修改数组才会触发数组对应的 watcher 进行更新

如果数组中包含着引用类型，会对数组中的引用类型再次递归遍历进行监控。这样就实现了监测数组变化。

相关代码如下

```javascript
// src/obserber/array.js
// 先保留数组原型
const arrayProto = Array.prototype;
// 然后将arrayMethods继承自数组原型
// 这里是面向切片编程思想（AOP）--不破坏封装的前提下，动态的扩展功能
export const arrayMethods = Object.create(arrayProto);
let methodsToPatch = [
  "push",
  "pop",
  "shift",
  "unshift",
  "splice",
  "reverse",
  "sort",
];
methodsToPatch.forEach((method) => {
  arrayMethods[method] = function (...args) {
    //   这里保留原型方法的执行结果
    const result = arrayProto[method].apply(this, args);
    // 这句话是关键
    // this代表的就是数据本身 比如数据是{a:[1,2,3]} 那么我们使用a.push(4)  this就是a  ob就是a.__ob__ 这个属性就是上段代码增加的 代表的是该数据已经被响应式观察过了指向Observer实例
    const ob = this.__ob__;

    // 这里的标志就是代表数组有新增操作
    let inserted;
    switch (method) {
      case "push":
      case "unshift":
        inserted = args;
        break;
      case "splice":
        inserted = args.slice(2);
      default:
        break;
    }
    // 如果有新增的元素 inserted是一个数组 调用Observer实例的observeArray对数组每一项进行观测
    if (inserted) ob.observeArray(inserted);
    // 之后咱们还可以在这里检测到数组改变了之后从而触发视图更新的操作--后续源码会揭晓
    return result;
  };
});
复制代码
```

数组的观测原理详解 [传送门](https://juejin.cn/post/6935344605424517128#heading-4)

#### 12 vue3.0 用过吗 了解多少

- 响应式原理的改变 Vue3.x 使用 Proxy 取代 Vue2.x 版本的 Object.defineProperty
- 组件选项声明方式 Vue3.x 使用 Composition API setup 是 Vue3.x 新增的一个选项， 他是组件内使用 Composition API 的入口。
- 模板语法变化 slot 具名插槽语法 自定义指令 v-model 升级
- 其它方面的更改 Suspense 支持 Fragment（多个根节点）和 Protal（在 dom 其他部分渲染组建内容）组件，针对一些特殊的场景做了处理。 基于 treeshaking 优化，提供了更多的内置功能。

Vue3.0 新特性以及使用经验总结 [传送门](https://juejin.cn/post/6940454764421316644)

#### 13 Vue3.0 和 2.0 的响应式原理区别

Vue3.x 改用 Proxy 替代 Object.defineProperty。因为 Proxy 可以直接监听对象和数组的变化，并且有多达 13 种拦截方法。

相关代码如下

```javascript
import { mutableHandlers } from "./baseHandlers"; // 代理相关逻辑
import { isObject } from "./util"; // 工具方法

export function reactive(target) {
  // 根据不同参数创建不同响应式对象
  return createReactiveObject(target, mutableHandlers);
}
function createReactiveObject(target, baseHandler) {
  if (!isObject(target)) {
    return target;
  }
  const observed = new Proxy(target, baseHandler);
  return observed;
}

const get = createGetter();
const set = createSetter();

function createGetter() {
  return function get(target, key, receiver) {
    // 对获取的值进行放射
    const res = Reflect.get(target, key, receiver);
    console.log("属性获取", key);
    if (isObject(res)) {
      // 如果获取的值是对象类型，则返回当前对象的代理对象
      return reactive(res);
    }
    return res;
  };
}
function createSetter() {
  return function set(target, key, value, receiver) {
    const oldValue = target[key];
    const hadKey = hasOwn(target, key);
    const result = Reflect.set(target, key, value, receiver);
    if (!hadKey) {
      console.log("属性新增", key, value);
    } else if (hasChanged(value, oldValue)) {
      console.log("属性值被修改", key, value);
    }
    return result;
  };
}
export const mutableHandlers = {
  get, // 当获取属性时调用此方法
  set, // 当修改属性时调用此方法
};
复制代码
```

#### 14 Vue 的父子组件生命周期钩子函数执行顺序

- 加载渲染过程

父 beforeCreate->父 created->父 beforeMount->子 beforeCreate->子 created->子 beforeMount->子 mounted->父 mounted

- 子组件更新过程

父 beforeUpdate->子 beforeUpdate->子 updated->父 updated

- 父组件更新过程

父 beforeUpdate->父 updated

- 销毁过程

父 beforeDestroy->子 beforeDestroy->子 destroyed->父 destroyed

#### 15 虚拟 DOM 是什么 有什么优缺点

由于在浏览器中操作 DOM 是很昂贵的。频繁的操作 DOM，会产生一定的性能问题。这就是虚拟 Dom 的产生原因。Vue2 的 Virtual DOM 借鉴了开源库 snabbdom 的实现。**Virtual DOM 本质就是用一个原生的 JS 对象去描述一个 DOM 节点，是对真实 DOM 的一层抽象**。



- 虚拟 DOM 的实现原理主要包括以下 3 部分：

- - 用 JavaScript 对象模拟真实 DOM 树，对真实 DOM 进行抽象；
  - diff 算法 — 比较两棵虚拟 DOM 树的差异；
  - pach 算法 — 将两个虚拟 DOM 对象的差异应用到真正的 DOM 树。

- key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速

- - 更准确：因为带 key 就不是就地复用了，在 sameNode 函数a.key === b.key对比中可以避免就地复用的情况。所以会更加准确。
  - 更快速：利用 key 的唯一性生成 map 对象来获取对应节点，比遍历方式更快

- 

**优点：**

1. 保证性能下限： 框架的虚拟 DOM 需要适配任何上层 API 可能产生的操作，它的一些 DOM 操作的实现必须是普适的，所以它的性能并不是最优的；但是比起粗暴的 DOM 操作性能要好很多，因此框架的虚拟 DOM 至少可以保证在你不需要手动优化的情况下，依然可以提供还不错的性能，即保证性能的下限；
2. 无需手动操作 DOM： 我们不再需要手动去操作 DOM，只需要写好 View-Model 的代码逻辑，框架会根据虚拟 DOM 和 数据双向绑定，帮我们以可预期的方式更新视图，极大提高我们的开发效率；
3. 跨平台： 虚拟 DOM 本质上是 JavaScript 对象,而 DOM 与平台强相关，相比之下虚拟 DOM 可以进行更方便地跨平台操作，例如服务器渲染、weex 开发等等。

**缺点:**

1. 无法进行极致优化： 虽然虚拟 DOM + 合理的优化，足以应对绝大部分应用的性能需求，但在一些性能要求极高的应用中虚拟 DOM 无法进行针对性的极致优化。
2. 首次渲染大量 DOM 时，由于多了一层虚拟 DOM 的计算，会比 innerHTML 插入慢。

#### 为什么不建议用index作为key?

- 不建议 用index 作为 key，和没写基本上没区别，因为不管你数组的顺序怎么颠倒，index 都是 0, 1, 2 这样排列，导致 Vue 会复用错误的旧子节点，做很多额外的工作

#### 16 v-model 原理

- v-model是用来在表单控件或者组件上创建双向绑定的
- 他的本质是v-bind和v-on的语法糖
- 在一个组件上使用v-model，默认会为组件绑定名为value的prop和名为input的事件



v-model 只是语法糖而已

v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 value property 和 input 事件；
- checkbox 和 radio 使用 checked property 和 change 事件；
- select 字段将 value 作为 prop 并将 change 作为事件。

> 注意:对于需要使用输入法 (如中文、日文、韩文等) 的语言，你会发现 v-model 不会在输入法组合文字过程中得到更新。

在普通标签上

```javascript
    <input v-model="sth" />  //这一行等于下一行
    <input v-bind:value="sth" v-on:input="sth = $event.target.value" />
复制代码
```

在组件上

```html
<currency-input v-model="price"></currentcy-input>
<!--上行代码是下行的语法糖
 <currency-input :value="price" @input="price = arguments[0]"></currency-input>
-->

<!-- 子组件定义 -->
Vue.component('currency-input', {
 template: `
  <span>
   <input
    ref="input"
    :value="value"
    @input="$emit('input', $event.target.value)"
   >
  </span>
 `,
 props: ['value'],
})

复制代码
```

#### 17 v-for 为什么要加 key

如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速

**更准确**：因为带 key 就不是就地复用了，在 sameNode 函数 a.key === b.key 对比中可以避免就地复用的情况。所以会更加准确。

**更快速**：利用 key 的唯一性生成 map 对象来获取对应节点，比遍历方式更快

相关代码如下

```javascript
// 判断两个vnode的标签和key是否相同 如果相同 就可以认为是同一节点就地复用
function isSameVnode(oldVnode, newVnode) {
  return oldVnode.tag === newVnode.tag && oldVnode.key === newVnode.key;
}

// 根据key来创建老的儿子的index映射表  类似 {'a':0,'b':1} 代表key为'a'的节点在第一个位置 key为'b'的节点在第二个位置
function makeIndexByKey(children) {
  let map = {};
  children.forEach((item, index) => {
    map[item.key] = index;
  });
  return map;
}
// 生成的映射表
let map = makeIndexByKey(oldCh);
复制代码
```

diff 算法详解 [传送门](https://juejin.cn/post/6953433215218483236)

#### 18 Vue 事件绑定原理

**原生事件绑定是通过addEventListener绑定给真实元素的，组件事件绑定是通过Vue自定义的`$on`实现的。**

原生事件绑定是通过 addEventListener 绑定给真实元素的，组件事件绑定是通过 Vue 自定义的$on 实现的。如果要在组件上使用原生事件，需要加.native 修饰符，这样就相当于在父组件中把子组件当做普通 html 标签，然后加上原生事件。

$on、$emit 是基于发布订阅模式的，维护一个事件中心，on 的时候将事件按名称存在事件中心里，称之为订阅者，然后 emit 将对应的事件进行发布，去执行事件中心里的对应的监听器

手写发布订阅原理 [传送门](https://juejin.cn/post/6844904153437700103#heading-2)

#### 19 vue-router 路由钩子函数是什么 执行顺序是什么

路由钩子的执行流程, 钩子函数种类有:全局守卫、路由守卫、组件守卫

- 全局前置/钩子：beforeEach、beforeResolve、afterEach
- 路由独享的守卫：beforeEnter
- 组件内的守卫：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave

**完整的导航解析流程:**

1. 导航被触发。
2. 在失活的组件里调用 beforeRouteLeave 守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。

#### 路由懒加载是什么意思？如何实现路由懒加载？

- 路由懒加载的含义：把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件

```javascript
- 实现：结合 Vue 的异步组件和 Webpack 的代码分割功能

   1. 可以将异步组件定义为返回一个 Promise 的工厂函数 \(该函数返回的 Promise 应该 resolve 组件本身\)
  

  const Foo = () => Promise.resolve({ /* 组件定义对象 */ }) 


   2. 在 Webpack 2 中，我们可以使用动态 import语法来定义代码分块点 \(split point\)


  import('./Foo.vue') // 返回 Promise 

- 结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件

  const Foo = () => import('./Foo.vue') const router = new VueRouter({ routes: [ { path: '/foo', component: Foo } ]}) 
 

- 使用命名 chunk，和webpack中的魔法注释就可以把某个路由下的所有组件都打包在同个异步块 (chunk) 中


chunkconst Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue') 

```



#### 20 vue-router 动态路由是什么 有什么问题

我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在 vue-router 的路由路径中使用“动态路径参数”(dynamic segment) 来达到这个效果：

```javascript
const User = {
  template: "<div>User</div>",
};

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: "/user/:id", component: User },
  ],
});
复制代码
```

> 问题:vue-router 组件复用导致路由参数失效怎么办？

解决方法：

1.通过 watch 监听路由参数再发请求

```javascript
watch: { //通过watch来监听路由变化

 "$route": function(){
 this.getData(this.$route.params.xxx);
 }
}
复制代码
```

2.用 :key 来阻止“复用”

```html
<router-view :key="$route.fullPath" />
复制代码
```

#### 21 谈一下对 vuex 的个人理解

> 运用到了js设计模式中的单例模式，单例模式想要做到的是，不管我们尝试去创建多少次，它都只给你返回第一次所创建的那唯一的一个实例。

vuex 是专门为 vue 提供的全局状态管理系统，用于多个组件中数据共享、数据缓存等。（无法持久化、内部核心原理是通过创造一个全局实例 new Vue）

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 store（仓库）。“store” 基本上就是一个容器，它包含着你的应用中大部分的状态 ( state )。

- - Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
  - 改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化。

![vuex.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb545e2edc0a4dcb94a412db0625799c~tplv-k3u1fbpfcp-watermark.image) 主要包

> Vuex 使用单一状态树，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 (SSOT)”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。——Vuex官方文档

括以下几个模块：

- State：定义了应用状态的数据结构，可以在这里设置默认的初始状态。
- Getter：允许组件从 Store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。
- Mutation：是唯一更改 store 中状态的方法，且必须是同步函数。
- Action：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。
- Module：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中。

#### 什么情况下使用 Vuex？

- 如果应用够简单，最好不要使用 Vuex，一个简单的 store 模式即可
- 需要构建一个中大型单页应用时，使用Vuex能更好地在组件外部管理状态

#### Vuex和单纯的全局对象有什么区别？

- Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
- 不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

#### 为什么 Vuex 的 mutation 中不能做异步操作？

- Vuex中所有的状态更新的唯一途径都是mutation，异步操作通过 Action 来提交 mutation实现，这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。
- 每个mutation执行完成后都会对应到一个新的状态变更，这样devtools就可以打个快照存下来，然后就可以实现 time-travel 了。如果mutation支持异步操作，就没有办法知道状态是何时更新的，无法很好的进行状态的追踪，给调试带来困难。

#### vuex的action有返回值吗？返回的是什么？

- store.dispatch 可以处理被触发的 action 的处理函数返回的 Promise，并且 store.dispatch 仍旧返回 Promise
- Action 通常是异步的，要知道 action 什么时候结束或者组合多个 action以处理更加复杂的异步流程，可以通过定义action时返回一个promise对象，就可以在派发action的时候就可以通过处理返回的 Promise处理异步流程

> 一个 store.dispatch 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。

#### 为什么不直接分发mutation,而要通过分发action之后提交 mutation变更状态

- mutation 必须同步执行，我们可以在 action 内部执行异步操作
- 可以进行一系列的异步操作，并且通过提交 mutation 来记录 action 产生的副作用（即状态变更）

#### 22 Vuex 页面刷新数据丢失怎么解决

需要做 vuex 数据持久化 一般使用本地存储的方案来保存数据 可以自己设计存储方案 也可以使用第三方插件

推荐使用 vuex-persist 插件，它就是为 Vuex 持久化存储而生的一个插件。不需要你手动存取 storage ，而是直接将状态保存至 cookie 或者 localStorage 中

#### 23 Vuex 为什么要分模块并且加命名空间

**模块**:由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块。

**命名空间**：默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的——这样使得多个模块能够对同一 mutation 或 action 作出响应。如果希望你的模块具有更高的封装度和复用性，你可以通过添加 namespaced: true 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。

#### 24 使用过 Vue SSR 吗？说说 SSR

SSR 也就是服务端渲染，也就是将 Vue 在客户端把标签渲染成 HTML 的工作放在服务端完成，然后再把 html 直接返回给客户端。

**优点：**

SSR 有着更好的 SEO、并且首屏加载速度更快

**缺点：** 开发条件会受到限制，服务器端渲染只支持 beforeCreate 和 created 两个钩子，当我们需要一些外部扩展库时需要特殊处理，服务端渲染应用程序也需要处于 Node.js 的运行环境。

服务器会有更大的负载需求

#### 25 vue 中使用了哪些设计模式

1.工厂模式 - 传入参数即可创建实例

虚拟 DOM 根据参数的不同返回基础标签的 Vnode 和组件 Vnode

2.单例模式 - 整个程序有且仅有一个实例

vuex 和 vue-router 的插件注册方法 install 判断如果系统存在实例就直接返回掉

3.发布-订阅模式 (vue 事件机制)

4.观察者模式 (响应式数据原理)

5.装饰模式: (@装饰器的用法)

6.策略模式 策略模式指对象有某个行为,但是在不同的场景中,该行为有不同的实现方案-比如选项的合并策略

...其他模式欢迎补充

#### 26 你都做过哪些 Vue 的性能优化

> 这里只列举针对 Vue 的性能优化 整个项目的性能优化是一个大工程 可以另写一篇性能优化的文章 哈哈

- 对象层级不要过深，否则性能就会差
- 尽量减少data中的数据，data中的数据都会增加getter和setter，会收集对应的watcher
- 不需要响应式的数据不要放到 data 中（可以用 Object.freeze() 冻结数据）
- v-if 和 v-show 区分使用场景 在更多的情况下，使用v-if替代v-show
- computed 和 watch 区分使用场景
- v-for 遍历必须加 key，且避免同时使用 v-if
- key保证唯一，key 最好是 id 值
- 大数据列表和表格性能优化-虚拟列表/虚拟表格
- 防止内部泄漏，组件销毁后把全局变量和事件销毁
- 图片懒加载
- 路由懒加载
- 第三方插件的按需引入
- SPA 页面采用 keep-alive 缓存组件
- 防抖、节流运用
- 服务端渲染 SSR or 预渲染
- 压缩代码
- Tree Shaking/Scope Hoisting
- 使用cdn加载第三方模块
- 多线程打包happypack
- splitChunks抽离公共文件
- sourceMap优化
- 骨架屏
- PWA
- 还可以使用缓存(客户端缓存、服务端缓存)优化、服务端开启gzip压缩等。

#### 说说你对 SPA 单页面的理解，它的优缺点分别是什么？

- SPA（ single-page application ）仅在 Web 页面初始化时加载相应的 HTML、JavaScript 和 CSS。一旦页面加载完成，SPA 不会因为用户的操作而进行页面的重新加载或跳转；取而代之的是利用路由机制实现 HTML 内容的变换，UI 与用户的交互，避免页面的重新加载。

- 优点：

- - 用户体验好、快，内容的改变不需要重新加载整个页面，避免了不必要的跳转和重复渲染；
  - 基于上面一点，SPA 相对对服务器压力小；
  - 前后端职责分离，架构清晰，前端进行交互逻辑，后端负责数据处理；

- 缺点：

- - 初次加载耗时多：为实现单页 Web 应用功能及显示效果，需要在加载页面的时候将 JavaScript、CSS 统一加载，部分页面按需加载；
  - 前进后退路由管理：由于单页应用在一个页面中显示所有的内容，所以不能使用浏览器的前进后退功能，所有的页面切换需要自己建立堆栈管理；
  - SEO 难度较大：由于所有的内容都在一个页面中动态替换显示，所以在 SEO 上其有着天然的弱势。

------

### 困难

#### 说说vue和react的异同

- 同

- - 使用 Virtual DOM
  - 提供了响应式 (Reactive) 和组件化 (Composable) 的视图组件。
  - 将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库。

- 异

- - 在 React 应用中，当某个组件的状态发生变化时，它会以该组件为根，重新渲染整个组件子树（除非使用PureComponent/shouldComponentUpdate），在 Vue 应用中，组件的依赖是在渲染过程中自动追踪的，所以系统能精确知晓哪个组件确实需要被重渲染
  - 在 React 中，一切都是 JavaScript。不仅仅是 HTML 可以用 JSX 来表达，现在的潮流也越来越多地将 CSS 也纳入到 JavaScript 中来处理
  - Vue 的路由库和状态管理库都是由官方维护支持且与核心库同步更新的。React 则是选择把这些问题交给社区维护，因此创建了一个更分散的生态系统，所以有更丰富的生态系统
  - Vue 提供了CLI 脚手架，能让你通过交互式的脚手架引导非常容易地构建项目。你甚至可以使用它快速开发组件的原型。React 在这方面也提供了create-react-app，但是现在还存在一些局限性
  - React Native 能使你用相同的组件模型编写有本地渲染能力的 APP，Vue 和Weex会进行官方合作，Weex 是阿里巴巴发起的跨平台用户界面开发框架，同时也正在 Apache 基金会进行项目孵化，另一个选择是NativeScript-Vue，一个用 Vue.js 构建完全原生应用的NativeScript插件

#### 27 Vue.mixin 的使用场景和原理

##### 什么是 mixin ？

- Mixin 使我们能够为 Vue 组件编写可插拔和可重用的功能。
- 如果你希望再多个组件之间重用一组组件选项，例如生命周期 hook、 方法等，则可以将其编写为 mixin，并在组件中简单的引用它。
- 然后将 mixin 的内容合并到组件中。如果你要在 mixin 中定义生命周期 hook，那么它在执行时将优化于组件自已的 hook。

在日常的开发中，我们经常会遇到在不同的组件中经常会需要用到一些相同或者相似的代码，这些代码的功能相对独立，可以通过 Vue 的 mixin 功能抽离公共的业务逻辑，原理类似“对象的继承”，当组件初始化时会调用 mergeOptions 方法进行合并，采用策略模式针对不同的属性进行合并。当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。

相关代码如下

```javascript
export default function initMixin(Vue){
  Vue.mixin = function (mixin) {
    //   合并对象
      this.options=mergeOptions(this.options,mixin)
  };
}
};

// src/util/index.js
// 定义生命周期
export const LIFECYCLE_HOOKS = [
  "beforeCreate",
  "created",
  "beforeMount",
  "mounted",
  "beforeUpdate",
  "updated",
  "beforeDestroy",
  "destroyed",
];

// 合并策略
const strats = {};
// mixin核心方法
export function mergeOptions(parent, child) {
  const options = {};
  // 遍历父亲
  for (let k in parent) {
    mergeFiled(k);
  }
  // 父亲没有 儿子有
  for (let k in child) {
    if (!parent.hasOwnProperty(k)) {
      mergeFiled(k);
    }
  }

  //真正合并字段方法
  function mergeFiled(k) {
    if (strats[k]) {
      options[k] = strats[k](parent[k], child[k]);
    } else {
      // 默认策略
      options[k] = child[k] ? child[k] : parent[k];
    }
  }
  return options;
}
复制代码
```

Vue.mixin 原理详解 [传送门](https://juejin.cn/post/6951671158198501383)

#### 28 nextTick 使用场景和原理

- nextTick 中的回调是在下次 DOM 更新循环结束之后执行的延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。主要思路就是采用微任务优先的方式调用异步方法去执行 nextTick 包装的方法。
- nextTick主要使用了**宏任务**和**微任务**。
- 根据执行环境分别尝试采用Promise、MutationObserver、setImmediate，如果以上都不行则采用setTimeout定义了一个异步方法，多次调用nextTick会将方法存入队列中，通过这个异步方法清空当前队列。



相关代码如下

```javascript
let callbacks = [];
let pending = false;
function flushCallbacks() {
  pending = false; //把标志还原为false
  // 依次执行回调
  for (let i = 0; i < callbacks.length; i++) {
    callbacks[i]();
  }
}
let timerFunc; //定义异步方法  采用优雅降级
if (typeof Promise !== "undefined") {
  // 如果支持promise
  const p = Promise.resolve();
  timerFunc = () => {
    p.then(flushCallbacks);
  };
} else if (typeof MutationObserver !== "undefined") {
  // MutationObserver 主要是监听dom变化 也是一个异步方法
  let counter = 1;
  const observer = new MutationObserver(flushCallbacks);
  const textNode = document.createTextNode(String(counter));
  observer.observe(textNode, {
    characterData: true,
  });
  timerFunc = () => {
    counter = (counter + 1) % 2;
    textNode.data = String(counter);
  };
} else if (typeof setImmediate !== "undefined") {
  // 如果前面都不支持 判断setImmediate
  timerFunc = () => {
    setImmediate(flushCallbacks);
  };
} else {
  // 最后降级采用setTimeout
  timerFunc = () => {
    setTimeout(flushCallbacks, 0);
  };
}

export function nextTick(cb) {
  // 除了渲染watcher  还有用户自己手动调用的nextTick 一起被收集到数组
  callbacks.push(cb);
  if (!pending) {
    // 如果多次调用nextTick  只会执行一次异步 等异步队列清空之后再把标志变为false
    pending = true;
    timerFunc();
  }
}
复制代码
```

nextTick 原理详解 [传送门](https://juejin.cn/post/6939704519668432910#heading-4)

Vue是异步执⾏dom更新的，⼀旦观察到数据变化，Vue就会开启⼀个**异步队列**，然后把在同

⼀个事件循环 (event loop) 当中观察到数据变化的 watcher 推送进这个队列。如果这个

watcher被触发多次，只会被推送到队列⼀次。

这种缓冲⾏为可以有效的去掉重复数据造成的不必要的计算和DOm操作。⽽在下⼀个事件循

环时，Vue会清空队列，并进⾏必要的DOM更新。

⽽vue内部这个异步队列是怎么开启的? 这⾥有⼀个优先级, Promise.then > 

MutationObserver > setImmediate > setTimeout

所以可以理解为, nextTick会优先尝试使⽤微任务, 如果浏览器不⽀持, 就⽤宏任务.当你设置 vm.someData = ‘new value’，DOM 并不会⻢上更新，⽽是在异步队列被清除，也

就是下⼀个事件循环开始时执⾏更新时才会进⾏必要的DOM更新.

所以nextTick的回调是在下⼀轮事件循环⾥执⾏的.

⼀般在什么时候⽤到nextTick呢? 

1. 在数据变化后要执⾏的某个操作，⽽这个操作需要使⽤随数据改变⽽改变的DOM结构的时

   候，这个操作都应该放进Vue.nextTick()的回调函数中

   ```javascript
   <template>
    <div v-if="loaded" ref="test"></div>
   </template>
   async showDiv() {
    this.loaded = true;
    //this.$refs.test.xxxxx;  拿不到
    await Vue.nextTick();
    this.$refs.test.xxxxx; 
   }
   ```

   

#### 29 keep-alive 使用场景和原理

keep-alive 是 Vue 内置的一个组件，可以实现组件缓存，当组件切换时不会对当前组件进行卸载。

- 常用的两个属性 include/exclude，允许组件有条件的进行缓存。两者都支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，其中 exclude 的优先级比 include 高；
- 两个生命周期 activated/deactivated，用来得知当前组件是否处于活跃状态。当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated。
- keep-alive 的中还运用了 LRU(最近最少使用) 算法，选择最近最久未使用的组件予以淘汰。

相关代码如下

```javascript
export default {
  name: "keep-alive",
  abstract: true, //抽象组件

  props: {
    include: patternTypes, //要缓存的组件
    exclude: patternTypes, //要排除的组件
    max: [String, Number], //最大缓存数
  },

  created() {
    this.cache = Object.create(null); //缓存对象  {a:vNode,b:vNode}
    this.keys = []; //缓存组件的key集合 [a,b]
  },

  destroyed() {
    for (const key in this.cache) {
      pruneCacheEntry(this.cache, key, this.keys);
    }
  },

  mounted() {
    //动态监听include  exclude
    this.$watch("include", (val) => {
      pruneCache(this, (name) => matches(val, name));
    });
    this.$watch("exclude", (val) => {
      pruneCache(this, (name) => !matches(val, name));
    });
  },

  render() {
    const slot = this.$slots.default; //获取包裹的插槽默认值
    const vnode: VNode = getFirstComponentChild(slot); //获取第一个子组件
    const componentOptions: ?VNodeComponentOptions =
      vnode && vnode.componentOptions;
    if (componentOptions) {
      // check pattern
      const name: ?string = getComponentName(componentOptions);
      const { include, exclude } = this;
      // 不走缓存
      if (
        // not included  不包含
        (include && (!name || !matches(include, name))) ||
        // excluded  排除里面
        (exclude && name && matches(exclude, name))
      ) {
        //返回虚拟节点
        return vnode;
      }

      const { cache, keys } = this;
      const key: ?string =
        vnode.key == null
          ? // same constructor may get registered as different local components
            // so cid alone is not enough (#3269)
            componentOptions.Ctor.cid +
            (componentOptions.tag ? `::${componentOptions.tag}` : "")
          : vnode.key;
      if (cache[key]) {
        //通过key 找到缓存 获取实例
        vnode.componentInstance = cache[key].componentInstance;
        // make current key freshest
        remove(keys, key); //通过LRU算法把数组里面的key删掉
        keys.push(key); //把它放在数组末尾
      } else {
        cache[key] = vnode; //没找到就换存下来
        keys.push(key); //把它放在数组末尾
        // prune oldest entry  //如果超过最大值就把数组第0项删掉
        if (this.max && keys.length > parseInt(this.max)) {
          pruneCacheEntry(cache, keys[0], keys, this._vnode);
        }
      }

      vnode.data.keepAlive = true; //标记虚拟节点已经被缓存
    }
    // 返回虚拟节点
    return vnode || (slot && slot[0]);
  },
};
复制代码
```

> 扩展补充：LRU 算法是什么？

![lrusuanfa.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa4a49bd77234467a53cd3b62c5bd135~tplv-k3u1fbpfcp-watermark.image)

LRU 的核心思想是如果数据最近被访问过，那么将来被访问的几率也更高，所以我们将命中缓存的组件 key 重新插入到 this.keys 的尾部，这样一来，this.keys 中越往头部的数据即将来被访问几率越低，所以当缓存数量达到最大值时，我们就删除将来被访问几率最低的数据，即 this.keys 中第一个缓存的组件。

#### 30 Vue.set 方法原理

了解 Vue 响应式原理的同学都知道在两种情况下修改数据 Vue 是不会触发视图更新的

1.在实例创建之后添加新的属性到实例上（给响应式对象新增属性）

2.直接更改数组下标来修改数组的值

Vue.set 或者说是$set 原理如下

因为响应式数据 我们给对象和数组本身都增加了__ob__属性，代表的是 Observer 实例。当给对象新增不存在的属性 首先会把新的属性进行响应式跟踪 然后会触发对象__ob__的 dep 收集到的 watcher 去更新，当修改数组索引时我们调用数组本身的 splice 方法去更新数组

- vm.`$set` 的实现原理是：
- 如果目标是数组，直接使用数组的 splice 方法触发相应式；
- 如果目标是对象，会先判读属性是否存在、对象是否是响应式，最终如果要对属性进行响应式处理，则是通过调用 defineReactive 方法进行响应式处理（ defineReactive 方法就是 Vue 在初始化对象时，给对象属性采用 Object.defineProperty 动态添加 getter 和 setter 的功能所调用的方法）

相关代码如下

```typescript
export function set(target: Array | Object, key: any, val: any): any {
  // 如果是数组 调用我们重写的splice方法 (这样可以更新视图)
  if (Array.isArray(target) && isValidArrayIndex(key)) {
    target.length = Math.max(target.length, key);
    target.splice(key, 1, val);
    return val;
  }
  // 如果是对象本身的属性，则直接添加即可
  if (key in target && !(key in Object.prototype)) {
    target[key] = val;
    return val;
  }
  const ob = (target: any).__ob__;

  // 如果不是响应式的也不需要将其定义成响应式属性
  if (!ob) {
    target[key] = val;
    return val;
  }
  // 将属性定义成响应式的
  defineReactive(ob.value, key, val);
  // 通知视图更新
  ob.dep.notify();
  return val;
}
复制代码
```

响应式数据原理详解 [传送门](https://juejin.cn/post/6935344605424517128)

#### 31 Vue.extend 作用和原理

官方解释：Vue.extend 使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。

其实就是一个子类构造器 是 Vue 组件的核心 api 实现思路就是使用原型继承的方法返回了 Vue 的子类 并且利用 mergeOptions 把传入组件的 options 和父类的 options 进行了合并

相关代码如下

```javascript
export default function initExtend(Vue) {
  let cid = 0; //组件的唯一标识
  // 创建子类继承Vue父类 便于属性扩展
  Vue.extend = function (extendOptions) {
    // 创建子类的构造函数 并且调用初始化方法
    const Sub = function VueComponent(options) {
      this._init(options); //调用Vue初始化方法
    };
    Sub.cid = cid++;
    Sub.prototype = Object.create(this.prototype); // 子类原型指向父类
    Sub.prototype.constructor = Sub; //constructor指向自己
    Sub.options = mergeOptions(this.options, extendOptions); //合并自己的options和父类的options
    return Sub;
  };
}
复制代码
```

Vue 组件原理详解 [传送门](https://juejin.cn/post/6954173708344770591)

#### 32 写过自定义指令吗 原理是什么

指令本质上是装饰器，是 vue 对 HTML 元素的扩展，给 HTML 元素增加自定义功能。vue 编译 DOM 时，会找到指令对象，执行指令的相关方法。

自定义指令有五个生命周期（也叫钩子函数），分别是 bind、inserted、update、componentUpdated、unbind

```
1. bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。

2. inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

3. update：被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。

4. componentUpdated：被绑定元素所在模板完成一次更新周期时调用。

5. unbind：只调用一次，指令与元素解绑时调用。
复制代码
```

**原理**

1.在生成 ast 语法树时，遇到指令会给当前元素添加 directives 属性

2.通过 genDirectives 生成指令代码

3.在 patch 前将指令的钩子提取到 cbs 中,在 patch 过程中调用对应的钩子

4.当执行指令对应钩子函数时，调用对应指令定义的方法

#### 33 Vue 修饰符有哪些

**事件修饰符**

- .stop 阻止事件继续传播
- .prevent 阻止标签默认行为
- .capture 使用事件捕获模式,即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理
- .self 只当在 event.target 是当前元素自身时触发处理函数
- .once 事件将只会触发一次
- .passive 告诉浏览器你不想阻止事件的默认行为

**v-model 的修饰符**

- .lazy 通过这个修饰符，转变为在 change 事件再同步
- .number 自动将用户的输入值转化为数值类型
- .trim 自动过滤用户输入的首尾空格

**键盘事件的修饰符**

- .enter
- .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right

**系统修饰键**

- .ctrl
- .alt
- .shift
- .meta

**鼠标按钮修饰符**

- .left
- .right
- .middle

#### 34 Vue 模板编译原理

简单说，Vue的编译过程就是将template转化为render函数的过程。会经历以下阶段（生成AST树/优化/codegen）：

- 首先解析模版，生成AST语法树(一种用JavaScript对象的形式来描述整个模板)。使用大量的正则表达式对模板进行解析，遇到标签、文本的时候都会执行对应的钩子进行相关处理。
- Vue的数据是响应式的，但其实模板中并不是所有的数据都是响应式的。有一些数据首次渲染后就不会再变化，对应的DOM也不会变化。那么优化过程就是深度遍历AST树，按照相关条件对树节点进行标记。这些被标记的节点(静态节点)我们就可以跳过对它们的比对，对运行时的模板起到很大的优化作用。
- 编译的最后一步是将优化后的AST树转换为可执行的代码。



Vue 的编译过程就是将 template 转化为 render 函数的过程 分为以下三步

```
第一步是将 模板字符串 转换成 element ASTs（解析器）
第二步是对 AST 进行静态节点标记，主要用来做虚拟DOM的渲染优化（优化器）
第三步是 使用 element ASTs 生成 render 函数代码字符串（代码生成器）
复制代码
```

相关代码如下

```javascript
export function compileToFunctions(template) {
  // 我们需要把html字符串变成render函数
  // 1.把html代码转成ast语法树  ast用来描述代码本身形成树结构 不仅可以描述html 也能描述css以及js语法
  // 很多库都运用到了ast 比如 webpack babel eslint等等
  let ast = parse(template);
  // 2.优化静态节点
  // 这个有兴趣的可以去看源码  不影响核心功能就不实现了
  //   if (options.optimize !== false) {
  //     optimize(ast, options);
  //   }

  // 3.通过ast 重新生成代码
  // 我们最后生成的代码需要和render函数一样
  // 类似_c('div',{id:"app"},_c('div',undefined,_v("hello"+_s(name)),_c('span',undefined,_v("world"))))
  // _c代表创建元素 _v代表创建文本 _s代表文Json.stringify--把对象解析成文本
  let code = generate(ast);
  //   使用with语法改变作用域为this  之后调用render函数可以使用call改变this 方便code里面的变量取值
  let renderFn = new Function(`with(this){return ${code}}`);
  return renderFn;
}
复制代码
```

模板编译原理详解 [传送门](https://juejin.cn/post/6936024530016010276)

#### 35 生命周期钩子是如何实现的

Vue 的生命周期钩子核心实现是利用发布订阅模式先把用户传入的的生命周期钩子订阅好（内部采用数组的方式存储）然后在创建组件实例的过程中会一次执行对应的钩子方法（发布）

相关代码如下

```javascript
export function callHook(vm, hook) {
  // 依次执行生命周期对应的方法
  const handlers = vm.$options[hook];
  if (handlers) {
    for (let i = 0; i < handlers.length; i++) {
      handlers[i].call(vm); //生命周期里面的this指向当前实例
    }
  }
}

// 调用的时候
Vue.prototype._init = function (options) {
  const vm = this;
  vm.$options = mergeOptions(vm.constructor.options, options);
  callHook(vm, "beforeCreate"); //初始化数据之前
  // 初始化状态
  initState(vm);
  callHook(vm, "created"); //初始化数据之后
  if (vm.$options.el) {
    vm.$mount(vm.$options.el);
  }
};
复制代码
```

生命周期实现详解 [传送门](https://juejin.cn/post/6951671158198501383#heading-4)

#### 36 函数式组件使用场景和原理

函数式组件与普通组件的区别

```
1.函数式组件需要在声明组件是指定 functional:true
2.不需要实例化，所以没有this,this通过render函数的第二个参数context来代替
3.没有生命周期钩子函数，不能使用计算属性，watch
4.不能通过$emit 对外暴露事件，调用事件只能通过context.listeners.click的方式调用外部传入的事件
5.因为函数式组件是没有实例化的，所以在外部通过ref去引用组件时，实际引用的是HTMLElement
6.函数式组件的props可以不用显示声明，所以没有在props里面声明的属性都会被自动隐式解析为prop,而普通组件所有未声明的属性都解析到$attrs里面，并自动挂载到组件根元素上面(可以通过inheritAttrs属性禁止)
复制代码
```

优点 1.由于函数式组件不需要实例化，无状态，没有生命周期，所以渲染性能要好于普通组件 2.函数式组件结构比较简单，代码结构更清晰

使用场景：

一个简单的展示组件，作为容器组件使用 比如 router-view 就是一个函数式组件

“高阶组件”——用于接收一个组件作为参数，返回一个被包装过的组件

相关代码如下

```javascript
if (isTrue(Ctor.options.functional)) {
  // 带有functional的属性的就是函数式组件
  return createFunctionalComponent(Ctor, propsData, data, context, children);
}
const listeners = data.on;
data.on = data.nativeOn;
installComponentHooks(data); // 安装组件相关钩子 （函数式组件没有调用此方法，从而性能高于普通组件）
复制代码
```

#### 37 能说下 vue-router 中常用的路由模式实现原理吗

**hash 模式**

1. location.hash 的值实际就是 URL 中#后面的东西 它的特点在于：hash 虽然出现 URL 中，但不会被包含在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。
2. 可以为 hash 的改变添加监听事件

```javascript
window.addEventListener("hashchange", funcRef, false);
复制代码
```

每一次改变 hash（window.location.hash），都会在浏览器的访问历史中增加一个记录利用 hash 的以上特点，就可以来实现前端路由“更新视图但不重新请求页面”的功能了

> 特点：兼容性好但是不美观

**history 模式**

利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。

这两个方法应用于浏览器的历史记录站，在当前已有的 back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。这两个方法有个共同的特点：当调用他们修改浏览器历史记录栈后，虽然当前 URL 改变了，但浏览器不会刷新页面，这就为单页应用前端路由“更新视图但不重新请求页面”提供了基础。

这两个 API 可以在改变 url，但是不会发送请求。这样就可以监听 url 变化来实现更新页面部分内容的操作

> 特点：虽然美观，但是刷新会出现 404 需要后端进行配置

- 区别

- - url 展示上，hash 模式有“#”，history 模式没有
  - 刷新页面时，hash 模式可以正常加载到 hash 值对应的页面，而 history 没有处理的话，会返回 404，一般需要后端将所有页面都配置重定向到首页路由
  - 兼容性，hash 可以支持低版本浏览器和 IE。

#### 38 diff 算法了解吗

![diff算法.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9e3c68d1b0884d9ca0f8ffc5ee64a28e~tplv-k3u1fbpfcp-watermark.image)

建议直接看 diff 算法详解 [传送门](https://juejin.cn/post/6953433215218483236)

- 同级比较，再比较子节点
- 先判断一方有子节点一方没有子节点的情况(如果新的children没有子节点，将旧的子节点移除)
- 比较都有子节点的情况(核心diff)
- 递归比较子节点



# Vue3.0 

## Vue 3.0 性能提升主要是通过哪几方面体现的？

### 1.响应式系统提升

vue2在初始化的时候，对data中的每个属性使用definepropery调用getter和setter使之变为响应式对象。如果属性值为对象，还会递归调用defineproperty使之变为响应式对象。

vue3使用proxy对象重写响应式。proxy的性能本来比defineproperty好，proxy可以拦截属性的访问、赋值、删除等操作，不需要初始化的时候遍历所有属性，另外有多层属性嵌套的话，只有访问某个属性的时候，才会递归处理下一级的属性。

#### 优势

- 可以监听动态新增的属性；
- 可以监听删除的属性 ；
- 可以监听数组的索引和 length 属性；

通过Proxy方法封装了数据，来设置监听，在给变量赋值时会走到Proxy方法重的set方法，在set方法中修改完值去更新UI界面

```javascript
let obj = { name: 'Bob', age: 18 }
let state = new Proxy(obj,handler: {
	get(obj,key) {
		console.log(obj,key) // { name: 'Bob', age: 18 } name
		return obj[key]
	}
	set(obj, key, value) {
		console.log(obj,key,value) // { name: 'Bob', age: 18 } name 鲍勃
		obj[key] = value
		console.log('更新UI界面')
	}
})
console.log(state.name)
state.name = "鲍勃"
```


set方法必须通过返回值告诉Proxy此次操作是否成功

set方法必须通过返回值告诉Proxy此次操作是否成功

```javascript
let arr = [1,3,5]

let state = new Proxy(arr,handler: {
	get(obj,key) {
		console.log(obj,key) // [1,3,5] 1
		return obj[key]
	}
	set(obj, key, value) {
		console.log(obj,key,value)
		//  [1,3,5]  3  7   先将数组最后插入一个7
		//  [1,3,5,7] length 4  再将数组长度改为4
		obj[key] = value
		console.log('更新UI界面')
		return true     // 这个是一个返回值，告诉本次操作成功，才能执行再一次操作，因为修改数组时，第一步修改数组数据，第二步修改数组长度
	}
})

console.log(state[1]) // 3
state.push(7) // [1,3,5,7]
```



### 2. 编译优化

优化编译和重写虚拟dom，让首次渲染和更新dom性能有更大的提升 vue2 通过标记静态根节点,优化 diff 算法 vue3 标记和提升所有静态根节点,diff 的时候只比较动态节点内容

Fragments, 模板里面不用创建唯一根节点,可以直接放同级标签和文本内容

### 静态提升

patch flag, 跳过静态节点,直接对比动态节点,缓存事件处理函数

### 3. 源码体积的优化

vue3移除了一些不常用的api，例如：inline-template、filter等 使用tree-shaking



## Composition Api 与 Vue 2.x使用的Options Api 有什么区别？

### Options Api

包含一个描述组件选项（data、methods、props等）的对象 options；

API开发复杂组件，同一个功能逻辑的代码被拆分到不同选项 ；

使用mixin重用公用代码，也有问题：命名冲突，数据来源不清晰；

### composition Api

vue3 新增的一组 api，它是基于函数的 api，可以更灵活的组织组件的逻辑。

解决options api在大型项目中，options api不好拆分和重用的问题。



## Proxy 相对于 Object.defineProperty有哪些优点？

proxy的性能本来比defineproperty好，proxy可以拦截属性的访问、赋值、删除等操作，不需要初始化的时候遍历所有属性，另外有多层属性嵌套的话，只有访问某个属性的时候，才会递归处理下一级的属性。

- 可以* 监听数组变化
- 可以劫持整个对象
- 操作时不是对原对象操作,是 new Proxy 返回的一个新对象
- 可以劫持的操作有 13 种



- **Vue3.x改用Proxy替代Object.defineProperty**。

- 因为Proxy可以直接监听对象和数组的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。

- Proxy只会代理对象的第一层，Vue3是怎样处理这个问题的呢？

- - 判断当前Reflect.get的返回值是否为Object，如果是则再通过reactive方法做代理， 这样就实现了深度观测。
  - 监测数组的时候可能触发多次get/set，那么如何防止触发多次呢？我们可以判断key是否为当前被代理对象target自身属性，也可以判断旧值与新值是否相等，只有满足以上两个条件之一时，才有可能执行trigger。

### Proxy 与 Object.defineProperty 优劣对比

- Proxy 的优势如下:

- - Proxy 可以直接监听对象而非属性；

- Proxy 可以直接监听数组的变化；

- - Proxy 有多达 13 种拦截方法,不限于 apply、ownKeys、deleteProperty、has 等等是 Object.defineProperty 不具备的；
  - Proxy 返回的是一个新对象,我们可以只操作新的对象达到目的,而 Object.defineProperty 只能遍历对象属性直接修改；
  - Proxy 作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利；

- Object.defineProperty 的优势如下:

- - 兼容性好，支持 IE9，而 Proxy 的存在浏览器兼容性问题,而且无法用 polyfill 磨平，因此 Vue 的作者才声明需要等到下个大版本( 3.0 )才能用 Proxy 重写。

##  Vue 3.0 在编译方面有哪些优化？

vue.js 3.x中标记和提升所有的静态节点，diff的时候只需要对比动态节点内容；

Vue2中的虚拟DOM是进行全量对比；Vue3新增了静态标记【PatchFlag】；与上次虚拟节点对比，只对比带有静态标记的节点 并且通过flag的信息能得知当前要对比的内容

### hoist static 静态提升

```
+ vue2中无论元素是否参与更新，每次都会重新创建，然后渲染
+ vue3中对于不参与更新的元素，会做静态提升，只会被创建一次，在渲染时直接复用即可
```

### diff 算法

```
+ Vue2中的虚拟DOM是进行全量对比
+ Vue3新增了静态标记【PatchFlag】

说明：
    
与上次虚拟节点对比，只对比带有静态标记的节点
并且通过flag的信息能得知当前要对比的内容
```

### Fragments（升级vetur插件):

template中不需要唯一根节点，可以直接放文本或者同级标签

静态提升(hoistStatic),当使用 hoistStatic 时,所有静态的节点都被提升到 render 方法之外.只会在应用启动的时候被创建一次,之后使用只需要应用提取的静态节点，随着每次的渲染被不停的复用。

patch flag, 在动态标签末尾加上相应的标记,只能带 patchFlag 的节点才被认为是动态的元素,会被追踪属性的修改,能快速的找到动态节点,而不用逐个逐层遍历，提高了虚拟dom diff的性能。

缓存事件处理函数cacheHandler,避免每次触发都要重新生成全新的function去更新之前的函数 tree shaking 通过摇树优化核心库体积,减少不必要的代码量

**
**

## Vue.js 3.0 响应式系统的实现原理？

### 1. reactive

设置对象为响应式对象。接收一个参数，判断这参数是否是对象。不是对象则直接返回这个参数，不做响应式处理。创建拦截器handerler，设置get/set/deleteproperty。

#### 什么是reactive?

```
 reactive是vue3提供的一个响应式数据方法
 在vue2中通过defindProperty来实现的
 在vue3中响应式数据是通过ES6的Proxy实现的
```

*reactive注意点：*

- 值必须是一个对象（json/arr）

- 如果没有给reactive传递对象，默认情况下给对象修改值，是无法作出页面响应，如果想反映到页面上，则需要给对象重新复值。

- 使用 `reactive` 组合函数时必须始终保持对这个所返回对象的引用以保持响应性。**这个对象不能被解构或展开，一旦被解构或者展开，返回的值将失去响应式**。

  `toRefs` API 用来提供解决此约束的办法——它将响应式对象的每个 property 都转成了相应的 `ref`。

#### 什么是ref？

- ref和reactive一样， 也是用来显示响应式数据的方法

- 犹豫reactive必须传递一个对象，所以导致在企业开发中如果我们只想让某一个变量实现响应式的时候就会非常麻烦，所以vue3就给我们提供了ref方法，实现对简单值的监听

- ref本质：

  底层的本质其实还是reactive，系统会自动根据我们传入的ref值将他转换成ref(10) ===> reactive({value:10})

  *ref注意点：*

  - 在vue中的template中使用ref的值不用通过value获取
  - 在js中使用ref的值必须通过value获取
  
  ref:num=100  num=200 无需.value

#### get

- 收集依赖（track）；
- 如果当前 key 的值是对象，则为当前 key 的对
- 象创建拦截器 handler, 设置 get/set/deleteProperty；

如果当前的 key 的值不是对象，则返回当前 key 的值。

#### set

设置的新值和老值不相等时，更新为新值，并触发更新（trigger）。

deleteProperty 当前对象有这个 key 的时候，删除这个 key 并触发更新（trigger）。

### 2. effect

接收一个函数作为参数。作用是：访问响应式对象属性时去收集依赖

### 3. track

#### 接收两个参数：target 和 key

－如果没有 activeEffect，则说明没有创建 effect 依赖

－如果有 activeEffect，则去判断 WeakMap 集合中是否有 target 属性

－WeakMap 集合中没有 target 属性，则 set(target, (depsMap = new Map()))

－WeakMap 集合中有 target 属性，则判断 target 属性的 map 值的 depsMap 中是否有 key 属性

－depsMap 中没有 key 属性，则 set(key, (dep = new Set())) －depsMap 中有 key 属性，则添加这个 activeEffect

### 4.trigger

判断 WeakMap 中是否有 target 属性，WeakMap 中有 target 属性，则判断 target 属性的 map 值中是否有 key 属性，有的话循环触发收集的 effect()。

## 其他

### watch和watchEffect

watch 监听reactive 需要用箭头函数 ，可以监听多个,想非惰性，可以接收第三个参数

watch（[()=>(),()=>()],([new,Pre],[new,Pre])=>{ },{ immediate:true}）

watchEffect 里面依赖发生变化触发且非惰性

###  生命周期

onBeforeMount

 onRenderTracked(页面渲染收集完依赖)

 onMount

 onRenderTriggered(页面重新渲染触发) 

onBeforeUpdate 

onUpdate 

onBeforeUnmount 

onUnmount (页面隐藏) 

onUnmounted (页面卸载)

### 递归监听 与 非递归监听

递归监听

```javascript
import { ref, reactive, isRef, isReactive } from 'vue'
let obj1 = ref({
	a:"a",
	grandfather:{
		b:"b",
		father:{
			c:"c",
			son:{
				d:"d"
			}
		}
	}
})
obj1.value.a
obj1.value.randfather...

let obj2 = reactive({
	a:"a",
	grandfather:{
		b:"b",
		father:{
			c:"c",
			son:{
				d:"d"
			}
		}
	}
})
每一层都是proxy
```

上面情况默认使用的是递归监听，打印其本身，ref第一层是一个ref，**reactive的每一层都是一个Proxy对象，只要改变某一个值，都会更新到页面上**

非递归监听

非递归监听就是，视图只能监听到数据第一层变化，而监听不到更深层的变化。这两个函数创建出来变量是非递归监听的，只为某个对象的私有（第一层）属性创建浅层的响应式代理，不会对“属性的属性”做深层次、递归地响应式代理，而只是保留原样。

​	

```javascript
import { shallowRef, shallowReactive } from 'vue'
注意：
 如果通过shallowRef创建数据，那么vue监听的是.value的变化，并不是第一层的变化
 
import { triggerRef } from 'vue'
注意：
vue3只提供了triggerRef的方法，没有提供triggerReactive方法
所以如果是reactive类型的数据，那么是无法主动触发界面更新

setup() {
        let person = shallowRef({
            a: 'a',
            gf: {
                b: 'b',
                f: {
                    c: 'c',
                    s: {
                        d: 'd'
                    }
                }
            }
        })
        function change() {
            person.value.a = 1  		//因为是shallowRef所以需要修改.vlaue属性
            person.value.gf.b = 2
            person.value.gf.f.c = 3
            person.value.gf.f.s.d = 4
            
            triggerRef(person)		// 使用该方法，页面才可以监听变化
        }
        return { change,person }
    }
    //不调用可用直接赋值
		person.value = {
			a: '1',
    		gf: {
        		b: '2',
        		f: {
            		c: '3',
            		s: {
                		d: '4'
            		}
        		}
   		 }
	}

	let obj = shallowReactive({
            a: 'a',
            gf: {
                b: 'b',
                f: {
                    c: 'c',
                    s: {
                        d: 'd'
                    }
                }
            }
        })
        function change() {
            console.log(obj)
            obj.a = 1	 //此时如果注释掉第一层变量修改值，则页面无法监听数据变化，若一层数据也会修改，则整体数据就会重新渲染
            // obj.gf.b = 2
            // obj.gf.f.c = 3
            obj.gf.f.s.d = 4

        }
        return { obj, change}

```

应用场景

一般情况下我们使用 ref 和 reactive即可，只有我们在需要监听的数据量比较大的时候，我们才使用shallowRef/shallowReactive提供监听

shallowRef底层原理

```javascript
ref -> reactive
ref(10) -> reactive({value:10})
shallowRef -> shallowReactive
shallowRef(10) -> shallowReactive({value:10})
```

所以如果是通过shallowRef创建的数据，他监听的是.value的变化
因为底层本质上vlaue才是第一层

### toRaw()

ref/reactive类型数据的特点：
每次修改都会被追踪，都会更新UI界面，但是这样其实是非常消耗性能的，**所以如果我们有一些操作不需要追踪，不需要更新UI界面**，那么这时候，就可以通过toRaw方法拿到他的原始数据，对原始数据进行修改，这样就不会被追踪，也不会更新UI界面，从而来优化性能



```javascript
import { reactive } from 'vue'
setup() {
	let obj = {
		name:'张三',
		age: 18
	}
	let state = reactive(obj)
	console.log(obj === state) //false 
	/*
		state 和 obj 的关系
		引用关系，state的本质是一个Proxy对象，在这个Proxy对象中引					用了obj
	*/
let changeData = function() {
	obj.name = 'lisi'
	// 修改了obj对象中一个属性，obj和state引用中都已修改，但是视图数据不变
	// 若想视图变化，需要修改包装后的对象。才能触发视图的更新
	state.obj.name = 'wangwu'
}
	
	let obj2 = toRaw(state)
	console.log(obj === obj2) //true
	let changeData = function() {
		// 此时调用修改数据方法，则页面ui不会更新，但数据已经修改 
		obj2.name = 'lisi'
	}
    
    let state = ref(obj)
	let obj2 = toRaw(state)
	console.log(obj)
	console.log(state)
	console.log(obj2) //此时obj2 并不是{name:'张三',age:18}
	let obj3 = toRaw(state.value)
	console.log(obj3) // {name:'张三',age:18}

	
}
```

### markRaw()

对数据永远不追踪，数据如何修改都不会响应到UI上

### 手写shallowReactive, shallowRef

```javascript
//shallowReactive
function shllowReactive(obj) {
    return new Proxy(obj, {
        get(obj, key) {
            return obj[key]
        },
        set(obj, key, value) {
            obj[key] = value
            console.log('更新UI界面')
            return true
        }
    })
}
let obj = {
    a: "a",
    gf: {
        b: "b",
        f: {
            c: "c",
            s: {
                d: "d"
            }
        }
    }
}

let test = shllowReactive(obj)
test.a = "1" // 会更新ui界面，若不修改第一层，则不会输出“更新UI界面”

//shallowRef

function shallowRef(val) {
    shllowReactive({ value: val })
}
function shllowReactive(obj) {
    return new Proxy(obj, {
        get(obj, key) {
            return obj[key]
        },
        set(obj, key, value) {
            obj[key] = value
            console.log('更新UI界面')
            return true
        }
    })
}
let obj = {
    a: "a",
    gf: {
        b: "b",
        f: {
            c: "c",
            s: {
                d: "d"
            }
        }
    }
}

let test = shallowRef(obj)
test.value = {
	a: "1",
    gf: {
        b: "2",
        f: {
            c: "3",
            s: {
                d: "4"
            }
        }
    }
}
```

### 手写reactive, ref

```javascript
function ref(val) {
    reactive({ value: val })
}
function reactive(obj) {
	// 递归判断属性是否为对象，如果为对象则将对象继续创建为Proxy
    if (typeof obj === 'object') {
        if (obj instanceof Array) {
            obj.forEach((item, index) => {
                if (typeof item === 'object')
                    obj[index] = reactive(obj[index])

            })
        } else {
            for (let key in obj) {
                if (typeof obj[key] === 'object')
                    obj[key] = reactive(obj[key])
            }
        }
    }
    return new Proxy(obj, {
        get(obj, key) {
            return obj[key]
        },
        set(obj, key, value) {
            obj[key] = value
            console.log('更新UI界面')
            return true
        }
    })
}

let obj = {
    a: "a",
    gf: {
        b: "b",
        f: {
            c: "c",
            s: {
                d: "d"
            }
        }
    }
}

let test = reactive(obj)
function btnFn() {
    test.a = "1"
    test.gf.b = "2"
    test.gf.f.c = "3"
    test.gf.f.s.d = "4"
    console.log('change test', test)
}

let arr = [{ name: 'bob', attr: { sex: 'boy', age: 12 } }, { name: 'alice', attr: { age: 16 } }]

let arrReactive = reactive(arr)

function changeArr() {
    arr[0].name = "特朗普"
    arr[0].sex = "1"
    arr[1].age = "89"
}

let state = ref(123)
function changeRef() {
    console.log(state)
    state.value = 345
}

```





# React

## React的优点

1.声明式编程

2.组件化编程

3.支持客户端和服务端渲染

4.高效：原因在于虚拟DOM和Diff算法

## 虚拟DOM与Diff算法

虚拟DOM本质上是一个轻量的JavaScript对象，它是真实DOM的一个副本

在setState之后创建新的虚拟DOM树，与旧的虚拟DOM树通过Diff算法进行比较

多次setState合并操作，记录比较差异，最终将差异绘制到真实DOM，进行局部更新

## 你的技术栈主要是react，那你说说你⽤react有什么坑点？

1、JSX做表达式判断时候，需要强转为boolean类型，如：

```javascript
render() {
  const b = 0;
  return <div>
    {
      !!b && <div>这是⼀段⽂本</div>
    } 
  </div>
}
```

如果不使⽤ !!b 进⾏强转数据类型，会在⻚⾯⾥⾯输出 0。
2、尽量不要在 componentWillReviceProps ⾥使⽤ setState，如果⼀定要使⽤，那么需要判断结束条 件，不然会出现⽆限重渲染，导致⻚⾯崩溃。(实际不是componentWillReviceProps会⽆限重渲染，⽽ 是componentDidUpdate)
3、给组件添加ref时候，尽量不要使⽤匿名函数，因为当组件更新的时候，匿名函数会被当做新的prop 处理，让ref属性接受到新函数的时候，react内部会先清空ref，也就是会以null为回调参数先执⾏⼀次 ref这个props，然后在以该组件的实例执⾏⼀次ref，所以⽤匿名函数做ref的时候，有的时候去ref赋值 后的属性会取到null
4、遍历⼦节点的时候，不要⽤ index 作为组件的 key 进⾏传⼊。



## 怎么去设计⼀个组件封装
- 组件封装的⽬的是为了重⽤，提⾼开发效率和代码质量 
- 低耦合，单⼀职责，可复⽤性，可维护性

## react 的虚拟dom是怎么实现的

⾸先说说为什么要使⽤Virturl DOM，因为操作真实DOM的耗费的性能代价太⾼，所以react内部使⽤js 实现了⼀套dom结构，在每次操作在和真实dom之前，使⽤实现好的diﬀ算法，对虚拟dom进⾏⽐较， 递归找出有变化的dom节点，然后对其进⾏更新操作。为了实现虚拟DOM，我们需要把每⼀种节点类 型抽象成对象，每⼀种节点类型有⾃⼰的属性，也就是prop，每次进⾏diﬀ的时候，react会先⽐较该节
点类型，假如节点类型不⼀样，那么react会直接删除该节点，然后直接创建新的节点插⼊到其中，假如 节点类型⼀样，那么会⽐较prop是否有更新，假如有prop不⼀样，那么react会判定该节点有更新，那 么重渲染该节点，然后在对其⼦节点进⾏⽐较，⼀层⼀层往下，直到没有⼦节点。

## react hooks 原理是什么？

hooks 是⽤闭包实现的，因为纯函数不能记住状态，只能通过闭包来实现。

## useState 中的状态是怎么存储的？

通过单向链表，ﬁber tree 就是⼀个单向链表的树形结构。

## 如何遍历⼀个dom树

```javascript
function traversal(node) { 
    //对node的处理
    if (node && node.nodeType === 1) { 
        console.log(node.tagName);
    }
    var i = 0,
        childNodes = node.childNodes, 
        item;
    for (; i < childNodes.length; i++) { 
        item = childNodes[i];
        if (item.nodeType === 1) { 
            //递归先序遍历⼦节点
            traversal(item); 
        }
    }
}
```

## 数据双向绑定单向绑定优缺点

双向绑定是⾃动管理状态的，对处理有⽤户交互的场景⾮常合适，代码量少，当项⽬越来越⼤的时 候，调试也变得越来越复杂，难以跟踪问题
单向绑定是⽆状态的, 程序调试相对容易, 可以避免程序复杂度上升时产⽣的各种问题, 当然写代码 时就没有双向绑定那么爽了

## React ﬁber 的理解和原理
理解 
React16 以前
React16 以前，对virtural dom的更新和渲染是同步的。就是当⼀次更新或者⼀次加载开始以后，diﬀ virtual dom并且渲染的过程是⼀⼝⽓完成的。如果组件层级⽐较深，相应的堆栈也会很深，⻓时间占⽤ 浏览器主线程，⼀些类似⽤户输⼊、⿏标滚动等操作得不到响应。借Lin的两张图，视频 A Cartoon Intro to Fiber - React Conf 2017。

React16 Fiber Reconciler
React16 ⽤了分⽚的⽅式解决上⾯的问题。 
就是把⼀个任务分成很多⼩⽚，当分配给这个⼩⽚的时间⽤尽的时候，就检查任务列表中有没有新的、 优先级更⾼的任务，有就做这个新任务，没有就继续做原来的任务。这种⽅式被叫做异步渲染(Async Rendering)。

⼀些原理 
Fiber就是通过对象记录组件上需要做或者已经完成的更新，⼀个组件可以对应多个Fiber。
在render函数中创建的React Element树在第⼀次渲染的时候会创建⼀颗结构⼀模⼀样的Fiber节点树。 不同的React Element类型对应不同的Fiber节点类型。⼀个React Element的⼯作就由它对应的Fiber节 点来负责。
⼀个React Element可以对应不⽌⼀个Fiber，因为Fiber在update的时候，会从原来的Fiber（我们称为 current）clone出⼀个新的Fiber（我们称为alternate）。两个Fiber diﬀ出的变化（side eﬀect）记录 在alternate上。所以⼀个组件在更新时最多会有两个Fiber与其对应，在更新结束后alternate会取代之 前的current的成为新的current节点。
其次，Fiber的基本规则：
更新任务分成两个阶段，Reconciliation Phase和Commit Phase。Reconciliation Phase的任务⼲的事 情是，找出要做的更新⼯作（Diﬀ Fiber Tree），就是⼀个计算阶段，计算结果可以被缓存，也就可以 被打断；Commmit Phase 需要提交所有更新并渲染，为了防⽌⻚⾯抖动，被设置为不能被打断。
PS: componentWillMount componentWillReceiveProps componentWillUpdate ⼏个⽣命周期⽅法， 在Reconciliation Phase被调⽤，有被打断的可能（时间⽤尽等情况），所以可能被多次调⽤。其实 shouldComponentUpdate 也可能被多次调⽤，只是它只返回true或者false，没有副作⽤，可以暂时 忽略。

## 解释 React 中 render() 的⽬的

每个React组件强制要求必须有⼀个 render()。它返回⼀个 React 元素，是原⽣ DOM 组件的表示。如 果需要渲染多个 HTML 元素，则必须将它们组合在⼀个封闭标记内，例如 、 、`` 等。此函数必须保持 纯净，即必须每次调⽤时都返回相同的结果。

## 调⽤ setState 之后发⽣了什么？

- 在代码中调⽤ setState 函数之后，React 会将传⼊的参数对象与组件当前的状态合并，然后触发 所谓的调和过程。 
- 经过调和过程，React会以相对⾼效的⽅式根据新的状态构建React元素树并且着⼿重新渲染整个 UI 界⾯。
- 在 React 得到元素树之后，React 会⾃动计算出新的树与⽼树的节点差异，然后根据差异对界⾯进 ⾏最⼩化重渲染。
- 在差异计算算法中，React 能够相对精确地知道哪些位置发⽣了改变以及应该如何改变，这就保证 了按需更新，⽽不是全部重新渲染。

## 触发多次setstate，那么render会执⾏⼏次？
- 多次setState会合并为⼀次render，因为setState并不会⽴即改变state的值，⽽是将其放到⼀个任 务队列⾥，最终将多个setState合并，⼀次性更新⻚⾯。 
- 所以我们可以在代码⾥多次调⽤setState，每次只需要关注当前修改的字段即可

## react中如何对state中的数据进⾏修改？setState为什么是⼀个异步的？
- 修改数据通过this.setState(参数1,参数2) 
- this.setState是⼀个异步函数
  - 参数1 : 是需要修改的数据是⼀个对象
  - 参数2 : 是⼀个回调函数，可以⽤来验证数据是否修改成功，同时可以获取到数据更新后的 DOM结构等同于componentDidMount

- this.setState中的第⼀个参数除了可以写成⼀个对象以外，还可以写成⼀个函数  ！，函数中第⼀ 个值为prevState 第⼆个值为prePprops     this.setState((prevState,prop)=>({}))

## 为什么建议传递给 setState的参数是⼀个callback⽽不是⼀个对象？
因为this.props 和this.state的更新可能是异步的，不能依赖它们的值去计算下⼀个state
## 为什么setState是⼀个异步的？ 
当批量执⾏state的时候可以让DOM渲染的更快,也就是说多个setstate在执⾏的过程中还需要被合
并

## 原⽣事件和React事件的区别？

- React  事件使⽤驼峰命名，⽽不是全部⼩写。
- 通过 JSX  , 你传递⼀个函数作为事件处理程序，⽽不是⼀个字符串。
- 在 React  中你不能通过返回 false  来阻⽌默认⾏为。必须明确调⽤ preventDefault 。

## React的合成事件是什么？

React  根据 W3C  规范定义了每个事件处理函数的参数，即合成事件。

事件处理程序将传递 SyntheticEvent  的实例，这是⼀个跨浏览器原⽣事件包装器。它具有与浏览器 原⽣事件相同的接⼝，包括 stopPropagation()  和 preventDefault() ，在所有浏览器中他们⼯作 ⽅式都相同。

React 合成的 SyntheticEvent 采⽤了事件池，这样做可以⼤⼤节省内存，⽽不会频繁的创建和销毁 事件对象。


另外，不管在什么浏览器环境下，浏览器会将该事件类型统⼀创建为合成事件，从⽽达到了浏览器兼容 的⽬的。
## 什么是⾼阶组件（HOC）
⾼阶组件是重⽤组件逻辑的⾼级⽅法，是⼀种源于 React 的组件模式。 HOC 是⾃定义组件，在它之内 包含另⼀个组件。它们可以接受⼦组件提供的任何动态，但不会修改或复制其输⼊组件中的任何⾏为。 你可以认为 HOC 是“纯（Pure）”组件。

## 你能⽤HOC做什么？

HOC可⽤于许多任务，例如：

- 代码重⽤，逻辑和引导抽象 
- 渲染劫持
- 状态抽象和控制
- Props 控制

## 你知道的react的生命周期

初始化

- componentWillMount 初始化调用，只调用一次
  render 渲染页面
- componentDidMount 第一次渲染后调用

更新

- componentWillReceireProps 接收新的props时调用
- shouldComponentUpdate state或props改变时调用
- componentWillUpdate 将要更新时调用，可改变state的值
  render
- componentDidUpdate 更新后调用

卸载

- componentWillUnMount

## react声明默认props

设置默认props有两种方式

- 指定 props 的默认值， 这个方法只有浏览器编译以后 才会生效

```
static defaultProps = { 
  age: 18
}
复制代码
```

- 指定 props 的默认值，这个方法会一直生效

```
  Greeting.defaultProps = {    
    name: '我是props的默认值！'
  }
```

## react创建组件的三种方式

1.函数式定义的无状态组件，适用于纯展示组件，只负责根据传入的props展示，不涉及state状态的操作，特点如下

- 组件不会被实例化，整体渲染性能得到提升。
- 组件不能访问this对象
- 组件无法访问生命周期的方法
- 无状态组件只能访问输入的props，同样的props会得到同样的渲染结果，不会有副作用

```
function  MyTest1() {
  return <h1>工厂(无状态)函数(最简洁, 推荐使用)</h1>
}
```

2.es5原生方式React.createClass定义有状态的组件

- 组件会被实例化
- 可以访问生命周期
- 会自绑定函数方法

```
var MyTest2=React.createClass({
  render(){
   return <h1>ES5老语法(不推荐使用了)</h1>
  }
})
复制代码
```

3.es6形式的extends React.Component定义的组件，是以ES6的形式来创建react的组件的，是React目前极为推荐的创建有状态组件的方式

```
class  MyTest3 extends React.Component{
  render(){
     return <h1>ES6类语法(复杂组件, 推荐使用)</h1>
  }
}
```

React.createClass与React.Component区别

- this绑定不同
  React.createClass创建的组件，其每一个成员函数的this都有React自动绑定，任何时候使用，直接使用this.method即可，函数中的this会被正确设置。
  React.Component创建的组件，其成员函数不会自动绑定this，需要开发者手动绑定，否则this不能获取当前组件实例对象。
- 组件属性类型propTypes及其默认props属性defaultProps配置不同
- 组件初始状态state的配置不同
- Mixins的支持不同



#  Webpack

## Webpack中的module是指什么?

其实webpack中提到的module概念, 和咱们平时前端开发的module概念是⼀样的.

webpack ⽀持ESModule, CommonJS, AMD, Assets等.

## 简单说⼏个模块的引⼊和导出⽅式: 

1. ESM

关键字 export 允许将 ESM 中的内容暴露给其他模块,

关键字 import 允许从其他模块获取引⽤到 ESM 中.

```
import { aa } from './a.js';

export { bb };
```

可以设置 package.json 中的属性来显式设置⽂件模块类型。

在 package.json 中

设置 “type”: “module” 会强制 package.json 下的所有⽂件使⽤ ECMAScript 模块。

设置 “type”: “commonjs” 将会强制使⽤ CommonJS 模块。

2. CommonJS

module.exports 允许将 CommonJS 中的内容暴露给其他模块,

require 允许从其他模块获取引⽤到 CommonJS 中.

```
const path = require('path');
module.exports = {
 aa,
 bb
}
```

## 我们常说的 chunk 和 bundle 的区别是什么? 

1. Chunk

   Chunk是Webpack打包过程中Modules的集合，是打包过程中的概念。

   Webpack的打包是从⼀个⼊⼝模块开始，⼊⼝模块引⽤其他模块，模块再引⽤模块。

   Webpack通过引⽤关系逐个打包模块，这些module就形成了⼀个Chunk。

   当然如果有多个⼊⼝模块，可能会产出多条打包路径，每条路径都会形成⼀个Chunk。 

2. Bundle

   Bundle是我们最终输出的⼀个或多个打包好的⽂件.

3. Chunk 和 Bundle 的关系?

   ⼤多数情况下，⼀个Chunk会⽣产⼀个Bundle。⽐如咱们来简单写个项⽬试⼀下

```javascript
module.exports = {
 mode: "production",
 entry: {
 index: "./src/index.js"
 },
 output: {
 filename: "[name].js"
 }
};
```

但是当我们开启source-map后, chunk和bundle就不是⼀对⼀的关系了.

```
module.exports = {
 mode: "production",
 entry: {
 index: "./src/index.js"
 },
 output: {
 filename: "[name].js"
 },
 devtool: "source-map"
}
```

可以看⼀下webpack的输出, ChunkNames只有⼀个Index, ⽽输出了两个bundle. index.js, 

index.js.map.

所以可以有这样的⼀个总结：

Chunk是过程中的代码块，Bundle是打包结果输出的代码块, Chunk在构建完成就呈现为

Bundle。 

4. ⽣成Chunk的⼏种⽅式

   **entry配置⼀个key, value为数组**

   ```
   module.exports = {
    mode: "production",
    entry: {
    index: ["./src/index.js", "./src/add.js"]
    },
    output: {
    filename: "[name].js"
    }
   };
   ```

   可以看到这种情况, 也只会产⽣⼀个chunk.

   **entry配置多个key**

   ```
   module.exports = {
    mode: "production",
    entry: {
    index: "./src/index.js",
    common: "./src/common.js"
    },
    output: {
    filename: "[name].js"
    },
   };
   ```

   可以看到这种情况, 产⽣了common和index两个chunk, 配置的key也就会被⽤为chunkName.

   ⽽output中filename字段, 将被⽤为bundle的名称。

   

   Compiler 对象包含了 Webpack 环境所有的的配置信息，包含 options，loaders，plugins 这

   些信息，这个对象在 Webpack 启动时候被实例化，它是全局唯⼀的，可以简单地把它理解为

   Webpack 实例；

   Compilation 对象包含了当前的模块资源、编译⽣成资源、变化的⽂件等。当 Webpack 以开

   发模式运⾏时，每当检测到⼀个⽂件变化，⼀次新的 Compilation 将被创建。Compilation 对

   象也提供了很多事件回调供插件做扩展。通过 Compilation 也能读取到 Compiler 对象。

## webpack modules 如何表达⾃⼰的各种依赖关系?

ESM import 语句

CommonJS require() 语句

AMD define 和 require 语句

css/sass/less ⽂件中的 @import 语句。

stylesheet url(…) 或者 HTML ⽂件中的图⽚链接。

## 有哪些常见的loader和plugin，你用过哪些

```
loader
⼀句话描述：模块转换器，将⾮js模块转化为webpack能识别的js模块

loader 让 webpack 能够去处理那些⾮ JavaScript ⽂件.
Loader 可以将所有类型的⽂件转换为 webpack 能够处理的有效模块,然后你就可以利⽤
webpack 的打包能⼒,对它们进⾏处理。
本质上,webpack loader 将所有类型的⽂件,转换为应⽤程序的依赖图（和最终的 bundle）可
以直接引⽤的模块。

file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
source-map-loader：加载额外的 Source Map 文件，以方便断点调试
image-loader：加载并且压缩图片文件
babel-loader：把 ES6 转换成 ES5
css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
eslint-loader：通过 ESLint 检查 JavaScript 代码

plugin
⼀句话描述：扩展插件，在webpack运⾏的各个阶段，都会⼴播出去相对应的事件，插件可以
监听到这些事件的发⽣，在特定的时机做相对应的事情

Loader 被⽤于转换某些类型的模块,⽽插件则可以⽤于执⾏范围更⼴的任务。
插件的范围包括,从打包优化和压缩,⼀直到重新定义环境中的变量。插件接⼝功能极其强⼤,可
以⽤来处理各种各样的任务。
在 webpack 运⾏的⽣命周期中会⼴播出各种事件，Plugin 就可以监听这些事件，在触发时通
过 webpack 提供的 API 改变输出结果。
在插件中，可以拿到 Compile 和 Compilation 的引⽤对象，使⽤它们⼴播事件，这些事件可
以被其他插件监听到，或者对他们做出⼀定修改，其他插件拿到的也是变化的对象。

define-plugin：定义环境变量
commons-chunk-plugin：提取公共代码
uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码

复制代码
```

## loader 和 plugin 的区别是什么？

```
不同的作用：
Loader直译为"加载器"。
    Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力。
Plugin直译为"插件"。
    Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

不同的用法：
Loader在module.rules中配置，也就是说他作为模块的解析规则而存在。类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）。
Plugin在plugins中单独配置，类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。
复制代码
```

##  如何按需加载代码？

```
Vue UI组件库的按需加载 为了快速开发前端项目，经常会引入现成的UI组件库如ElementUI、iView等，但是他们的体积和他们所提供的功能一样，是很庞大的。 
而通常情况下，我们仅仅需要少量的几个组件就足够了，但是我们却将庞大的组件库打包到我们的源码中，造成了不必要的开销。

不过很多组件库已经提供了现成的解决方案，如Element出品的babel-plugin-component和AntDesign出品的babel-plugin-import 安装以上插件后，
在.babelrc配置中或babel-loader的参数中进行设置，即可实现组件按需加载了。

{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
单页应用的按需加载 现在很多前端项目都是通过单页应用的方式开发的，但是随着业务的不断扩展，会面临一个严峻的问题——首次加载的代码量会越来越多，影响用户的体验。

通过import(*)语句来控制加载时机，webpack内置了对于import(*)的解析，会将import(*)中引入的模块作为一个新的入口在生成一个chunk。 
当代码执行到import(*)语句时，会去加载Chunk对应生成的文件。import()会返回一个Promise对象，所以为了让浏览器支持，需要事先注入Promise polyfill
复制代码
```

##  如何提高webpack的构建速度

```
1.多入口情况下，使用CommonsChunkPlugin来提取公共代码
2.通过externals配置来提取常用库
3.利用DllPlugin和DllReferencePlugin预编译资源模块 4.通过DllPlugin来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过DllReferencePlugin将预编译的模块加载进来。
5.使用Happypack 实现多线程加速编译
6.使用webpack-uglify-parallel来提升uglifyPlugin的压缩速度。 7.原理上webpack-uglify-parallel采用了多核并行压缩来提升压缩速度
8.使用Tree-shaking和Scope Hoisting来剔除多余代码
复制代码
```

##  怎么配置单页应用？怎么配置多页应用？

```
单页应用可以理解为webpack的标准模式，直接在entry中指定单页应用的入口即可，这里不再赘述

多页应用的话，可以使用webpack的 AutoWebPlugin来完成简单自动化的构建，但是前提是项目的目录结构必须遵守他预设的规范。 

多页应用中要注意的是：
每个页面都有公共的代码，可以将这些代码抽离出来，避免重复的加载。比如，每个页面都引用了同一套css样式表
随着业务的不断扩展，页面可能会不断的追加，所以一定要让入口的配置足够灵活，避免每次添加新页面还需要修改构建配置
复制代码
```

##  webpack的proxy是如何解决跨域的？

```
webpack的dev-serve模块会启动一个服务器，这个服务器不止帮我们做了自动更新，同时也可以做到反向代理。

就是我们把请求发送给webpack-dev-serve，然后webpack-dev-serve再去请求后端服务器，服务器之间的请求是没有跨域问题的。

只要后端返回了，webpack-dev-serve就能拿到，然后再返回给前端。
复制代码
```

## 能简单描述⼀下webpack的打包流程吗？

1. 初始化参数：从配置⽂件和 Shell 语句中读取与合并参数,得出最终的参数。

2. 开始编译：⽤上⼀步得到的参数初始化 Compiler 对象,加载所有配置的插件,执⾏对象的 run ⽅法开始执⾏编译。

3. 确定⼊⼝：根据配置中的 entry 找出所有的⼊⼝⽂件。

4. 编译模块：从⼊⼝⽂件出发,调⽤所有配置的 Loader 对模块进⾏翻译,再找出该模块依赖的模

   块,再递归本步骤直到所有⼊⼝依赖的⽂件都经过了本步骤的处理。

5. 完成模块编译：在经过第 4 步使⽤ Loader 翻译完所有模块后,得到了每个模块被翻译后的最终

   内容以及它们之间的依赖关系。

6. 输出资源：根据⼊⼝和模块之间的依赖关系,组装成⼀个个包含多个模块的 Chunk,再把每个

   Chunk 转换成⼀个单独的⽂件加⼊到输出列表,这步是可以修改输出内容的最后机会。

7. 输出完成：在确定好输出内容后,根据配置确定输出的路径和⽂件名,把⽂件内容写⼊到⽂件系

   统

## 性能优化

###  前端性能优化

```
三个方面来说明前端性能优化

一： webapck优化与开启gzip压缩
    1.babel-loader用 include 或 exclude 来帮我们避免不必要的转译，不转译node_moudules中的js文件
    其次在缓存当前转译的js文件，设置loader: 'babel-loader?cacheDirectory=true'
    2.文件采用按需加载等等
    3.具体的做法非常简单，只需要你在你的 request headers 中加上这么一句：accept-encoding:gzip
    4.图片优化，采用svg图片或者字体图标
    5.浏览器缓存机制，它又分为强缓存和协商缓存
二：本地存储——从 Cookie 到 Web Storage、IndexedDB
    说明一下SessionStorage和localStorage还有cookie的区别和优缺点
三：代码优化
    1.事件代理
    2.事件的节流和防抖
    3.页面的回流和重绘
    4.EventLoop事件循环机制
    5.代码优化等等
复制代码
```

##  工具类

#### 1.npm与yarn的区别

```
Yarn文档中有说，Yarn 是为了弥补 npm 的一些缺陷而出现的

yarn 的速度更快；安装版本会更统一；更简洁的输出（配合emoji表情）；更好的语义化，比如yarn add/remove 替代 npm install/uninstall
复制代码
```

## 主观题

#### 1.说说你经常遇到的问题

```
经常遇到的问题就是Cannot read property ‘prototype’ of undefined
解决办法通过浏览器报错提示代码定位问题，解决问题

Vue项目中遇到视图不更新，方法不执行，埋点不触发等问题
一般解决方案查看浏览器报错，查看代码运行到那个阶段未之行结束，阅读源码以及相关文档等
然后举出来最近开发的项目中遇到的算是两个比较大的问题。
复制代码
```

#### 2.说说你对前端工程化的理解

```
我个人理解主要是从模块化、组件化、规范化、自动化这四个方面来实现前端工程化的：
1.模块化：
    简单来说，模块化就是将一个大文件拆分成相互依赖的小文件，再进行统一的拼装和加载。
2.组件化：
    组件化有点类似模块化，但模块化主要是在文件层面上的，而组件化则是在平时写代码的时候就有封装组件的意识，让组件能够多次复用
3.规范化：
    比如目录结构的制定、代码的规范、接口的规范、命名的规范等，遵循一套规范可以让代码更容易维护，出问题的时候也方便快速定位
4.自动化：
    任何简单机械的重复劳动都应该让机器去完成，比如自动化测试、自动化部署等
```



