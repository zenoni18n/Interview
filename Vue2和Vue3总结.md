

# Vue3.0 最常出现的面试题

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

