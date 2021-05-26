抓紧面试前的宝贵时间，快速搞定前端JS面试

## 基础

1. 前端两个标准 W3C标准 html css dom bom 等应用标准和ECMA262标准 es6 原型闭包等语法标准

2. JavaScript一共有8种数据类型，其中有7种基本数据类型：Undefined、Null、Boolean、Number、String、Symbol（es6新增，表示独一无二的值）和BigInt（es10新增）Object；

3. Object 中包含了哪几种类型？
   其中包含了Data、function、Array等。这三种是常规用的。
   注意：null高程中明确说了是基本数据类型，虽然用 typeof返回的是Object ，但instanceof Object 结果是false
   typeof 能返回的去掉null加上function 也是7种

   **值类型和引用类型的区别**

   原始数据类型：直接存储在**栈**（stack）中，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储。

   引用数据类型：同时存储在**栈**（stack）和**堆**（heap）中，占据空间大、大小不固定。引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

   https://blog.csdn.net/linxiaoduo/article/details/80117628

4. 3.使用const定义常量，而且定义一次以后不能再进行更改，否者会报错。但是要注意， 如果给常量定义的是对象，只要该对象指向在内存中的地址不发生改变， 数据可以随便改的（这涉及到计算机的传值和传址）；

5. 列举三种强制类型转换和两种隐式类型转换？
   parseInt(),parseFloat(),Number()
   ==,!!
   !!唯一的作用就是把值转化为布尔值，其实对代码逻辑来说没什用，即使不用不会有任何问题，只是看起来统一了数据类型为布尔值，建议不用。实际上Boolean(a)和!!a效果一样

6. 什么时候用===，什么时候用==？

   ```
   // 什么情况下用 == 或者 ===
   // 除了 == null 之外，其他的一律用 ===
   const obj = {x: 20};
   if (obj.x == null) {}
   // 相当于：
   // if(obj.x === null || obj.x === undefined) {}
   ```

   现在来探讨 [] == ! [] 的结果为什么会是true
   ①、根据运算符优先级 ，！ 的优先级是大于 == 的，所以先会执行 ![]

   ！可将变量转换成boolean类型，null、undefined、NaN以及空字符串('')取反都为true，其余都为false。

   所以 ! [] 运算后的结果就是 false

   也就是 [] == ! [] 相当于 [] == false

   ②、根据上面提到的规则（如果有一个操作数是布尔值，则在比较相等性之前先将其转换为数值——false转换为0，而true转换为1），则需要把 false 转成 0

   也就是 [] == ! [] 相当于 [] == false 相当于 [] == 0

   ③、根据上面提到的规则（如果一个操作数是对象，另一个操作数不是，则调用对象的valueOf()方法，用得到的基本类型值按照前面的规则进行比较，如果对象没有valueOf()方法，则调用 toString()）

   而对于空数组，[].toString() ->  '' (返回的是空字符串)

   也就是  [] == 0 相当于 '' == 0

   ④、根据上面提到的规则（如果一个操作数是字符串，另一个操作数是数值，在比较相等性之前先将字符串转换为数值）

   Number('') -> 返回的是 0

   相当于 0 == 0 自然就返回 true了
   

7. && 、 ||和!! 运算符分别能做什么

   - `&&` 叫逻辑与，在其操作数中找到第一个虚值表达式并返回它，如果没有找到任何虚值表达式，则返回最后一个真值表达式。它采用短路来防止不必要的工作。
   - `||` 叫逻辑或，在其操作数中找到第一个真值表达式并返回它。这也使用了短路来防止不必要的工作。在支持 ES6 默认函数参数之前，它用于初始化函数中的默认参数值。
   - `!!` 运算符可以将右侧的值强制转换为布尔值，这也是将值转换为布尔值的一种简单方法




8. 引用类型两道题

   **typeof只能识别不了array，object，null ，都是object,obj1和obj2地址一样，所以改哪个都影响x1只是单纯的赋值，和地址没关系**

   typeof 对于对象来说，除了函数都会显示 object，所以说 typeof 并不能准确判断变量到底是什么类型,所以想判断一个对象的正确类型，这时候可以考虑使用 instanceof

   详情(https://www.cnblogs.com/leiting/p/8081413.html)

   ```
   let a ={ age:20 };
   let b = a;
   b.age=21;
   console.log(a.age) //21
   // 干扰
   const obj1 ={ x:100,y:200 };
   const obj2=obj1;
   let x1=obj1.x;
   obj2.x=101;
   x1=102;
   console.log(obj1.x) //101
   ```

   **instanceof 可以正确的判断对象的类型**，因为内部机制是通过判断对象的原型链中是不是能找到类型的 prototype。

   ```
   console.log(2 instanceof Number);                    // false
   console.log(true instanceof Boolean);                // false 
   console.log('str' instanceof String);                // false  
   console.log([] instanceof Array);                    // true
   console.log(function(){} instanceof Function);       // true
   console.log({} instanceof Object);                   // true  
   ```

   可以看出直接的字面量值判断数据类型，instanceof可以精准判断引用数据类型（Array，Function，Object），而基本数据类型不能被instanceof精准判断。

   **constructor**

   ```
   console.log((2).constructor === Number); // true
   console.log((true).constructor === Boolean); // true
   console.log(('str').constructor === String); // true
   console.log(([]).constructor === Array); // true
   console.log((function() {}).constructor === Function); // true
   console.log(({}).constructor === Object); // true
   ```

   

9. 手写深拷贝

   ```javascript
   /**
    * 深拷贝
    */
   
   const obj1 = {
       age: 20,
       name: 'xxx',
       address: {
           city: 'beijing'
       },
       arr: ['a', 'b', 'c']
   }
   
   const obj2 = deepClone(obj1)
   obj2.address.city = 'shanghai'
   obj2.arr[0] = 'a1'
   console.log(obj1.address.city)
   console.log(obj1.arr[0])
   
   /**
    * 深拷贝
    * @param {Object} obj 要拷贝的对象
    */
   function deepClone(obj = {}) {
       if (typeof obj !== 'object' || obj == null) {
           // obj 是 null ，或者不是对象和数组，直接返回
           // typeof 运算符返回一个用来表示数据类型的字符串。
           return obj
       }
   
       // 初始化返回结果
       let result
       if (obj instanceof Array) {
           result = []
       } else {
           result = {}
       }
   
       for (let key in obj) {
           // 保证 key 不是原型的属性 
           //for in循环对象会访问原型链上所有对象属性，因为在工作中我们很难保证其他人是否会修改原型链
           if (obj.hasOwnProperty(key)) {
               // 递归调用！！！
               result[key] = deepClone(obj[key])
           }
       }
   
       // 不要忘了返回结果
       return result
   }
   ```

   **递归是防止obj[key]中还有数组或者对象**
   最后运行结果obj1.address.city还是等于beijing，不会被改变

   ![image-20200516172301805](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200516172301805.png)

```
一.函数声明语法定义：function sum(num1,num2){return num1+num2} 


二.函数表达式定义函数：1.var sum = function(num1,num2){return num1+num2}; 


2.var sum = new
Function("num1","num2","return
num1+num2");Function构造函数可以接受任意数量的参数，但最后一个参数始终被看成函数体，注意函数表达式定义函数的方法与声明其他变量时一样需要加分号。
```



## 原型 原型链

**class只是语法糖，但本质上依旧是基于原型的继承**

```
// 父类
class People {
    constructor(name) {
        this.name = name
    }
    eat() {
        console.log(`${this.name} eat something`)
    }
}
// 子类
class Student extends People {
    constructor(name, number) {
        super(name)
        this.number = number
    }
    sayHi() {
        console.log(`姓名 ${this.name} 学号 ${this.number}`)
    }
}
// 子类
class Teacher extends People {
    constructor(name, major) {
        super(name)
        this.major = major
    }
    teach() {
        console.log(`${this.name} 教授 ${this.major}`)
    }
}
// 实例
const xialuo = new Student('夏洛', 100)
console.log(xialuo.name)
console.log(xialuo.number)
xialuo.sayHi()  // ===  xialuo.__proto__.eat.call(xialuo)
xialuo.eat() //拿得到值
xialuo.__proto__.eat() //值是undefined 因为this是__proto__

```

![image-20200515171131597](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200515171131597.png)

1. 每个class都有显式原型prototype
2. 每个实例都有隐式原型**proto**
3. 实例的**proto**指向对应class的prototype

- xialuo是new Student出来的，是一个Student对象，xialuo的**proto**等于Student的Prototype
  这里People.prototype和Student.prototype.**proto**是相等的，也可以认为Student.prototype是new出来People对象
- People.prototype.**proto**和Object.prototype是相等的，那么可以认为People.prototype是new出来的Object对象 

![image-20200515171545215](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200515171545215.png)

Student.prototype.**proto** === People.prototype             Student.prototype.prototype === People.prototype
//true																				        //false
**实例对象的隐式 原型指向其构造函数的显式原型，Function的隐式原型指向自身的显式原型-----北梦**

> **instanceof** 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。

Student.prototype instanceof People  //true
People.prototype instanceof Object    //true

![img](https://images0.cnblogs.com/blog/138012/201409/181512068463597.png)

![image-20200803151439527](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200803151439527.png)

4. **prototype**：
   (1)每个函数都有一个prototype属性，它默认指向一个Object空对象（即：原型对象）

   (2)原型对象中有一个属性constructor，它指向函数对象 

   (3).它主要用来共享属性和方法（常用于构造函数创建实例对象给实例对象共享属性）
   (4).主要用来被访问
   (5).判断**自有属性和原型属性** 一般用hasOwnProperty()
   **proto**：
   (1).基本所有的对象有该属性
   (2).用来构建原型链访问构造方法中的显示原型

5. **hasOwnProperty**表示是否有自己的属性。这个方法会查找一个对象实例是否有某个属性，**但是不会去查找它的原型链**。

xialuo.hasOwnProperty('eat')  //false    xialuo.hasOwnProperty('hasOwnProperty')  //false    在object上

```javascript
Object.prototype只是一个普通对象，它是js原型链的最顶端

  Object.prototype.__proto__=== null;//true

  Object.prototype.prototype === undefied;//true

Object.prototype只是一个普通对象(普通对象没有prototype属性，所以值是undefined)，Object.prototype是js原型链的最顶端，它的__proto__是null(有__proto__属性，但值是null，因为这是原型链的最顶端)。

Ø  在js中如果A对象是由B函数构造的，那么A.__proto__ === B.prototype。

javascript中对象是由Object创建的，函数是由Function创建的。

Ø  内置的Object是其实也是一个函数对象，它是由Function创建的。

Object.__proto__ === Function.prototype;

Ø  js中每一个对象或函数都有__proto__属性，但是只有函数对象才有prototype属性。

//函数对象
function Person()

{
       

}
 
// 普通对象

var obj = {};

obj.__proto__ === Object.prototype;//true

obj.prototype === undefined;//true

Person.__proto__ === Function.prototype;//true

Person.prototype !== undefined;//true

 

原型链是基于__proto__形成的，继承是通过prototype实现的。

Ø  Function.prototype是个特例，它是函数对象，但是没有prototype属性。其他所有函数都有prototype属性。

Function.prototype.prototype === undefined;//true

Ø  内置的Function也是一个函数对象，它是通过自己来创建自己的。

Function.__proto__=== Function.prototype;//true

Ø  函数也是对象，因为Function.prototype__proto__指向Object.prototype。

typeof Function.prototype.__proto__ === "object";//true

Function.prototype.__proto__=== Object.prototype;//true
```

###  JavaScript 继承的几种实现方式？

```
（1）第一种是以原型链的方式来实现继承，但是这种实现方式存在的缺点是，在包含有引用类型的数据时，会被所有的实例对象所共享，容易造成修改的混乱。还有就是在创建子类型的时候不能向超类型传递参数。

（2）第二种方式是使用借用构造函数的方式，这种方式是通过在子类型的函数中调用超类型的构造函数来实现的，这一种方法解决了不能向超类型传递参数的缺点，但是它存在的一个问题就是无法实现函数方法的复用，并且超类型原型定义的方法子类型也没有办法访问到。

（3）第三种方式是组合继承，组合继承是将原型链和借用构造函数组合起来使用的一种方式。通过借用构造函数的方式来实现类型的属性的继承，通过将子类型的原型设置为超类型的实例来实现方法的继承。这种方式解决了上面的两种模式单独使用时的问题，但是由于我们是以超类型的实例来作为子类型的原型，所以调用了两次超类的构造函数，造成了子类型的原型中多了很多不必要的属性。

（4）第四种方式是原型式继承，原型式继承的主要思路就是基于已有的对象来创建新的对象，实现的原理是，向函数中传入一个对象，然后返回一个以这个对象为原型的对象。这种继承的思路主要不是为了实现创造一种新的类型，只是对某个对象实现一种简单继承，ES5 中定义的 Object.create() 方法就是原型式继承的实现。缺点与原型链方式相同。

（5）第五种方式是寄生式继承，寄生式继承的思路是创建一个用于封装继承过程的函数，通过传入一个对象，然后复制一个对象的副本，然后对象进行扩展，最后返回这个对象。这个扩展的过程就可以理解是一种继承。这种继承的优点就是对一个简单对象实现继承，如果这个对象不是我们的自定义类型时。缺点是没有办法实现函数的复用。

（6）第六种方式是寄生式组合继承，组合继承的缺点就是使用超类型的实例做为子类型的原型，导致添加了不必要的原型属性。寄生式组合继承的方式是使用超类型的原型的副本来作为子类型的原型，这样就避免了创建不必要的属性。

```



## 作用域和闭包

### 作用域和自由变量

- 自由变量：在全局中定义了一个变量a，然后在函数中使用了这个a，这个a就可以称之为自由变量，可以这样理解，**凡是跨了自己的作用域的变量都叫自由变量**。一个自由变量在当前作用域没有定义，但被使用了，则向上级作用域一层一层依次寻找，直至找到为止，如果到全局作用域都没找到，则报错 xx is not defined。
  注意：所有的自由变量的查找，是在函数定义的地方function（）向上级作用域查找，不是在执行的地方fn（）！

  ```javascript
  // 函数作为返回值
  function create() {
      const a = 100
      return function () {
          console.log(a)
      }
  }
  
  const fn = create()
  const a = 200
  fn() // 100
  
  // 函数作为参数被传递
  function print(fn) {
      const a = 200
      fn()
  }
  const a = 100
  function fn() {
      console.log(a)
  }
  print(fn) // 100
  
  // 所有的自由变量的查找，是在函数定义的地方，向上级作用域查找
  // 不是在执行的地方！！！
  ```

  上图进一步说明自由变量是在函数定义的地方向上级作用域查找，找不到就直接报错！如果在找不到的情况接着在执行的地方查找就会打印200，但是这里不打印，说明不会在执行的地方查找。
  
  **作用域：** 作用域是定义变量的区域，它有一套访问变量的规则，这套规则来管理浏览器引擎如何在当前作用域以及嵌套的作用域中根据变量（标识符）进行变量查找。
  
  **作用域链：** 作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，我们可以访问到外层环境的变量和 函数。
  
  
  

### 闭包

*闭包*就是能够读取其他函数内部变量的函数

闭包：闭包是作用域应用的特殊情况，有两种表现：
（1）函数作为参数被传递
（2）函数作为返回值被传递
简单理解：当一个嵌套的子函数引用了父函数的变量(函数)时, 就产生了闭包

    <button>按钮1</button>
    <button>按钮2</button>
    <button>按钮3</button>
    <button>按钮4</button>
    <button>按钮5</button>

js

```javascript
var btns = document.querySelectorAll('button')
    for (var i = 0; i < btns.length; i++) { //for循环是同步事件
        btns[i].onclick = function () {//点击是一个异步的过程,点击的时候,for循环已经循环完毕,所以会取最终值
            alert(i)//弹5
        }
    }
    for (var i = 0; i < btns.length; i++) { 
        btns[i].onclick = (function (i) {//立即执行0,1,2,3,4
            alert(i)
        })(i)
    }
    for (var i = 0; i < btns.length; i++) {
        (function(i) {//自调用匿名函数,形成了一个闭包,它会独自开辟一个空间存放传进来的i,
            btns[i].onclick = function () {//因为点击函数会用到父函数匿名函数的i,所以i被调用后不会被销毁
                alert(i)//点击执行0,1,2,3,4
            }
        })(i)
    }
    //es6=>let自带作用域
    for (let i = 0; i < btns.length; i++) { //for循环的时候因为点击事件会用到i,每次循环的i会保存在let的作用域大括号里
        btns[i].onclick = function () {
            alert(i)
        }
    }
```



#### apply、call和bind的区别

https://www.runoob.com/w3cnote/js-call-apply-bind.html

作用：用来改变函数this指向
相同处：第一个参数都是this要指向的对象
区别一：call()和apply()都是对函数的直接地调用，而bind()本身返回一个函数，后面还需要()进行调用
区别二：call()和bind()传参可以一个一个传入，但是apply()要以数组格式传参



```
var xw = {
                name : "小王",
                gender : "男",
                age : 24,
                say : function(school,grade) {
                 alert(this.name + " , " + this.gender + " ,今年" + this.age + " ,在" + school + "上" + grade);                                
                        }
                }
var xh = {
                name : "小红",
                gender : "女",
                age : 18
                }
```

`xw.say.call(xh,"实验小学","六年级");`
而对于apply来说是这样的

`xw.say.apply(xh,["实验小学","六年级"])`

看到区别了吗，call后面的参数与say方法中是一一对应的，而**apply的第二个参数是一个数组**，数组中的元素是和say方法中一一对应的，这就是两者最大的区别。

`xw.say.bind(xh,"实验小学","六年级")();`

但是由于bind返回的仍然是一个函数，所以我们还可以在调用的时候再进行传参。
`xw.say.bind(xh)("实验小学","六年级");`

如果`xw.say.bind(xh,["实验小学","六年级"])();`  在**实验小学，六年级**上 **undefined**

#### this指向

this的应用场景一般如下
(1)在普通函数使用
(2)使用call、apply、bind
(3)在对象方法中被调用
(4)在class方法中调用
(5)在箭头函数中使用
this的取值是**在函数执行的时候确定的，不是在函数定义的时候确定的**，这个规则适用于上面所有场景

```
function fn1(){
    console.log(this)
}
fn1() //window
fn1.call({x：100}) // {x:100}
const fn2=fn1.bind({x:200})
fn2() //{x:200}
```

这里直接fn1()打印的this是window
调用call之后打印是this是这个对象{ x: 100}
调用bind之后不会直接执行，返回另外一个函数，执行这个函数，this指向bind的对象{ x : 200 }

```
const ZhangSan={
    name:'zhangsan',
    age:18,
    sayHi(){
        console.log(this) //this即当前对象ZhangSan
    },
    wait(){
        setTimeout(function(){
            console.log(this) //window
        })
    }
    waitAgain(){
        setTimeout(()=>{
            console.log(this) //this即当前对象ZhangSan
        })
}
```

sayHi()里面的this就是当前对象，这里setTimeout里面的函数和普通函数一样，this就是window
**箭头函数中的this是上级作用域的this的值**，嵌套setTimeout也是一样，因为外层setTimeout的this是上级作用域的this，所以最内层的setTimeout中的this还是和外层的一样，即上级作用域的this。
**如果箭头函数被非箭头函数包含，this绑定的就是最近一层非箭头函数的this。**
原因箭头函数没有this，箭头函数的this是继承父执行上下文里面的this
注意：简单对象（非函数）是没有执行上下文this的！
如果父级f也是一个箭头函数，它就只能继续向上找也就是window了

#### 手写bind

```
  // 模拟 bind
Function.prototype.bind1 = function () {
    // 将参数拆解为数组 
    const args = Array.prototype.slice.call(arguments)

    // 获取 this（数组第一项）
    const t = args.shift()

    // fn1.bind(...) 中的 fn1
    const self = this

    // 返回一个函数
    return function () {
        return self.apply(t, args)
    }
}

function fn1(a, b, c) {
    console.log('this', this)
    console.log(a, b, c)
    return 'this is fn1'
}

const fn2 = fn1.bind1({x: 100}, 10, 20, 30)
const res = fn2()
console.log(res)

```

- 在es5标准中，我们经常需要把arguments对象转换成真正的数组
  Array.prototype.slice.call(arguments)的作用是将函数传入的参数转换为数组对象。那大家肯定有疑问，为什么不直接使用arguments对象呢？这里需要注意的是typeof arguments打印出的是object，可以看出arguments并不是真正的数组，它是一个对象。

- es6语法中新增了Array.from()，所以上述类型的对象可以Array.from(obj)就直接转化成数组！

- ```
  // 你可以这样写
  var arr = Array.prototype.slice.call(arguments)
  // 你还可以这样写
  var arr = [].slice.call(arguments)
  // 你要是不怕麻烦，你还可以这样写
  var arr = [].__proto__.slice.call(arguments)
  //以上三种写法是等价的。
  ```

#### 创建10个标签，点击的时候弹出对应的序号

```
// let i 这样写i是自由变量，每次点击都是10
let a; 
for(let i=0;i<10;i++){
    a=document.createElement('a');
    a.innerHTML=i+'<br>';
    a.addEventListener('click',function(e){
        e.preventDefault();
        alert(i)
    })
    document.body.appendChild(a)
}
```

#### 做一个简单的cache工具

```
// 闭包隐藏数据，只提供 API
function createCache() {
    const data = {} // 闭包中的数据，被隐藏，不被外界访问
    return {
        set: function (key, val) {
            data[key] = val
        },
        get: function (key) {
            return data[key]
        }
    }
}
 
const c = createCache()
c.set('a', 100)
console.log( c.get('a') )56
```

如果不调用set方法，就不能设置键值对，如果不调用get方法，就不能存储键值对
比如想要设置data.b = 101会显示data is not defined，无法访问data

## 异步和单线程

1. 异步和同步的区别，举个例子：基于js是单线程语言，**异步不会阻塞代码执行，同步会阻塞代码执行**。例子：alert 是同步，setTimeout 是异步
2. 前端异步两个主要应用场景
   (1)网络请求：如 ajax 请求，动态img加载
   (2)定时任务:如setTimeout、setInverval
3. 嵌套的回调地狱的问题导致了promise的出现，也就是说promise的出现，解决了callback嵌套的回调地狱(callback hell)问题

```
function loadImg(src) {
    const p = new Promise(
        (resolve, reject) => {
            const img = document.createElement('img')
            img.onload = () => {
                resolve(img)
            }
            img.onerror = () => {
                const err = new Error(`图片加载失败 ${src}`)
                reject(err)
            }
            img.src = src
        }
    )
    return p
}

const url1 = 'https://img.mukewang.com/5a9fc8070001a82402060220-140-140.jpg'
const url2 = 'https://img3.mukewang.com/5a9fc8070001a82402060220-100-100.jpg'

loadImg(url1).then(img1 => {
    console.log(img1.width)
    return img1 // 普通对象
}).then(img1 => {
    console.log(img1.height)
    return loadImg(url2) // promise 实例
}).then(img2 => {
    console.log(img2.width)
    return img2
}).then(img2 => {
    console.log(img2.height)
}).catch(ex => console.error(ex))
```

### 手写Promise.all

```javascript
 function promiseAll(promiseArray) {
      return new Promise((resolve, reject) => {
        if (!Array.isArray(promiseArray)) {
          reject(new Error('传入的参数必须是数组'))
        }
        const res = []
        const promiseNums = promiseArray.length
        let counter = 0 //用来判断length返回 
        for (let i = 0; i < promiseNums; i++) {
         // Promise.resolve可能不是Promise类型，这样也可以直接返回
          Promise.resolve(promiseArray[i]).then(value => {
            counter++
            res[i] = value // res.push()不可取，会有顺序问题，快的先Push
            if (counter == promiseNums) { //res.length==promiseNums不可取
                				//arr=[] arr[2]=1 arr.length=3 不可取 res.length
              resolve(res)
            }
          }).catch(e => reject(e))
        }
      })
    }
    const a = new Promise((res, req) => {
      setTimeout(() => {
        res('1')
      }, 1000);
    })
    const b = new Promise((res, req) => {
      setTimeout(() => {
        res('2')
      }, 2000);
    })
    const c = new Promise((res, req) => {
      setTimeout(() => {
        res('3')
      }, 3000);
    })
    promiseAll([b, a, c]).then(res => {
      console.log(res);
    })
```



## JS web api

![image-20200516172942633](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200516172942633.png)

### Dom

#### Dom本质

Dom本质是从html解析出来的一颗树，一层一层

DOM是Document Object Module的缩写。大概意思是浏览器给Javascript操作网页Html文档的一种树状数据结构，可以通过浏览器给Javascript设置的可以访问网页文档的全局变量如`document`、`window`来创建、获取、添加、删除浏览器Html文档上的节点

#### 获取Dom节点

```
// const divList = document.getElementsByTagName('div') // 标签名集合
// console.log('divList.length', divList.length)
// console.log('divList[1]', divList[1])

// const containerList = document.getElementsByClassName('container') // 样式集合
// console.log('containerList.length', containerList.length)
// console.log('containerList[1]', containerList[1])

// const pList = document.querySelectorAll('p')
// console.log('pList', pList)

// const pList = document.querySelectorAll('p')
// const p1 = pList[0]

// // property 形式  已有
// p1.style.width = '100px'
// console.log( p1.style.width )
// p1.className = 'red'
// console.log( p1.className )
// console.log(p1.nodeName) // p
// console.log(p1.nodeType) // 1

// // attribute  新增
// p1.setAttribute('data-name', 'imooc')
// console.log( p1.getAttribute('data-name') )
// p1.setAttribute('style', 'font-size: 50px;')
// console.log( p1.getAttribute('style') )
```

![image-20200516174800675](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200516174800675.png)

![image-20200601232736927](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200601232736927.png)

简单理解，Attribute就是dom节点自带的属性，例如html中常用的id、class、title、align等,而Property是这个DOM元素作为对象，其附加的内容，例如childNodes、firstChild等。另外，常用的Attribute，例如id、class、title等，已经被作为Property附加到DOM对象上，可以和Property一样取值和赋值。即只要是DOM标签中出现的属性（html代码），都是Attribute。然后有些常用特性（id、class、title等）会被转化为Property。可以很形象的说，这些特性/属性，是“脚踏两只船”的。



总结：property和attribute形式都可以修改节点的属性，但是对于新增或删除的自定义属性，能在html的dom树结构上体现出来的，就必须要用到attribute形式了。
最后注意：“class”变成Property之后叫做“className”，因为“class”是ECMA的关键字。以下代码等价：

```
var className = div1.className;
var className1 = div1.getAttribute("class");
```

《js高级程序设计》中提到，为了方便操作，建议大家用setAttribute()和getAttribute()来操作即可。
另一种说法：虽说修改属性可能会导致DOM重新渲染，但推荐优先使用property形式，因为可能在内置js机制中优化，避免一些不必要的DOM渲染，而使用attribute形式更有可能会重新渲染

#### Dom结构操作

```
// 新建节点
const newP = document.createElement('p')
newP.innerHTML = 'this is newP'
// 插入节点
div1.appendChild(newP)

// 移动节点
const p1 = document.getElementById('p1')
div2.appendChild(p1)

// 获取父元素
console.log( p1.parentNode )

// 获取子元素列表
const div1ChildNodes = div1.childNodes
console.log( div1.childNodes )
const div1ChildNodesP = Array.prototype.slice.call(div1.childNodes).filter(child => {
    if (child.nodeType === 1) {
        return true
    }
    return false
})
// 过滤filter之后只剩下p元素，filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。
console.log('div1ChildNodesP', div1ChildNodesP)
// 删除节点
div1.removeChild( div1ChildNodesP[0] )
```

#### Dom性能

(1)对DOM查询做缓存操作

```
for(var i=0;i<document.get...id('**');i++) // 不能频繁操作Dom

var a=document.get...id('**').length
for(var i=0;i<length;i++) // 缓存
```

(2)将频繁操作改为一次性操作(创建文档片段)

```
const list =document.getElementById('list')
//创建一个文档片段，此时还没有插入到dom树种
const frag=document.createDocumentFragment()
//先插入文档片段中
 for(let i=0;i<20;i++){
    const li = document.createElement('li')
    li.innerHTML=`list item ${i}`
    frag.appendChild(li)
}
//都完成之后，再统一插入到Dom结构中
 list.appendChild(frag)
```

在执行最后list.apendChild(frag)之前，frag片段还在内存中，没有渲染，执行了之后就渲染在页面上
综上所述，DOM性能就是要记得做缓存和合并的处理，合并就是不要添加一次渲染一次。一次性添加到fragment再添加在DOM节点，这样只会渲染一次。

### Bom操作

#### 知识

![image-20200518132842754](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200518132842754.png)

1. Navigator 对象 Navigator 对象包含有关浏览器的信息。
   如何识别浏览器类型？
   navigator.userAgent就是获取浏览器的类型

2. screen.width /height 主要获取屏幕的宽和高

3. history

   ```
   history.back(); // 浏览器后退
   history.forward();// 浏览器前进
   ```

4. 拆解url

   ```
   //https://coding.imooc.com/class/chapter/400.html?a=100&b=200#Anchor
   
   location.href // 整个链接
   location.protocal // 协议https
   location.pathname // /class/chapter/400.html
   location.search // ?a=100&b=200
   location.hash //#Anchor 哈希值
   location.host //coding.imooc.com 域名
   ```

#### 事件

1. 事件对象event:JS在事件处理函数中提供了事件对象，帮助处理鼠标和键盘事件。同时还可以修改一些事件的捕获和冒泡流的函数。

   事件处理分为三部分：对象.事件处理函数=函数

   ```
   document.οnclick=function(){ 
       alert(this.value); //this代表着在该作用域中离它最近的对象。
   }
   ```

以上事件处理中，document为对象，click是事件处理类型，onclick为事件处理函数。function()为匿名函数，用于触发后执行。
那么什么是事件对象呢？当我们触发document的click事件时，便会产生一个事件对象，这个对象中包含着这个事件的相关信息，包括导致事件的元素、事件的类型、以及其它与特定事件相关的信息等。这个对象是在执行事件时，浏览器通过函数传递过来的。

```
document.οnclick=function(){
	alert(arguments.length); //浏览器会默认传递一个参数
	alert(arguments[0]);//[object MouseEvent]，如果是keydown,则为[object KeyboardEvent]
}
```

可以看出，事件处理中，浏览器已经默认将一个参数传递过来了。而在普通函数和匿名函数中，是不传递event对象的。
event对象的接收：
在W3C中可以直接接受event对象，如：

```
input.onclick = function (evt) { //接受 event 对象，名称不一定非要 event
	alert(evt); //MouseEvent，鼠标事件对象 IE不支持
};
```

#### 事件绑定和事件冒泡

在JavaScript中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能，因为需要不断的与dom节点进行交互，访问dom的次数越多，引起浏览器重绘与重排的次数也就越多，就会延长整个页面的交互就绪时间，这就是为什么性能优化的主要思想之一就是减少DOM操作的原因；每个函数都是一个对象，是对象就会占用内存，对象越多，内存占用率越大，100个li就要占用100个内存空间。如果要用事件委托，就会将所有的操作放到js程序里面，**只对它的父级(如果只有一个父级)这一个对象进行操作**，与dom的操作就只需要交互一次，这样就能大大的减少与dom的交互次数，提高性能；



```javascript
function bindEvent(elem, type, fn, selector) {
    elem.addEventListener(type, event => {
        const target = event.target
        if (selector) {
            // 代理绑定
            if (target.matches(selector)) {
                fn.call(target, event)
            }
        } else {
            // 普通绑定
            fn.call(target, event)
        }
    },selector)
}

// 普通绑定
const btn1 = document.getElementById('btn1')
bindEvent(btn1, 'click', function (event) {
    // console.log(event.target) // 获取触发的元素
    event.preventDefault() // 阻止默认行为
    alert(this.innerHTML)
})

// 代理绑定
const div3 = document.getElementById('div3')
bindEvent(div3, 'click', function (event) {
    event.preventDefault(),
    alert(this.innerHTML)
},'a')

 <ul>
    <li onclick="console.log(666)">1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
  </ul>
const ul = document.querySelector('ul')
    ul.addEventListener('click', (e) => {
      // e.stopPropagation() 
      const target = e.target
      if (target.tagName.toLowerCase() == 'li') {
        const liList = document.querySelectorAll('li')
        //不是个真正的数组，所以用Array原型链上的方法
        const index = Array.prototype.indexOf.call(liList, target)
        console.log(`内容${target.innerHTML},下标${index}`)
      }
    })
场景题：一个历史页面,上面有若干按钮的点击逻辑,每个按钮都有自己的 click事件。
新需求来了:给每一个访问的用户添加了一个属性, banned=true,此用户点击页面上的任何按钮或者元素,都不可响应原来的函数。而是直接 alert提示,你被封禁了
window.addEventListener('click',()=>{
	if(banned){
        e.stopPropagation()
    }    
},true)// 开启捕获
```

JavaScript三种绑定事件的方式：
在JavaScript中，有三种常用的绑定事件的方法：

1. 在DOM元素中直接绑定；(1)

   在JavaScript代码中绑定；(2)

   绑定事件监听函数;(3)

   (3)事件是在(2)级事件的基础上添加很多事件类型。

   ```
   (1). <div id="btn" onclick="clickone()"></div> //直接在DOM里绑定事件
   　   <script>
   　   function clickone(){ alert("hello"); }
   　　 </script>
   (2). <div id="btn"></div>
   　　　　<script>
   　　　　document.getElementById("btn").onclick = function(){ alert("hello"); } //脚本里面绑定
   　　　　</script>
   (3). <div id="btn"></div>
   　　　<script>
   　　　document.getElementById("btn").addeventlistener("click",clickone,false); //通过侦听事件处理相应的函数
   　　　　function clickone(){ alert("hello"); }
   　　　　</script>
   ```

   那么问题来了，1 和 2 的方式是我们经常用到的，那么既然已经有两种绑定事件的方法为什么还要有第三种呢？答案是这样的：

   **用 “addeventlistener” 可以绑定多次同一个事件，且都会执行，而在DOM结构如果绑定两个 “onclick” 事件，只会执行第一个**；**在脚本通过匿名函数的方式绑定的只会执行最后一个事件**。

   ```
   　(1). <div id="btn" onclick="clickone()" onclick="clicktwo()"></div> 
   　　　　<script>
   　　　　function clickone(){ alert("hello"); } //执行这个
   　　　　function clicktwo(){ alert("world!"); }
   　　　　</script>
   　(2). <div id="btn"></div>
   　　　　<script>
   　　　　　document.getElementById("btn").onclick = function（）{ alert("hello"); }
   　　　　　document.getElementById("btn").onclick = function（）{ alert("world"); } //执行这个
   　　　　</script>
   　　1. <div id="btn"></div>
   　　　　<script>
   　　　　　document.getElementById("btn").addeventlistener("click",clickone,false);
   　　　　　function clickone(){ alert("hello"); } //先执行
   　　　　　document.getElementById("btn").addeventlistener("click",clicktwo,false);
   　　　　　function clicktwo(){ alert("world"); } //后执行
   　　　　</script>
   ```

   以上；可根据场景灵活选择。

2. 描述事件冒泡的流程
   **基于DOM树形结构，事件会顺着触发元素往上冒泡。**
   **事件冒泡的应用场景就是事件代理**
   **event.stopPropagation() // 阻止冒泡**

3. 事件委托又叫事件代理

4. JavaScript中matches(matchesSelector)：
   规范中，为DOM节点添加了一个方法，主要是用来判断当前DOM节点不否能完全匹配对应的CSS选择器规则；如果匹配成功，返回true，反之则返回false。千万不要和字符串对象的match()方法混淆。语法如下：
   element.matches(selector);
   JavaScript match() 方法：
   match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。

### ajax

https://www.cnblogs.com/soyxiaobi/p/9616011.html

#### json和jsonp区别

JSON和JSONP虽然只有一个字母的差别，但其实他们根本不是一回事儿：JSON是一种数据交换格式，而JSONP是一种依靠开发人员的聪明才智创造出的一种非官方跨域数据交互协议。我们拿最近比较火的谍战片来打个比方，JSON是地下党们用来书写和交换情报的“暗号”，而JSONP则是把用暗号书写的情报传递给自己同志时使用的接头方式。看到没？一个是描述信息的格式，一个是信息传递双方约定的方法。


1. JSON.stringify()的作用是将 JavaScript 值转换为 JSON 字符串，而JSON.parse()可以将JSON字符串转为一个对象。

   简单点说，它们的作用是相对的，我用JSON.stringify()将对象a变成了字符串c，那么我就可以用JSON.parse()将字符串c还原成对象a。

   ```
   let arr = [1,2,3];
   JSON.stringify(arr);//'[1,2,3]'
   typeof JSON.stringify(arr);//string
   
   let string = '[1,2,3]';
   console.log(JSON.parse(string))//[1,2,3]
   console.log(typeof JSON.parse(string))//object
   ```

2. AJAX 不是新的编程语言,而是一种使用现有标准的新方法。AJAX 是与服务器交换数据并更新部分网页的艺术,在不重新加载整个页面的情况下。如需将请求发送到服务器，使用XMLHttpRepues对象的open和send方法
   xhr.open(“get”,”/test.json”,true)
   第三个参数规定应当对请求进行异步地处理**(true（异步）或 false（同步）)**
   xhr.send(null)
   xhr.send(string)仅用于post请求

3. HTTP状态码
   1xx 信息状态码
   2xx 成功状态码
   3xx 重定向
   4xx 客户端请求错误
   5xx 服务端错误
   200 ok 请求成功。一般用于GET与POST请求
   301 Moved Permanently 永久重定向
   302 临时重定向
   304 Not Modified 所请求的资源未修改
   403 Forbidden 服务器禁止访问
   404 Not Found 请求的资源（网页等）不存在

   

   #### 什么是同源策略？

   ajax请求时，浏览器要求和当前网页的server必须同源（安全）
   什么是同源？
   同源：协议、域名、端口，三者必须一致
   比如前端http://a.com:8080/
   server是https://b.com/api/xxx
   比如这个前端网页上发ajax去请求server页面的数据，浏览器肯定会截获，不符合同源，不管是get还是post都不会让你发
   服务端server不一样，没有同源策略，可以去获取其他的数据
   可以不遵守同源策略的情况：加载图片img、css、js可以无视同源策略
   不仅如此，我们还发现凡是拥有”src”这个属性的标签都拥有跨域的能力
   img可用于统计打点，可使用第三方统计服务
   link script可使用CDN，CDN一般都是外域
   script可实现JSONP
   CDN (Content Delivery Network) 内容分发网络

4. 跨域访问

   (1）JSONP（json with padding 填充式json）
   **JSONP的原理：ajax请求受同源策略影响，不允许进行跨域请求，而script标签src属性中的链接却可以访问跨域的js脚本，利用这个特性，服务端不再返回JSON格式的数据，而是返回一段调用某个函数的js代码，在src中进行了调用，这样实现了跨域。**
   (2)CORS（Cross-origin resource sharing 跨域资源共享）
   服务器设置 http header

   ![image-20200519132649344](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200519132649344.png)

   #### 手写一个简易的ajax

   ```
   function ajax(url) {
       const p = new Promise((resolve, reject) => {
           const xhr = new XMLHttpRequest()
           xhr.open('GET', url, true)
           xhr.onreadystatechange = function () {
               if (xhr.readyState === 4) {
                   if (xhr.status === 200) {
                       resolve(
                           JSON.parse(xhr.responseText)
                       )
                   } else if (xhr.status === 404 || xhr.status === 500) {
                       reject(new Error('404 not found'))
                   }
               }
           }
           xhr.send(null)
       })
       return p
   }
   //引用
   const url = '/data/test.json'
   ajax(url)
   .then(res => console.log(res))
   .catch(err => console.error(err))
   ```

   ```
   const x = Error('404 not found');
   console.log(x); // Error:404 not found
   ```

   Promise其实是一个构造函数，它有resolve，reject，race等静态方法;它的原型（prototype）上有then，catch方法，因此只要作为Promise的实例，都可以共享并调用Promise.prototype上面的方法(then,catch)
   (1)resolve的东西，一定会进入then的第一个回调，肯定不会进入catch；
   (2)reject后的东西，一定会进入then中的第二个回调，如果then中没有写第二个回调，则进入catch

5. ajax的常用插件
   (1)jquery
   (2)fetch
   (3)axios 基于XMLHttpRequest，多用于三大框架

## cookie和Strorage存储

Cookie、localStorage、sessionStorage三者的区别

1.1、存储大小：Cookie ->4K Storage ->5M

1.2、有效期：Cookie有用有效期，Storage永久储存

1.3、Cookie会发送到服务器端，存储在内存中(sessionStorage也是存储在内存中)，Storage只存储在浏

览器端(localStorage)

1.4：路径：Cookie有路径限制，Storage只存储在域名下

1.5：API:Cookie没有特定的API，Storage有对应的API

1. cookie的特点
   (1).本身用于浏览器和server通讯·
   (2).是http请求的一部分
   (3).是被”借用”到本地存储上来（localstorage和sessionstorage是H5之后才提出的，存储不是cookie主要应该做的事）
   (4).只可用document.cookie = ‘….’来修改（前后端都可以修改cookie）
   (5).每次只能添加一个key：value，没有的追加相同的覆盖
   cookie的缺点
   (1).存储大小最大是4KB
   (2).http请求时需要发送到服务端，增加请求数据量（刷新一个网页是有很多请求的，比如上面的图，如果每个请求都带上这个cookie，那将是一个非常大的开销）
   (3).只能用document.cookie = ‘…’来修改，太过于简陋
2. html5存储
   localStorage和sessionStorage特点
   (1).localStorage和sessionStorage是HTML5专门为存储设计的，每个域最大可存储5M（对于前端存储字符串这个空间够大了）
   (2).localStorage和sessionStorage的API简单易用setItem getItem
   (3).不会随着http请求被发送出去（cookie就会被发送出去，要是localStorage和sessionStorage随着http请求发送出去手机流量早就没了）
   `localStorage.setItem('a',100)`
   localStorage和sessionStorage区别
   (1).localStorage数据会永久存储，除非代码或手动删除
   (2).sessionStorage数据只存在于当前会话，浏览器关闭则清空
   (3).一般用localStorage更多一些
3. cookie和html5存储的区别
   (1)容量
   (2)API易用性
   (3)是否跟随http请求发送出去

## 开发环境

### git

- git常用命令

  git init //当前目录
  git init newrepo //指定目录
  $ git add *.c //指定文件
  $ git add . //全部文件
  $ git add README
  $ git commit -m ‘初始化项目版本’
  使用 git add 命令将想要快照的内容写入缓存区， 而执行 git commit 将缓存区内容添加到仓库中。
  git clone //当前目录
  git clone //指定目录
  git status //命令用于查看项目的当前状态
  git status -s 以精简的方式显示文件状态。
  git status 输出的命令很详细，但有些繁琐。
  如果用 git status -s 或 git status –short 命令，会得到更为紧凑的格式输出。
  新添加的未跟踪文件前面有 ?? 标记，
  新添加到暂存区中的文件前面有 A 标记，
  修改过的文件前面有 M标记。
  git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。
  如果你觉得 git add 提交缓存的流程太过繁琐，Git 也允许你用 -a 选项跳过这一步。
  命令格式如下：git commit -a
  git commit -am ‘修改 hello.php 文件’
  git reset HEAD 命令用于取消已缓存的内容,即取消之前 git add 添加。这时我们可以使用以下命令将 hello.php 的修改提交：
  $ git commit -am ‘修改 hello.php 文件
  创建分支命令：git branch (branchname) 依然在当前分支
  切换分支命令:git checkout (branchname)
  当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。
  合并分支命令:git merge
  echo ‘runoob.com’ > test.txt // 新建 txt文件内容为runoob.com
  也可以使用 git checkout -b (branchname) 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。
  git rm (filename) 全局删除
  git branch -d (branchname)
  使用 git log 命令列出历史提交记录
  git stash应用场景：
  (1)当正在dev分支上开发某个项目，这时项目中出现一个bug，需要紧急修复，但是正在开发的内容只是完成一半，还不想提交，这时可以用git stash命令将修改的内容保存至堆栈区，然后顺利切换到hotfix分支进行bug修复，修复完成后，再次切回到dev分支，从堆栈中恢复刚刚保存的内容。
  (2)由于疏忽，本应该在dev分支开发的内容，却在master上进行了开发，需要重新切回到dev分支上进行开发，可以用git stash将内容保存至堆栈中，切回到dev分支后，再次恢复内容即可。
  总的来说，git stash命令的作用就是将目前还不想提交的但是已经修改的内容进行保存至堆栈中，后续可以在某个分支上恢复出堆栈中的内容。这也就是说，stash中的内容不仅仅可以恢复到原先开发的分支，也可以恢复到其他任意指定的分支上。git stash作用的范围包括工作区和暂存区中的内容，也就是说没有提交的内容都会保存至堆栈中。
  git stash pop
  将当前stash中的内容弹出，并应用到当前分支对应的工作目录上。
  注：该命令将堆栈中最近保存的内容删除（栈是先进后出）
  git stash apply
  将堆栈中的内容应用到当前目录，不同于git stash pop，该命令不会将内容从堆栈中删除，也就说该命令能够将堆栈的内容多次应用到工作目录中，适应于多个分支的情况。
  git push -u origin master 上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。

### 调试工具

### 抓包

移动端的h5页面查看网络请求的时候，需要用工具抓包
windows一般用fiddler，Mac OS一般用charles，我个人是用的charles (windows)

### webpack babel

1. webpack搭建环境(开发)
   (1)npm init -y

-y 的含义：yes的意思，在init的时候省去了敲回车的步骤，生成的默认的package.json
(2)cnpm install webpack webpack-cli -D
(3)在这里目webpack-demo目录下创建src目录
(4)接着在src里面新建index.js //console.log(“this is index js”)
(5)然后在webpack-demo目录新建webpack.config.js，这就是webpack默认的配置文件的名字
在webpack.config.js内容如下：

```
const path = require('path')
module.exports = {
    mode: 'development', // production
    entry: path.join(__dirname, 'src', 'index.js'),//__dirname就是当前目录(同一套代码不同环境可能当前环境路径字符串不一样,__dirname能统一表示)
    //拼接src再拼接index.js，就能找到整个文件的入口
    output: {
        filename: 'bundle.js',
        path: path.join(__dirname, 'dist')
    }
}
```

(6)接着准备运行了，我们来到package.json，在scripts里面加上”build”: “webpack”，
webpack打包的时候默认配置文件是webpack.config.js，所以不用指定webpack –config webpack.config.js，除非配置文件是其它名字
(7)接着我们执行命令，npm run build，我们指定了build为webpack，所以等同于你执行npm run webpack
(8)在src下面新建index.html
(9)接着还得安装一个解析html的插件html-webpack-plugin
cnpm install html-webpack-plugin -D
(10)继续安装一个能启动服务的插件webpack-dev-server
cnpm install webpack-dev-server -D
(11)接下来要去webpack.config.js去加上插件和开启本地服务的配置
(12)然后在package.json的scripts里面加上一句”dev”: “webpack-dev-server”
(13)然后命令行执行npm run dev 执行之后看到服务器开起来了，端口3000 ，运行结果如下，就是index.html的内容
\5. webpack-babel
将ES6转换成ES5语法，因为有的浏览器太落后，ES6语法无法识别
babel就是js的高级语法向低级语法转变的工具，babel可以提供插件给webpack用
(1)执行命令cnpm i @babel/core @babel/preset-env babel-loader
i就是install的简写，@就是组，@babel/core表示安装babel组里面的core模块，env是babel的配置插件集合，babel-loader就是给webpack用的一个插件
(2)接着在webpack-demo目录下新建.babelrc文件（和webpack.config.js在同一目录）

```
{
    "presets": [
        "@babel/preset-env"
    ]
}
```

(3)接着修改webpack.config.js，主要是加上module模块
(4)module模块就是针对不同模块做不同的解析
\6. webpack-ES6-Module（ES6的模块化）
用模块化就是想用一个导出导入的过程
export和export default的区别
(1)export命令用于规定模块的对外接口
一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用export关键字输出该变量。下面是一个 JS 文件，里面使用export命令输出变量。

```
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```

上面代码是profile.js文件，保存了用户信息。ES6 将其视为一个模块，里面用export命令对外部输出了三个变量。
export的写法，除了像上面这样，还有另外一种。

```
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;
export {firstName, lastName, year};
```

上面代码在export命令后面，使用大括号指定所要输出的一组变量。它与前一种写法（直接放置在var语句前）是等价的，但是应该优先考虑使用这种写法。因为这样就可以在脚本尾部，一眼看清楚输出了哪些变量。
export命令除了输出变量，还可以输出函数或类（class）。

```
export function multiply(x, y) {
  return x * y;
};
```

上面代码对外输出一个函数multiply。
export命令对外输出了指定名字的变量（变量也可以是函数或类）。
与export default命令的区别：import命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。
如果想为输入的变量重新取一个名字，import命令要使用as关键字，将输入的变量重命名。
`import { lastName as surname } from './profile.js';`
export default命令，为模块指定默认输出。
使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。但是，用户肯定希望快速上手，未必愿意阅读文档，去了解模块有哪些属性和方法。
为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到export default命令，为模块指定默认输出。

```
// export-default.js
export default function () {
  console.log('foo');
}
```

上面代码是一个模块文件export-default.js，它的默认输出是一个函数。
与export命令的区别：其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。

```
// import-default.js
import customName from './export-default';
customName(); // 'foo'
```

上面代码的import命令，可以用任意名称指向export-default.js输出的方法，这时就不需要知道原模块输出的函数名。需要注意的是，这时import命令后面，不使用大括号。
本质上，export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。所以，下面的写法是有效的。

```
// modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
```

正是因为export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句。
总结：export命令对外接口是有名称的且import命令从模块导入的变量名与被导入模块对外接口的名称相同，而export default命令对外输出的变量名可以是任意的，这时import命令后面，不使用大括号。
export default命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此export default命令只能使用一次。所以，import命令后面才不用加大括号，因为只可能唯一对应export default命令。

7.webpack-配置生产环境
现在的webpack.config.js是开发环境的webpack配置，但是不能直接上线，体积又大运行又慢，接下来我们来看看怎么配置一个生产环境的打包
(1)我们在当前目录新建一个生产环境的配置文件webpack.prod.js，prod就是production的简写
(2)接着我们需要去package.json去修改”build”，因为”build”的value还是”webpack”，默认运行的开发环境配置webpack.config.js，它会去启动我们写的服务devServer，那我们怎么办呢？
来改一改package.json，我们上面讲了，”build” : “webpack”就是”build” : “webpack –config webpack.config.js”，而现在我们需要指定生产环境的配置，所以需要变成”build” : “webpack –config webpack.prod.js”
(3)接着再次执行npm run build，现在运行的就是生产环境的配置webpack.prod.js了
(4)bundle.hash值.js，即配置的[contenthash]的结果，主要是命中缓存，进行性能优化
我们可以看到bundle.hash值.js的内容都是经过压缩的，压缩后的代码体积又小运行又快，下载也快，所以生产环境必须单独配置。

### linux

mkdir 创建文件夹

touch 新建文件

ls平铺看文件

ll列表看文件

rm -rf 文件夹名  删除文件  rm 文件名

mv 文件  新文件名  修改文件名

mv 文件名+tab补全  ../../ 移动文0.0件

cp 文件名 文件名  拷贝

vi 没有文件自动创建，修改文件内部  进去后点i编辑  退出ESC  :w写入保存 :q退出  不想保存强制退出:q!

vim 看文件跟vi一样

cat 文件名 看全部

grep "关键字"  查地方



## 正则表达式

## 简介

(1).Regular Expression 使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。
按某种规则去匹配符合条件的字符串。
不同编程语言的正则表达式略有不同。
(2).一个正则可视化网站：https://regexper.com/



1. RegExp对象

   JavaScript通过内置对象 RegExp 支持正则表达式，有两种方法实例化RegExp对象：字面量和构造函数。

   （1）字面量

   ```
   // 实例化一个正则表达式，匹配字符串中的is单词
   var reg = /\bis\b/g;
   'She is girl, This is a computer.'.replace(reg, 'IS');
   // 结果 'She IS girl, This IS a computer.'
   ```

   (2)构造函数

   ```
   var reg = new RegExp('\\bis\\b', 'g'); 
   'She is girl, This is a computer.'.replace(reg, 'IS');
   // 结果 'She IS girl, This IS a computer.'
   ```

   注：var reg = new RegExp()接受两个字符串参数，此外需要注意双重转义

g:global 全文搜索，不添加时会搜索到第一个匹配停止
i: ignore case 忽略大小写，默认大小写敏感
m： multiple lines多行搜索
\2. 正则表达式由两种基本字符类型组成：原义文本字符和元字符。
(1).原义文本字符：代表它原来含义的字符 例如：abc、123
(2).元字符：在正则表达式中有特殊意义的非字母字符 例如：\b表示匹配单词边界，而非\b。在正则表达式中具体特殊含义的字符：* + ? $ ^ . \ () {} [] ，！！！因此这些字符如果用到需要转义\。
字符 含义
\t 水平制表符
\v 垂直制表符
\n 换行符
\r 回车符
\0 空字符
\f 换页符
\cX 与X对应的控制字符(ctrl+X)

1. 字符类
   (1).一般情况下正则表达式一个字符对应字符串一个字符，表达式ab\t的含义是’ab’ + table键
   (2).匹配一类字符[]
   可以使用元字符[]来构建一个简单的类。所谓类就是指符合某些特性的对象，一个泛指，而不是特指某个字符。表达式[abc]把字符a或b或c归为一类，表达式可以匹配这类的字符，即可以匹配a或b或c中的任一个。
   [] 构建一个简单的类:
   ‘a1b2c3d4’.replace(/[abc]/g, ‘X’); // 结果：’X1X2X3d4’
   ‘a1b2c3d4’.replace(/[abc]/, ‘X’); // 结果：’X1b2c3d4’
   字符类取反
   ^ 不属于某个类的内容:’a1b1c1d1’.replace=(‘/[^abc]/‘,’0’);
   ‘a1b2c3d4’.replace(/[^abc]/g, ‘X’); // 结果：’aXbXcXXX’

2. 范围类

   使用[a-z]来连接两个字符表示从a到z的任意字符，这是一个闭区间，也就是包含a和z本身。

   ```
   'a1b2c3D4'.replace(/[a-zA-Z]/g, 'X'); // 结果：'X1X2X3X4'
   // 匹配数字
   '2017-05-01'.replace(/[0-9]/g, 'X'); // 结果：'XXXX-XX-XX'
   // 匹配横线
   '2017-05-01'.replace(/[0-9-]/g, 'X'); // 结果：'XXXXXXXXXX'
   ```

3. (1)预定义类

   字符 等价类 含义

   . [^\r\n] 除回车符和换行符之外的所有字符

   \d [0-9] 数字字符 \D [^0-9] 非数字字符

   \s [\t\n\xOB\f\r] 空白符

   \S [^\t\n\xOB\f\r] 非空白符

   \w [a-zA-Z_0-9] 单词字符(字母、数字、下划线)

   \W [^a-zA-Z_0-9] 非单词字符

   (2)边界

   几个常见的边界匹配字符

   字符 含义

   ^ 以xxx开始

   $ 以xxx结束

   \b 单词边界

   \B 非单词边界

   ```
   'This is a dog'.replace(/\bis\b/g,'X')
   "This X a dog"
   'This is a dog'.replace(/\Bis\b/g,'X')
   "ThX is a dog"
   'This is a dog'.replace(/\Bis\B/g,'X')
   "This is a dog"
   ```

   \b，\B是字母边界，不匹配任何实际字符，所以是看不到的；\B是\b的非(补)。

   \b：表示字母数字与非字母数字的边界，非字母数字与字母数字的边界。

   \B：表示字母数字与字母数字的边界，非字母数字与非字母数字的边界。

   注意：^符号不在中括号[]内，则表示以xxx开始，而非取反的意思。

   ```
   '@123@abc@'.replace(/@./g, 'X'); // 'X23Xbc@'
   '@123@abc@'.replace(/^@./g, 'X'); // 'X23@bc@'
   '@123@abc@'.replace(/.@/g, 'X'); // '@23XbcX'
   mulSrt
   "@123
   @456
   @789
   "
   mulSrt.replace(/^\d/g,'x');//
   "x123
   @456
   @789
   "
   mulSrt.replace(/^\d/gm,'x');//
   "x123
   x456
   x789
   "
   ```

4. 量词
   字符 含义
   ‘?’ 出现零次或一次(最多出现一次)
   ‘+’ 出现一次或多次(至少出现一次)
   ‘*’ 出现零次或多次(任意次)
   {n} 出现n次
   {n, m} 出现n到m次
   {n,} 至少出现n次
   {0,n} 最多出现n次

5. 贪婪模式与非贪婪模式
   (1).贪婪模式
   尽可能多的匹配。
   ‘123456789’.replace(/\d{3,6}/, ‘X’); // X789
   (2).非贪婪模式
   尽可能少的匹配，也就是说一旦成功匹配就不再继续尝试。方法：在量词后面加上 ?即可。
   ‘123456789’.match(/\d{3,6}?/g); // [‘123’, ‘456’, ‘789’]

6. 分组

   (1)分组

   使用()可以达到分组的功能，使量词作用于分组

   ```
   // 匹配字符串 'Byron' 连续出现3次的场景。
    Byron{3}  // 匹配'n'连续出现了3次的场景
   (Byron){3} // 匹配'Byron'连续出现了3次的场景 
   // 匹配字母+数字连续出现3次的场景
   'a1b2c3d4'.replace(/[a-z]\d{3}/g, 'X'); // a1b2c3d4
   'a1b2c3d4'.replace(/([a-z]\d){3}/g, 'X'); // Xd4
   ```

   (2)或

   使用 |可以达到或的效果

   ```
   'ByronCasper'.replace(/Byron|Casper/g, 'X'); // XX
   'ByronsperByrCasper'.replace(/Byr(on|Ca)sper/g, 'X'); // XX
   ```

   (3)反向引用(分组捕获)

   // 实现：2017-05-01 => 05/01/2017

   ‘2017-05-01’.replace(/(\d{4})-(\d{2})-(\d{2})/g, ‘$2/$3/$1’); // 05/01/2017

   (4).忽略分组

   不希望捕获某些分组，只需在分组内加上?:即可

   (?:Byron).(ok)

7. 前瞻（后顾js不支持）

   正则表达式从文本头部向尾部开始解析，文本尾部方向，称为前。

   前瞻就是在正则表达式匹配到规则的时候，向前检查是否符合断言，后顾/后瞻方向相反。

   JavaScript不支持后顾。

   符合与不符合特定断言称为肯定/正向匹配和否定/负向匹配

   名称 正则 备注

   正向前瞻 exp(?=assert)

   负向前瞻 exp(?!assert)

   正向后顾 exp(?<=assert) JavaScript不支持

   负向后顾 exp(?<!assert) JavaScript不支持

   ```
   'a2*34v8' .repalce(/\w(?=\d)/g, 'X'); // X2*X4X8
   'a2*34vv' .repalce(/\w(?!\d)/g, 'X'); // aX*3XXX
   ```

   助记：“正则表达式是从文本头部向尾部解析”。这就像在走路，没走过的路在你的前面，需要你往前看（前瞻）；走过的路需要你回头看（后顾）

8. 对象属性
   字符 含义 默认值
   g global 全文搜索，不添加则搜索到第一个匹配停止 flase
   i ignoreCase 忽略大小写，默认大小写敏感 flase
   m multipleline 多行搜索 flase
   l lastIndex 当前表达式匹配内容的最后一个字符的下一个位置
   s source 正则表达式的文本字符串
   正则表达式可对其进行调用x.global …..等属性
   但这些属性是只读不能写的

9. 正则表达式对象的test和exec方法
   (1)test() 方法用于检测一个字符串是否匹配某个模式。
   RegExpObject.test(string);
   string 必需。要检测的字符串。
   返回值:
   如果字符串 string 中含有与 RegExpObject 匹配的文本，则返回 true，否则返回 false。
   说明
   调用 RegExp 对象 r 的 test() 方法，并为它传递字符串 s，与这个表示式是等价的：(r.exec(s) != null)
   lastIndex 我的理解是下次匹配开始的位置
   js 都是从0开始计数的
   reg = /\w/g str = ‘ab’
   lastIndex = 0; 匹配从str[0] 开始 reg.test(‘str’) = true
   lastIndex = 1; 匹配从str[1] 开始 reg.test(‘str’) = true
   lastIndex = 2; 匹配从str[2] 开始 str[2] = undefined 所以 reg.test(‘str’) = false
   注意：非全局下lastIndex不生效
   (2)
   exec() 方法用于检索字符串中的正则表达式的匹配。
   使用正则表达式模式对字符串执行搜索，并将更新全局RegExp对象的属性以反映匹配结果
   RegExpObject.exec(string)
   返回值：
   返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。
   说明：
   exec() 方法的功能非常强大，它是一个通用的方法，而且使用起来也比 test() 方法以及支持正则表达式的 String 对象的方法更为复杂。
   如果 exec() 找到了匹配的文本，则返回一个结果数组。否则，返回 null。此数组的第 0 个元素是与正则表达式相匹配的文本，第 1 个元素是与 RegExpObject 的第 1 个子表达式相匹配的文本（如果有的话），第 2 个元素是与 RegExpObject 的第 2 个子表达式相匹配的文本（如果有的话），以此类推。除了数组元素和 length 属性之外，exec() 方法还返回两个属性。index 属性声明的是匹配文本的第一个字符的位置。input 属性则存放的是被检索的字符串 string。我们可以看得出，在调用非全局的 RegExp 对象的 exec() 方法时，返回的数组与调用方法 String.match() 非全局调用时返回的数组是相同的。
   但是，当 RegExpObject 是一个全局正则表达式时，exec() 的行为就稍微复杂一些。它会在 RegExpObject 的 lastIndex 属性指定的字符处开始检索字符串 string。当 exec() 找到了与表达式相匹配的文本时，在匹配后，它将把 RegExpObject 的 lastIndex 属性设置为匹配文本的最后一个字符的下一个位置。这就是说，您可以通过反复调用 exec() 方法来遍历字符串中的所有匹配文本。当 exec() 再也找不到匹配的文本时，它将返回 null，并把 lastIndex 属性重置为 0。

10. 字符串对象的search、match、split、replace方法
    方法 描述
    search 检索与正则表达式相匹配的值
    match 找到一个或多个正则表达式的匹配
    replace 替换与正则表达式匹配的子串
    split 把字符串分割为字符串数组
    (1).search()方法用于检索字符串中指定的字符串，或检索与正则表达式相匹配的子字符串
    方法返回一个匹配结果index，查不到返回-1
    search（）方法不执行全局匹配，它将忽略标志g，并且总是从字符串开始进行检索
    ‘a1b2c3d4’.search(/1/g); //1
    (2).match()方法将检索字符串，以找到一个或多个与RegExp匹配的文本
    RegExp是否具有标志g对结果影响很大
    非全局调用
    如果RegExp没有标志g，那么match（）方法就只能执行一次匹配
    如果没有找到任何匹配，则返回null
    否则返回一个数组，其中存放了了与它找到的匹配文本有关的信息
    返回数组的第一个元素存在的是匹配文本，而其他元素存放的是与正则表达式的子表达式匹配的文本
    处理常规的数组元素之外，返回的数组还含有两个对数属性
    index声明匹配文本的其实字符串在字符串的位置
    input声明对stringObject的引用
    全局调用
    如果RegExp具有标志g，则match方法执行全局检索，找到字符串的所有匹配子字符串
    没有找到匹配，返回null
    如果找到一个或多个匹配子串，则返回以数组
    数组元素中存放的是字符串所有的匹配子串，而且也没有index属性或则input属性
    (3).使用split方法把字符串分割为字符数组
    ‘a,b,c,d’.split(‘,’);//[‘a’,’b’,’c’,’d’]
    在一些复杂分割情况下我们可以使用正则表达式解决
    ‘a1b2c3d4’.split(/\d/);//[‘a’,’b’,’c’,’d’]
    (4).String.prototype.replace(str, replaceStr)
    str 被替换目标（字符串）
    replaceStr 替换的字符串
    String.prototype.replace(reg, replaceStr)
    str 被替换目标（正则表达式）
    replaceStr 替换的字符串
    String.prototype.replace(reg, function)
    function有四个参数
    匹配字符串
    正则表达式分组ner，没有分组则没有该参数
    匹配项在字符串中的index
    原字符串



## 页面加载和渲染过程

### 加载过程

1. 一般来说，页面资源分为3类：

   html代码、

   媒体文件，如图片、视频等、
   javascript、css

2. 页面加载基础知识解释：
   (1)**.DNS解析，把域名解析成IP地址**

   DNS英文全称:Domain Name Service 中文全称:域名解析服务: 域名–>IP地址
   为什么要域名？仅仅是因为 IP 不好记吗？
   不是的，同一个域名对应的IP在不同的区域是不一样的，因为大型网站做分区域的IP均衡代理。你在北京访问百度和你在深圳访问百度的域名是一样的都是baidu.com，但是DNS解析出来的IP是不一样的，如果你在北京直接访问深圳的IP就会慢很多
   在浏览器访问网站的时候，实际还是访问 IP，域名解析服务（DNS）会根据地域去解析成不同的 IP 让你的网站访问的更快
   综上所述，使用域名不仅仅是因为容易记住，而且是网页访问更快，因为DNS会解析出来的 IP 距离你很近。
   (2).**浏览器根据 IP 地址向服务器发起http请求**
   浏览器只是发起方，真正的核心模块还是操作系统，操作系统里有一些可以发起网络请求的服务，调用操作系统的服务，然后操作系统去发起http请求。这里面还有建立连接的三次握手过程.
   (3).**服务器处理http请求，并返回给浏览器**
   至于服务器怎么处理http请求这里不讲，返回的页面资源就可能有html代码，媒体文件，如图片、视频等，javascript、css等等。
   
   ### 输入一个网址，直到网址打开并加载结束，这个过程浏览器发生了什么
   
   - 查看浏览器中是否有缓存，有的话直接访问缓存
   - 如果缓存过期或者没有缓存，重新请求
   - 在发送http请求前，需要域名解析，获取相应的IP地址
   - 浏览器想服务器发起tcp链接，与浏览器建立tcp三次握手
   - 握手成功后，浏览器向服务器发送http请求，请求数据包
   - 服务器处理收到的请求，将数据返回至浏览器
   - 浏览器收到HTTP响应
   - 读取页面内容，浏览器渲染，解析html源码

### 渲染过程

3. 渲染过程

   根据HTML代码生成DOM Tree
   根据CSS代码生成CSSOM
   将DOM Tree和CSSOM整合形成Render Tree
   浏览器根据Render Tree渲染页面
   遇到script标签则暂停渲染，优先加载并执行JS代码，完成再继续
   直至把Render Tree渲染完成

4. 请求的是页面返回html代码
   (1).根据HTML代码生成DOM Tree（文本代码生成树结构）
   (2).根据CSS生成CSSOM
   (3).将DOM Tree和CSSOM整合形成Render Tree（渲染树）
   只根据DOM Tree是无法渲染的，其标签的CSS属性是在CSSOM Tree里面的，可以将Render Tree理解为挂着CSS属性的DOM Tree
   **渲染过程（二）**
   **(1).根据Render Tree渲染页面**
   **(2).遇到script则暂停渲染，优先加载并执行JS代码，完成再继续**
   **(3).直到把Render Tree渲染完成**

5. **JS操作和渲染页面操作是共用一个线程的，因为JS可能改变DOM结构而改变Render Tree的结构，所以遇到script就暂停渲染，否则渲染了可能没用，Render Tree变了(js要放最后)**

6. **Reflow（回流）：浏览器要花时间去渲染，当它发现了某个部分发生了变化影响了布局，那就需要倒回去重新渲染。**
   **Repaint（重绘）：如果只是改变了某个元素的背景颜色，文字颜色等，不影响元素周围或内部布局的属性，将只会引起浏览器的repaint，重画某一部分。**
   **Reflow要比Repaint更花费时间，也就更影响性能。所以在写代码的时候，要尽量避免过多的Reflow**

7. **window.onload和DOMContentLoaded**

   ```
   window.addEventListener('load',function(){}) //页面的全部资源加载完才会执行，包括图片、视频等
   document.addEventListener('DOMContentLoaded',function(){})//DOM 渲染完成即可执行，此时图片的视频可能还没有加载完
   ```

   如果在渲染的时候遇到img标签链接的图片还没下载完也会继续渲染，会把这个位置空出来，等到图片下载完成插入就行，图片的大小未指定可能引起reflow，比如图片过大把旁边内容撑下去了，所以最好图片设置宽和高来避免重新渲染

## 性能优化

![image-20200521132056704](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200521132056704.png)

1. **让加载更快**
   **(1)减少资源体积：压缩代码**
   **(2)减少访问次数：合并代码(几个js合并成一个js)，SSR服务器端渲染，缓存**
   **(3)使用更快网络:CDN**

2. **让渲染更快**
   **(1)CSS放在head、JS放在body最下面**
   **(2)尽早开始执行JS，用DOMContentLoaded触发**
   **(3)懒加载（图片懒加载和上划加载更多）**
   **(4)对DOM查询进行缓存  ** 

   ```
   for(var i=0;i<document.get...id('**');i++) // 不能频繁操作Dom
   
   var a=document.get...id('**').length
   for(var i=0;i<length;i++) // 缓存
   ```

   **(5)频繁DOM操作，合并到一起插入DOM结构**

   ```
   const list =document.getElementById('list')
   //创建一个文档片段，此时还没有插入到dom树种
   const frag=document.createDocumentFragment()
   //先插入文档片段中
    for(let i=0;i<20;i++){
       const li = document.createElement('li')
       li.innerHTML=`list item ${i}`
       frag.appendChild(li)
   }
   //都完成之后，再统一插入到Dom结构中
    list.appendChild(frag)
   ```

   **(6)节流throttle和防抖debounce**

   ![image-20200521173806407](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200521173806407.png)

   ### 手写防抖debounce
   
   ```
   const input1 = document.getElementById('input1')
   // let timer = null
   // input1.addEventListener('keyup', function () {
   //     if (timer) {
   //         clearTimeout(timer)
   //     }
   //     timer = setTimeout(() => {
   //         // 模拟触发 change 事件
   //         console.log(input1.value)
   
   //         // 清空定时器
   //         timer = null
   //     }, 500)
   // })
   
   // 防抖
   function debounce(fn, delay = 500) {
       // timer 是闭包中的
       let timer = null
   
       return function () {
           if (timer) {
               clearTimeout(timer)
           }
           timer = setTimeout(() => {
               fn.apply(this, arguments)  //fn()也行
               timer = null
           }, delay)
       }
   }
   
   input1.addEventListener('keyup', debounce(function (e) {
       console.log(e.target)
       console.log(input1.value)
   }, 600))
   ```
   
   ### 手写节流throttle  drag拖拽
   
   ```
   const div1 = document.getElementById('div1')
   
   // let timer = null
   // div1.addEventListener('drag', function (e) {
   //     if (timer) {
   //         return
   //     }
   //     timer = setTimeout(() => {
   //         console.log(e.offsetX, e.offsetY)
   
   //         timer = null
   //     }, 100)
   // })
     <div id="div1" draggable="true">可拖拽</div>
   // 节流
   function throttle(fn, delay = 100) {
       let timer = null
   
       return function () {
           if (timer) {
               return
           }
           timer = setTimeout(() => {
               fn.apply(this, arguments)
               timer = null
           }, delay)
       }
   }
   
   div1.addEventListener('drag', throttle(function (e) {
       console.log(e.offsetX, e.offsetY)
   }))
   
   div1.addEventListener('drag', function(event) {
   
   })
   
   ```
   
   **防抖**
   
   > 触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间
   >
   > 当你持续触发事件后，一定时间内没有再触发事件，事件处理函数才会处理一次
   >
   > 场景：input 搜索框
   
   - 思路：
   
   > 每次触发事件时都取消之前的延时调用方法
   
   ```js
   function debounce(fn) {
         let timeout = null; // 创建一个标记用来存放定时器的返回值
         return function () {
           clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
           timeout = setTimeout(() => { // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
             fn.apply(this, arguments);
           }, 500);
         };
       }
       function sayHi() {
         console.log('防抖成功');
       }
   
       var inp = document.getElementById('inp');
       inp.addEventListener('input', debounce(sayHi)); // 防抖
   ```
   
   **节流**
   
   > 高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率
   >
   > 点点点，每隔一秒执行一次
   >
   > 场景：resize（改变窗口大小） scroll
   
   - 思路：
   
   > 每次触发事件时都判断当前是否有等待执行的延时函数
   
   ```text
   function throttle(fn) {
         let canRun = true; // 通过闭包保存一个标记
         return function () {
           if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
           canRun = false; // 立即设置为false
           setTimeout(() => { // 将外部传入的函数的执行放在setTimeout中
             fn.apply(this, arguments);
             // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是false，在开头被return掉
             canRun = true;
           }, 500);
         };
       }
       function sayHi(e) {
         console.log(e.target.innerWidth, e.target.innerHeight);
       }
       window.addEventListener('resize', throttle(sayHi));
   ```
   
3. **https不是一个新的协议，它只是http协议身披一层SSL(Secure Socket Layer，安全套阶 层)协议,HTTPS比HTTP更加安全，对搜索引擎更友好，利于SEO,谷歌、百度优先索引HTTPS网页;**
   **HTTPS需要用到SSL证书，而HTTP不用;**

4. **SSR是 Server-Side Rendering(服务器端渲染)的缩写，将网页和数据一起加载一起渲染**

   ![image-20200521133518115](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200521133518115.png)

5. keyup：当用户释放键盘上的按键时触发

6. onchange 属性可以使用于： input, select, 和 textarea

7. clearInterval和 clearTimeout

8. 常见的web前端攻击方式有什么？
   (1)Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种代码注入攻击。攻击者通过在目标网站上注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可获取用户的敏感信息如 Cookie、SessionID 等，进而危害数据安全。

   ![image-20200523134200027](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523134200027.png)

   ![image-20200523134350743](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523134350743.png)

   npmjs.com/package/xss 用插件

   (2)XSRF攻击

   ![image-20200523134555117](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523134555117.png)

   ![image-20200523134617582](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523134617582.png)

   img支持跨域，网站已经登陆了，直接帮你购买

   ![image-20200523134759692](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523134759692.png)

9. 常用防范方法
   httpOnly: 在 cookie 中设置 HttpOnly 属性后，js脚本将无法读取到 cookie 信息。
   输入过滤: 一般是用于对于输入格式的检查，例如：邮箱，电话号码，用户名，密码……等，按照规定的格式输入。不仅仅是前端负责，后端也要做相同的过滤检查。因为攻击者完全可以绕过正常的输入流程，直接利用相关接口向服务器发送设置。
   转义 HTML: 如果拼接 HTML 是必要的，就需要对于引号，尖括号，斜杠进行转义,但这还不是很完善.想对 HTML 模板各处插入点进行充分的转义,就需要采用合适的转义库.(可以看下这个库,还是中文的)
   (2)跨站请求伪造（英语：Cross-site request forgery），也被称为 one-click attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已登录的 Web 应用程序上执行非本意的操作的攻击方法。如:攻击者诱导受害者进入第三方网站，在第三方网站中，向被攻击网站发送跨站请求。利用受害者在被攻击网站已经获取的注册凭证，绕过后台的用户验证，达到冒充用户对被攻击的网站执行某项操作的目的。
   “cookie本身就保存在浏览器端 到了时间才失效 你不用其他或者手动去清除 没到时间cookie不会失效 只有session 才会失效”

   



## 面试

![image-20200523142052639](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523142052639.png)

![image-20200523142111543](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523142111543.png)

如：小程序方面不是很熟

## 题目讲解

### BFC(Block formatting context)

块级格式化上下文，它是一个独立的渲染区域，只有Block-level box参与，它规定了内部的Block-level box如何布局，并且与这个区域外部毫不相干

#### BFC布局规则

1.内部的Box会在垂直方向上一个接一个放置

2.Box垂直方向上的距离由margin决定，同属于同一个BFC的两个相邻的Box的margin会发生重叠（兄弟元素margin重叠问题）

3.每个Box的左外边缘（margin-left)， 与包含块的左边（contain box left）相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。除非这个元素自己形成了一个新的BFC。

4.BFC区域不会与float-box重叠（两列布局）

5.计算BFC高度时，浮动元素也参与计算

6.BFC是页面上的一个隔离的独立容器，容器里的元素不会影响到外部元素，反之亦然

#### BFC开启方式

1.根元素

2.float不为none

3.position为absolute或fixed

4.overflow不为visible

5.display为inline-block, table-ceil, table-caption, flex, inline-flex

#### 三列布局

三列布局的要求

1.中间优先加载

2.两边固定，中间自适应

#### 圣杯布局

左右两列固定200px中间列自适应

内容区三列，HTML结构先写中间列然后左列右列

中间列width: 100%;

三列全部浮动，内容区清除浮动

左列margin-left: -100%; 将左列提升到上一行最左侧

右列margin-left: -200px; 将右列提升到上一行最右侧

内容区padding: 0 200px; 为左右两列留出空间

左列position: relative; left: -200px;

右列position: relative; left: 200px;

#### 双飞翼布局

左右两列固定200px中间列自适应

内容区三列，HTML结构先写中间列然后左列右列

中间列用warp包裹

wrap的width: 100%

三列全部浮动，内容区清除浮动

左列margin-left: -100%; 将左列提升到上一行最左侧

右列margin-left: -200px; 将右列提升到上一行最右侧

中间wrap padding: 0 200px; 为左右两列留出空间

与圣杯布局的区别是不使用定位

#### flex布局

左右两列固定200px中间列自适应

内容区三列，HTML结构先写中间列然后左列右列

内容区display: flex;

左右两列width: 200px; 或flex-basis: 200px;

中间列flex-grow: 1; 或flex: 1;设置拉伸因子为1 或者width: 100%

三列设置order 左中右排列

#### Grid布局

左右两列固定200px中间列自适应

内容区三列，HTML结构先写中间列然后左列右列

内容区

display: grid;

grid-template-columns: 200px auto 200px;

grid-template-areas: ‘left center right’;

中间列

grid-area: center;

###  元素垂直水平居中方案

#### 不使用定位

##### flex

容器：

display: flex;

justify-content: center;

align-items: center

##### Grid

容器：

display: grid;

justify-content: center;

align-items: center

#### 使用定位

容器：相对定位

元素绝对定位

top left: 50%

transform: translate3D(-50%, -50%, 0)

或者利用绝对定位盒子的特性

水平方向上：left + right + width + padding + margin = 包含块的content-box

垂直方向上：top + bottom + height + padding + margin = 包含块的content-box

容器：相对定位

元素绝对定位

top left right bottom:0

margin: auto

前提：已知元素高宽

### CSS隐藏元素

#### display:none;

不占据空间，无法响应自身事件

#### position: absolute;+z-index : -9999;

不占据空间，无法响应自身事件

#### overflow:hidden;

隐藏元素溢出部分，依然占据空间，但无法响应自身事件

#### visibility:hidden;

依然占据空间，无法响应自身事件

#### transform:scale(0,0);

依然占据空间，无法响应自身事件

#### opacity:0;

透明度为0，依然占据空间，可响应自身事件

### px em rem vw vh vmin vmax

px：相对于显示器屏幕分辨率

em：是相对长度，相对于自身的font-size，chrome默认16px

rem：是相对长度，相对于根标签的font-size

vw：相对视口宽度的1%

vh：相对视口高度的1%

vmin：相对视口高宽中较小值的1%

vmax：相对视口高宽中较大值的1%

### 响应式布局方案

#### 媒体查询

例如：

```
/* iphone6 7 8 plus */
@media screen and (max-width: 414px) {
    body {
        background-color: blue;
    }
}
```

#### 百分比布局

#### rem布局

rem布局原理：不改变CSS像素大小，改变不同设备上所占CSS像素的个数

#### viewport布局

viewport布局原理，改变CSS像素和物理像素的比例，不改变元素所占CSS像素的个数

#### vw/vh布局

#### flex布局

#### grid布局

### 常见的语法糖

指计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用

https://www.jianshu.com/p/bc2e172cb322



### var和let const的区别

```
1.var声明变量可以重复声明，而let不可以重复声明
2.var是不受限于块级的，而let是受限于块级
3.var会与window相映射（会挂一个属性），而let不与window相映射
4.var可以在声明的上面访问变量，而let有暂存死区，在声明的上面访问变量会报错
5.const声明之后必须赋值，否则会报错
6.const定义不可变的量，改变了就会报错
7.const和let一样不会与window相映射、支持块级作用域、在声明的上面访问变量会报错
```



### typeof能判断哪些类型

![image-20200523142705419](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523142705419.png)

### 列举强制类型转换和隐式类型转换





![image-20200523142747953](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523142747953.png)

### 手写深度比较isEqual

![image-20200523171130044](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523171130044.png)

```
// 判断是否是对象或数组
function isObject(obj) {
    return typeof obj === 'object' && obj !== null
}
// 全相等（深度）
function isEqual(obj1, obj2) {
    if (!isObject(obj1) || !isObject(obj2)) {
        // 值类型（注意，参与 equal 的一般不会是函数）
        return obj1 === obj2
    }
    // 如果传入的都是同个对象
    if (obj1 === obj2) {
        return true
    }
    // 两个都是对象或数组，而且不相等
    // 1. 先取出 obj1 和 obj2 的 keys ，比较个数
    const obj1Keys = Object.keys(obj1)
    const obj2Keys = Object.keys(obj2)
    if (obj1Keys.length !== obj2Keys.length) {
        return false
    }
    // 2. 以 obj1 为基准，和 obj2 一次递归比较
    for (let key in obj1) {
        // 比较当前 key 的 val —— 递归！！！
        const res = isEqual(obj1[key], obj2[key])
        if (!res) {
            return false
        }
    }
    // 3. 全相等
    return true
}

// 测试
const obj1 = {
    a: 100,
    b: {
        x: 100,
        y: 200
    }
}
const obj2 = {
    a: 100,
    b: {
        x: 100,
        y: 200
    }
}
// console.log( obj1 === obj2 )
console.log( isEqual(obj1, obj2) )

const arr1 = [1, 2, 3]
const arr2 = [1, 2, 3, 4]
```

![image-20200606152107125](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200606152107125.png)



### split()和join()区别

![image-20200523175125425](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523175125425.png)

```
// const arr = [10, 20, 30, 40]

// // pop
// const popRes = arr.pop()
// console.log(popRes, arr)  //40,[10,20,30]

// // shift
// const shiftRes = arr.shift()
// console.log(shiftRes, arr) // 10,[20,30,40]

// // push
// const pushRes = arr.push(50) // 返回 length
// console.log(pushRes, arr) // 5,[10,20,30,40,50]

// // unshift
// const unshiftRes = arr.unshift(5)  // 返回 length
// console.log(unshiftRes, arr) //5,[5,10,20,30,40]



// // 纯函数：1. 不改变源数组（没有副作用）；2. 返回一个数组
// const arr = [10, 20, 30, 40]

// // concat
// const arr1 = arr.concat([50, 60, 70])  // arr[10, 20, 30, 40,50,60,70]
// // map
// const arr2 = arr.map(num => num * 10)
// // filter
// const arr3 = arr.filter(num => num > 25)
// // slice
// const arr4 = arr.slice()

// // 非纯函数  不会返回函数
// // push pop shift unshift
// // forEach
// // some every
// // reduce

// const arr = [10, 20, 30, 40, 50]

// // slice 纯函数  ---切片 不会改变原函数
// const arr1 = arr.slice()
// const arr2 = arr.slice(1, 4)  //20，30，40
// const arr3 = arr.slice(2)	//30，40，50
// const arr4 = arr.slice(-2)  //40，50

// // splice 非纯函数 ---剪接  改变原数组
// const spliceRes = arr.splice(1, 2, 'a', 'b', 'c') //10,a,b,c,40,50  返回20,30
// // const spliceRes1 = arr.splice(1, 2) // 返回20，30  
// // const spliceRes2 = arr.splice(1, 0, 'a', 'b', 'c')
// console.log(spliceRes, arr)

网红题
const res = [10, 20, 30].map(parseInt)
console.log(res)

// 拆解
[10, 20, 30].map((num, index) => {
    return parseInt(num, index)  // [10，NAN,NAN]
})

```

### ajax请求get和post的区别

![image-20200524002654931](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524002654931.png)

### 闭包是什么？什么特性？什么影响？

![image-20200524004648434](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524004648434.png)

![image-20200524004701671](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524004701671.png)

```javascript
// // 自由变量示例 —— 内存会被释放
// let a = 0
// function fn1() {
//     let a1 = 100

//     function fn2() {
//         let a2 = 200

//         function fn3() {
//             let a3 = 300
//             return a + a1 + a2 + a3
//         }
//         fn3()
//     }
//     fn2()
// }
// fn1()

// // 闭包 函数作为返回值 —— 内存不会被释放
// function create() {
//     let a = 100  //返回函数用到a，变量a不会被释放
//     return function () {
//         console.log(a)
//     }
// }
// let fn = create()
// let a = 200
// fn() // 100

function print(fn) {
    let a = 200
    fn()
}
let a = 100   //同理不会被释放
function fn() {
    console.log(a)
}
print(fn) // 100
```



### 阻止事件冒泡和默认行为

![image-20200524005021290](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524005021290.png)



### 如何减少Dom操作

![image-20200524005214794](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524005214794.png)

### 查找，添加，删除，移动Dom节点

### 解释jsonp原理，为什么不是真正的ajax

1. jsonp的原理:就是利用浏览器可以动态地插入一段js并执行的特点完成的。

2.为什么不是真正的 ajax?   

  ajax的核心是 ： 通过XmlHttpRequest获取非本页内容，

  jsonp的核心 ： 动态添加<script>标签来调用服务器提供的js脚本。

3..ajax和jsonp的调用方式很像，目的一样，都是请求url，然后把服务器返回的数据进行处理，因此jquery和ext等框架都把jsonp作为ajax的一种形式进行了封装；

还是有不同点的：

\4. 实质不同
　ajax的核心是通过xmlHttpRequest获取非本页内容
　jsonp的核心是动态添加script标签调用服务器提供的js脚本

\6. ajax通过服务端代理一样跨域
　jsonp也不并不排斥同域的数据的获取

7 .jsonp是一种方式或者说非强制性的协议
　ajax也不一定非要用json格式来传递数据　

\8. .jsonp只支持get请求，ajax支持get和post请求

![image-20200524005741214](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524005741214.png)

![image-20200524005804294](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524005804294.png)

### ==和===区别

![image-20200524005714455](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524005714455.png)



### 函数声明和函数表达式区别

![image-20200524005941493](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524005941493.png)

```
// // 函数声明  函数提升
// const res = sum(10, 20)
// console.log(res)
// function sum(x, y) {
//     return x + y   //30
// }   

// // 函数表达式
// var res = sum(10, 20)
// console.log(res)
// var sum = function (x, y) {
//     return x + y   // 报错，sum不是一个函数 
// }

```

### new Object() 和 Object.create()区别

```
const obj1 = {
    a: 10,
    b: 20,
    sum() {
        return this.a + this.b
    }
}   
	obj1!==obj2
	
const obj2 = new Object({
    a: 10,
    b: 20,
    sum() {
        return this.a + this.b
    }
})

const obj21 = new Object(obj1) // obj1 === obj21

const obj3 = Object.create(null) // {}没有原型
const obj4 = new Object() // {} 有原型 object

const obj5 = Object.create({
    a: 10,
    b: 20,
    sum() {
        return this.a + this.b
    }
}) // {} 空对象，但是原型上有属性

const obj6 = Object.create(obj1)
//obj6 返回{},__proto__上有obj1， obj6.__proto__===obj1  但obj6!==obj1
//object.create 创建一个空对象，然后空对象的原型指向传入的对象
```

### 正则表达式

```
// ^可有可无，加了开头必须是   \w 字母数字下划线
// 邮政编码
/\d{6}/  // \d数字

// 小写英文字母
/^[a-z]+$/  //+一次或多次

// 英文字母
/^[a-zA-Z]+$/

// 日期格式 2019.12.1
/^\d{4}-\d{1,2}-\d{1,2}$/

// 用户名
/^[a-zA-Z]\w{5, 17}$/ //6到18位

// 简单的 IP 地址匹配
/\d+\.\d+\.\d+\.\d+/

```

![image-20200525003355560](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525003355560.png)

100，10，10



### 手写字符串trim保证浏览器兼容性

![image-20200525003721877](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525003721877.png)

\s 空白的字符

### 获取多个数值中最大值

![image-20200525004004301](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525004004301.png)

可以直接![image-20200525004017881](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525004017881.png)

### 如何实用js实现继承



![image-20200525004118294](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525004118294.png)









### 如何捕获js中的异常

![image-20200525004330948](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525004330948.png)







### 什么是JSON

![image-20200525004455081](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525004455081.png)

![image-20200525004547330](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525004547330.png)





### 获取当前页面url参数

![image-20200525004640738](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525004640738.png)

```
// // 传统方式
// function query(name) {
//     const search = location.search.substr(1) // 类似 array.slice(1) 1个开始截取 去掉前面？
//     // search: 'a=10&b=20&c=30' 
//		(^|&) 开头或者&开头  	([^&]*) 不包括& 0个或多个开始取  	(&|$) &结尾  i 不区分大小写				
//     const reg = new RegExp(`(^|&)${name}=([^&]*)(&|$)`, 'i')
//     const res = search.match(reg)
//     if (res === null) {
//         return null
//     }
//     return res[2]
// }
// query('d')

// URLSearchParams  要考虑兼容性问题

function query(name) {
    const search = location.search
    const p = new URLSearchParams(search)
    return p.get(name)
}
console.log( query('b') )

```

### 讲url参数解析为js对象

![image-20200525123515437](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525123515437.png)

![image-20200525123528383](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525123528383.png)

### 手写flatern考虑多层级  摊平数组

![image-20200525124311387](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525124311387.png)

```
function flat(arr) {
    // 验证 arr 中，还有没有深层数组 [1, 2, [3, 4]]
    const isDeep = arr.some(item => item instanceof Array)
    if (!isDeep) {
        return arr // 已经是 flatern [1, 2, 3, 4]   //没有深层数组的话就返回
    }
	
    const res = Array.prototype.concat.apply([], arr)
    return flat(res) // 递归  有深层次，一直摊平，等到!isDeep 就返回
}

const res = flat( [1, 2, [3, 4, [10, 20, [100, 200]]], 5] )
console.log(res)

```

![image-20200525125258371](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525125258371.png)

### 数组去重

```
// // 传统方式  速度慢，数据多不建议
// function unique(arr) {
//     const res = []
//     arr.forEach(item => {
//         if (res.indexOf(item) < 0) {
//             res.push(item)
//         }
//     })
//     return res
// }

// 使用 Set （无序，不能重复）  浏览器环境高就用
function unique(arr) {
    const set = new Set(arr)
    return [...set]
}

const res = unique([30, 10, 20, 30, 40, 10])
console.log(res)

```

### Object.assign不是深拷贝！ 深度的不能深拷贝，原地址变了还是会变

### 是否用过requestAnimationFrame

![image-20200525142317594](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525142317594.png)

```
// 3s 把宽度从 100px 变为 640px ，即增加 540px
// 60帧/s ，3s 180 帧 ，每次变化 3px

const $div1 = $('#div1')
let curWidth = 100
const maxWidth = 640

// // setTimeout
// function animate() {
//     curWidth = curWidth + 3
//     $div1.css('width', curWidth)
//     if (curWidth < maxWidth) {
//         setTimeout(animate, 16.7) // 自己控制时间
//     }
// }
// animate()

// RAF
function animate() {
    curWidth = curWidth + 3
    $div1.css('width', curWidth)
    if (curWidth < maxWidth) {
        window.requestAnimationFrame(animate) // 时间不用自己控制 
    }
}
animate()
```

### 如何性能优化，从几个方面考虑

![image-20200525142105758](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200525142105758.png)



### 进程和线程 区别

进程是资源分配的最小单位，线程是CPU调度的最小单位

### http长轮询和短轮询

#### http 长轮询：

http 长轮询是服务器收到请求后如果有数据, 立刻响应请求; 如果没有数据就会 hold 一段时间, 这段时间内如果有数据立刻响应请求; 如果时间到了还没有数据, 则响应 http 请求;浏览器受到 http 响应后立在发送一个同样 http 请求查询是否有数据;

http 长轮询的[局限](http://www.ibm.com/developerworks/cn/web/wa-lo-comet/#N1018B):

1. 浏览器端对统一服务器同时 http 连接有最大限制, 最好同一用户只存在一个长轮询;
2. 服务器端没有数据 hold 住连接时会造成浪费, 容易产生服务器瓶颈;

#### http 短轮询：

http端轮询是服务器收到请求不管是否有数据都直接响应 http 请求; 浏览器受到 http 响应隔一段时间在发送同样的 http 请求查询是否有数据;

http 短轮询的局限是实时性低;

两者相同点：
可以看出 http 长轮询和 http 短轮询的都会 hold 一段时间;

两者不同点
间隔发生在服务端还是浏览器端: http 长轮询在服务端会 hold 一段时间, http 短轮询在浏览器端 “hold” 一段时间;

### 一次完整的HTTP事务是怎么一个过程

1、域名解析； 2、发起TCP的三次握手； 3、建立TCP连接后发起http请求； 4、服务器端响应http请求，浏览器得到html码； 5、浏览器解析html代码，并请求html代码中的资源； 6、浏览器对页面进行渲染并呈现给客户。

# JavaScript相关

## 数组常用操作：

```
map: 遍历数组，返回回调返回值组成的新数组
forEach: 无法break，可以用try/catch中throw new Error来停止
filter: 过滤
some: 有一项返回true，则整体为true
every: 有一项返回false，则整体为false
join: 通过指定连接符生成字符串
push / pop: 末尾推入和弹出，改变原数组， 返回推入/弹出项【有误】
unshift / shift: 头部推入和弹出，改变原数组，返回操作项【有误】
sort(fn) / reverse: 排序与反转，改变原数组
concat: 连接数组，不影响原数组， 浅拷贝
slice(start, end): 返回截断后的新数组，不改变原数组
splice(start, number, value...): 返回删除元素组成的数组，value 为插入项，改变原数组
indexOf / lastIndexOf(value, fromIndex): 查找数组项，返回对应的下标
reduce / reduceRight(fn(prev, cur)， defaultPrev): 两两执行，prev 为上次化简函数的return值，cur 为当前值(从第二项开始)
```



## JS实现函数柯里化+偏函数组合应用

什么是柯里化？

多参数函数转换为一系列单参数函数的技术

什么是偏函数？

多函数参数，每次应用其一个或多个参数，但不是全部参数，在这个过程中创建一个新函数，这个函数用于接受剩余的参数。

```
    // js实现函数柯里化+偏函数
    function curryPartial(fn, args) {
      // 获取函数形参个数
      const length = fn.length
      // 获取实参数组
      args = args || []
      // 返回柯里化函数
      return function () {
        // 实参数组整合
        const argsArray = args.concat(Array.from(arguments))
        // 如果实参个数小于形参个数
        if (argsArray.length < length) {
          // 继续整合
          return curryPartial.call(this, fn, argsArray)
        } else {
          // 当实参整合完成调用函数
          return fn.apply(this, argsArray)
        }
      }
    }
    var fn = curryPartial(function (a, b, c) {
      console.log([a, b, c]);
    });

    fn("a", "b", "c") // ["a", "b", "c"]
    fn("a", "b")("c") // ["a", "b", "c"]
    fn("a")("b")("c") // ["a", "b", "c"]
    fn("a")("b", "c") // ["a", "b", "c"]
```

## JS中为什么 0.1+0.2 !== 0.3

JS中Number数据类型遵循IEEE 754标准

64位二进制从左往右依次为：

第63位：符号位，0表示正数，1表示负数(s)

第62位到第52位：储存指数部分（e）

第51位到第0位：储存小数部分（即有效数字）f

但JS的最大安全数 Number.MAX_SAFE_INTEGER = Math.pow(2,53)-1

因为二进制有效数字总是 1.xxxxxxx (xxxxxxx为尾数部分f，有效数字首位默认为1}

因此JS二进制有效数字长度为53（64位浮点的前52位+被省略的1位）

在运算 0.1+0.2 的过程中两处产生精度损失

1.进制转换

0.1转二进制0.0001100110011001100110011001100110011001100110011001101(无限循环)

0.2转二进制

0.001100110011001100110011001100110011001100110011001101(无限循环)

超过IEEE 754尾数位置限制将被截取

2.对阶运算

由于指数位数不相同，运算时需要对阶运算 这部分也可能产生精度损失

最终造成 0.1+0.2 !== 0.3 的结果

注：浮点数值的最高精度是 17 位小数，因此0.1.toPrecision(17) = 0.100000000000000000导致 0.1 还是 0.1

解决方法:

```
function add(num1, num2) {
    const num1Digits = (num1.toString().split('.')[1] || '').length;
    const num2Digits = (num2.toString().split('.')[1] || '').length;
    const baseNum = Math.pow(10, Math.max(num1Digits, num2Digits));
    return (num1 * baseNum + num2 * baseNum) / baseNum;
}
```

## 包装类

JS三个包装类：Number() String() Boolean()
作用：将基本数据转换为引用数据类型，

当我们对一些基本数据类型的值去调用属性和方法时，浏览器会临时使用包装类将其转换为对象，然后在调用对象的属性和方法

调用完以后，再将其转换为基本数据类型

## this指向

解析器在调用函数每次都会向函数内部传递进一个隐含的参数,这个隐含的参数就是this，this指向的是一个对象

这个对象我们称为函数执行的 上下文对象，根据函数的调用环境的不同，this会指向不同的对象

一句话：this指向调用它的对象

1.以函数的形式调用时，this指向window

2.以方法的形式调用时，this就是调用方法的那个对象

3.当以构造函数的形式调用时，this就是新创建的实例对象

4.使用call() apply() bind()调用时，this自定义

5.箭头函数自身没有this，箭头函数的this继承的是定义时外层最近的对象的this

6.严格模式下，函数this指向undefined

## call() apply() bind()的区别

call和apply改变了函数的this上下文后便执行该函数,而bind则是返回改变了上下文后的一个新函数。

call和apply之间的差别在于参数的区别，call和apply的第一个参数都是要改变上下文的对象，

而call从第二个参数开始以参数列表的形式展现，apply则是将参数放在一个数组或类数组里面作为它的第二个参数。

## new的执行过程

1.创建新的实例对象

2.给新建实例对象属性赋值，其__proto__隐式原型指向其构造函数的prototype显式原型

3.this上下文指向新建的实例对象

4.执行构造函数

5.构造函数有无return
无return（即return undefined）或return类型为基本类型，则最终return还是新建的实例对象
若return为一个引用类型，则最终return这个引用类型

## 事件流

事件流分为三个阶段：捕获阶段 目标阶段 冒泡阶段

### 捕获阶段

从最外层祖先元素，向目标元素进行事件的捕获，默认此时不会触发事件

（addEventListener第三个参数为false, attachEvent没有第三个参数因为IE8及以下没有捕获阶段）

### 目标阶段

事件捕获到目标元素，捕获结束开始在目标元素上触发事件

### 冒泡阶段

事件从目标元素向他的祖先元素传递，依次触发祖先元素上的事件

阻止冒泡：event.stopPropagation()

## JS引擎如何管理内存

### 内存的生命周期

1.分配内存，得到使用权

2.存储数据进行反复操作

3.释放内存空间

### 释放内存

栈内存使用一级缓存，堆内存使用二级缓存

1.局部变量：函数执行完自动释放

2.对象：称为垃圾对象（无变量引用 ） => 垃圾回收机制回收

## JavaScript垃圾回收机制

### 标记清除

当变量进入JS执行栈环境标记为 ‘进入环境’ ，标记为’进入环境’的变量不可被回收

> 从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到他们。

当变量从JS执行栈出栈，离开环境时，则标记为’离开环境’，离开环境的变量可以被回收

### 引用清除

堆对象被变量引用的次数为0时，回收其占用的内存空间

引用清除存在循环引用的问题

```
const A = {}
const B = {}
A.b = B
B.a = A
```

A, B二个对象的堆内存的引用次数均为2，会一直存在于内存中
解决：赋值为null，不再引用

IE9把DOM和BOM转换成真正的JS对象了，所以避免了这个问题。

#### 事件循环机制eventloop 和 JS垃圾回收机制

```
事件循环机制：
1.在代码的执行过程中会先执行同步代码，然后宏任务（script,setTimeout...）进入宏任务队列，微任务（Promise.then()，Promise）进入微任务队列
2.当宏任务执行完之后出队，检查微任务列表，继续执行微任务直到执行完毕
3.执行浏览器的UI渲染过程
4.检查是否有Web Worker任务，有则执行
5.继续下一轮的宏任务和微任务

垃圾回收机制：
方法一：标记清除（比较常见）
1.当变量进入执行环境时，就标记这个变量为“进入环境”。
//从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到他们。
2.当变量离开环境时，则将其标记为“离开环境”。
3.垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记，它会去掉环境中的变量以及被环境中的变量引用的标记。
4.在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。
5.最后垃圾收集器完成内存清除工作，销毁那些带标记的值，并回收他们所占用的内存空间。

方法二：引用计数（不常见）
1.跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型赋值给该变量时，则这个值的引用次数就是1。
2.相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数就减1。
3.当这个引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其所占的内存空间给收回来。
4.这样，垃圾收集器下次再运行时，它就会释放那些引用次数为0的值所占的内存。
```



## 原型与原型链

### 显式原型与隐式原型

每个函数对象都有 prototype，它默认指向一个 Object 实例对象，即 显式原型 prototype

每个实例对象都有__proto__，即隐式原型，**proto** 指向其构造函数的 prototype

### 原型链

原型链即隐式原型链

实例对象的隐式原型指向其构造函数的显式原型

注意点：

1.Object和Function都是函数，所有函数都是Function的实例，所以其隐式原型均指向Function.prototype

2.Object.prototype.**proto** = null

当访问一个实例对象的属性时

1.先在实例对象自身的属性中查找，找到返回，否则

2.沿着原型链向上查找，找到返回，否则

3.返回 undefined

## instanceof

A instanceof B

在同一全局环境下，A的隐式原型链是否经过B的显式原型

## 预解析

var 提前申明，但不赋值，执行到才赋值

function 提前声明且定义

## 执行上下文

### 全局执行上下文

1.在执行全局代码前将window确定为全局执行上下文对象

2.预解析
var 提前声明不赋值，添加为window的属性
function 提前声明且定义，添加为window的方法
this上下文指向window

3.压栈，执行全局代码

### 函数执行上下文

1.在调用函数准备执行函数体之前，创建对应的函数执行上下文对象

2.预解析
形参，arguments，this赋值，添加为函数执行上下文的属性
var 提前声明不赋值，添加为函数执行上下文的属性
function 提前声明且定义，添加为函数执行上下文的方法

3.压栈，执行函数代码

## 执行上下文栈

1.在全局代码执行前，JS引擎会创建执行栈来存储所有的执行上下文对象

2.在全局执行上下文确定后，将其压栈

3.在函数执行上下文创建后，将其压栈

4.在当前函数执行完后，将栈顶出栈，仍然在内存中，等待垃圾回收机制回收

5.所有代码执行完后，栈中只剩window

## 作用域

JavaScript采用的是词法作用域（静态作用域）

分类：全局作用域，函数作用域，ES6块级作用域

作用：隔离变量，不同作用域下同名变量不会有冲突

## 作用域链

通过词法环境的对外部词法环境的引用（scope）将作用域链起

变量可通过作用域链逐层向上查找

## 闭包

### 什么是闭包？

高程：有权访问另一函数作用域中的变量的函数
MDN：函数以及其对外部词法环境的引用捆绑称为闭包

### 闭包的作用

1.延长局部变量的生命周期

2.让函数外部可操作函数内部的数据

### 闭包可能导致的问题

内存泄漏 解决：即时释放引用的外部词法环境

## 进程与线程

### 进程

进程是资源（CPU、内存等）分配的基本单位，它是程序执行时的一个实例。程序运行时系统就会创建一个进程，并为它分配资源

### 线程

线程是进程内的一个独立执行单元，是程序执行的一个完整流程，也是CPU最小的调度单元

### 关系

应用程序必须运行在一个进程的某个线程上

一个进程至少有一个运行的线程，也就是主线程

一个进程多线程运行时，线程之间数据共享

多个进程之间的数据不能直接共享

## 浅拷贝与深拷贝

### 浅拷贝

拷贝指针，修改拷贝后的数据会影响原数据，使得原数据不安全

### 深拷贝

拷贝的时候生成新数据，修改拷贝后的数据不会影响原数据

### JSON.parse(JSON.stringify(result))

缺点：不能对undefined，函数，symbol进行深拷贝

### 递归实现深拷贝

1.判断数据类型

typeof 注意：typeof判断array,object,null均返回’object’

Object.prototype.toString.call(param).slice(8, -1)

Array.isArray()

A instanceof B

2.遍历递归

```
    function deepCopy(obj) {
      if (typeof obj !== 'object' || obj === null) {
        return obj
      }
      const result = obj instanceof Array ? [] : {}
      for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
          result[key] = deepCopy(obj[key])
        }
      }
      return result
    }
```

## 如何判断是否是数组

### instanceof

缺陷：instanceof假定只有一个全局环境，如果网页中包含多个框架，那实际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的Array构造函数。

如果你从一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别具有各自不同的构造函数。

### param.constructor === Array

缺陷：construcor属性可改写

### Object.prototype.toString.call(param).slice(8, -1)

### Array.isArray(param)

## 数组去重

```
      for (let i = 0; i < arr.length; i++) {
        if (arr.indexOf(arr[i]) !== i) {
          arr.splice(i, 1)
          i--
        }
      }
      return arr
Array.from(new Set(arr))
arr.filter((item, index) => arr.indexOf(item) === index)
arr.reduce((prev, cur) => prev.includes(cur) ? prev : [...prev, cur], [])
```

## 判断一个对象是否是空对象

```
String(obj) === '[object Object]' && Reflect.ownKeys(obj).length === 0
```

Reflect.ownKeys() 为 Object.getOwnPropertyNames() 与 Object.getOwnPropertySymbols() 二者 concat() 之后的数组

## 伪数组转数组

### `[...param]`(有限)

拥有可迭代属性接口，也就是 Symbol.iterator 的伪数组对象（如arguments, NodeList, HTMLCollection）才可用

对于自定义的伪数组，若没有添加 Symbol.iterator 则会报错

### Array.prototype.slice.call(param)

### Array.from(param) 接受伪数组和可迭代对象

## super关键字 与 super()

### super关键字

super是指向当前对象的原型的一个指针，实际上就是Object.getPrototypeOf(this)的值

super在函数简写写法内才有效，否则会报错

```
const person = {
    say() {
        return 'Hi'
    }
}
const friend = {
    say() {
        return super.say() + 'friend'
    }
}
Object.setPrototypeOf(friend, person)
```

### super()

派生类构造器中

super(params) 相当于 基类.call(this, params)

所以派生类构造器中的super()是用来初始化this

super()使用注意点

1.你只能在派生类中使用super()。若尝试在非派生的类（即：没有使用extends关键字的类）或函数中使用它，就会抛出错误。

2.在构造器中，你必须在访问this之前调用super()。由于super()负责初始化this，因此试图先访问this自然就会造成错误。

3.唯一能避免调用super()的办法，是从类构造器中返回一个对象。

## class关键字

class关键字是以ES5组合继承为基础的一个语法糖，但是它和组合继承又有区别

1.组合继承的自定义function构造函数存在变量提升，而类声明类似于let，没有变量提升，存在暂时性死区

2.类声明中的所有代码会自动运行在严格模式下，且无法退出严格模式

3.类的所有实例方法都是不可枚举的，而自定义构造函数只有通过Object.defineProperty()才能将方法改为不可枚举

4.类的所有实例方法内部都没有constructor，使用new调用类的实例方法会报错

5.调用类构造器时，不加new会报错

6.在类的实例方法内部尝试重写类名会报错

## Promise

### 什么是Promise

Promise是ES6新增的对于JS异步编程的一种新的解决方案，可以解决回调地狱问题

从语法上来说：Promise是一个构造函数

从功能上来说：Promise实例用来封装异步操作，并可获取其结果

### Promise的三种状态

Pending初始化状态，fulfilled成功状态，rejected失败状态

### Promise的API

Promise.resolve()：返回一个成功状态的promise

Promise.reject()：返回一个失败状态的promise

Promise.all()：多个 Promise 任务同时执行。如果全部成功执行，则以数组的方式返回所有 Promise 任务的执行结果。 如果有一个 Promise 任务 rejected，则只返回 rejected 任务的结果。

Promise.race()：多个 Promise 任务同时执行，返回最先执行结束的 Promise 任务的结果，不管这个 Promise 结果是成功还是失败

Promise.prototype.then()：注册回调

Promise.prototype.catch()：捕获异常

### 如何让Promise.all()在抛出异常后依然有效?

将传入 Promise.all 的数组进行遍历，如果 catch 住 reject 结果，直接返回，这样就可以在最后结果中将所有结果都获取到

ES11：Promise.settled()

## 简述箭头函数

1.箭头函数本身没有this，箭头函数的this继承的是外部函数的this

2.箭头函数没有prototype

3.箭头函数没有construct内部方法，不能用new调用

什么时候使用箭头函数？

非对象的方法且不用做构造函数时，使用箭头函数

## for in 和 for of 和 Object.keys()的区别

### for in

1.for in遍历对象以及原型链上的可枚举属性

2.如果用于遍历数组，除了遍历其元素外，还会遍历数组对象自定义的可枚举属性及其原型链上的可枚举属性

3.遍历对象返回的属性名和遍历数组返回的索引都是 string 类型

### for of

1.for of遍历可迭代对象（String, Array, Map, Set, arguments, HTMLCollection, Nodelist）

2.遍历数组后输出的结果为元素的值，而非索引

```
// 如果要遍历对象，可与 Object.keys 配合
var person = {
    name: 'June',
    age: 17,
    city: 'guangzhou'
}
for(var key of Object.keys(person)) {
    console.log(person[key]); // June, 17, guangzhou
}

// 配合 entries 输出数组索引和值/对象的键值
var arr = ['a', 'b', 'c'];
for(let [index, value] of Object.entries(arr)) {
    console.log(index, ':', value);
    // 0:a, 1:b, 2:c
}
var obj = {name: 'June', age: 17, city: 'guangzhou'};
for(let [key, value] of Object.entries(obj)) {
    console.log(key, ':', value);
    // name:June,age:17,city:guangzhou
}
```

### Object.keys()

1.返回对象自身可枚举属性组成的数组

2.不会遍历对象原型链上的属性以及 Symbol 属性

3.如果用于遍历数组，除了遍历其元素外，还会遍历数组对象自定义的可枚举属性

4.遍历对象返回的属性名和遍历数组返回的索引都是 string 类型

# Web相关

## 同源政策

同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。

同源：同协议，同域名，同接口

## Ajax

```
    // 手写Ajax
    var xhr = new XMLHttpRequest()
    xhr.open('post', 'http://localhost:3000')
    xhr.send(obj)
    xhr.onreadystatechange = () => {
      // Ajax状态码
      // 0：请求未初始化，未open
      // 1：请求初始化未发送，open未send
      // 2：请求已发送
      // 3：请求正在处理
      // 4：请求已经完成
      if (xhr.readyState === 4 && xhr.status === 200) {
        console.log(xhr.responseText)
      }
    }
```

## 跨域的解决方案

### 封装jsonp

它不属于Ajax请求，利用script标签可以向非同源地址发送请求

```
    // 手写jsonp
    function jsonp(configObj) {
      const fnName = configObj.callbackName
      window[fnName] = configObj.success
      const scriptNode = document.createElement('script')
      scriptNode.src = `${configObj.url}?callback=${fnName}`
      document.body.appendChild(scriptNode)
      scriptNode.onload = () => {
        document.body.removeChild(scriptNode)
        window[fnName] = null
      }
    }
```

### CORS跨域资源共享

#### 简单请求

只要同时满足以下两个条件就属于简单请求，简单请求会跳过预检请求

条件1：

Method为 GET, HEAD, POST 之一

条件2：

Content-Type为 text/plain, multipart/form-data, application/x-www-form-urlencoded之一

请求中的任意 XMLHttpRequestUpload 对象均没有注册任何事件监听器

#### 预检请求

```
const express = require('express')
const app = express()
const whiteList = ['http://localhost:3000'] //设置白名单
app.use(function(req, res, next) {
  const origin = req.headers.origin
  if (whiteList.includes(origin)) {
    // 设置哪个源可以访问我
    res.setHeader('Access-Control-Allow-Origin', origin)
    // 允许携带哪个头访问我
    res.setHeader('Access-Control-Allow-Headers', 'name')
    // 允许哪个方法访问我
    res.setHeader('Access-Control-Allow-Methods', 'PUT')
    // 允许携带cookie
    res.setHeader('Access-Control-Allow-Credentials', true)
    // 预检的存活时间
    res.setHeader('Access-Control-Max-Age', 6)
    // 允许返回的头
    res.setHeader('Access-Control-Expose-Headers', 'name')
    if (req.method === 'OPTIONS') {
      res.end() // OPTIONS请求不做任何处理
    }
  }
  next()
})
```

### postMessage()

postMessage是HTML5 XMLHttRequest Level 2中的 API, 并且为数不多跨域操作的window属性之一,它可用于解决以下方面的问题:

1.页面和其打开的新窗口的数据传递

2.多窗口之间消息传递

3.页面与嵌套的iframe消息传递

4.上面三个场景的跨域数据传递

postMessage()方法允许来自不同源的脚本采用异步方式进行有限的通信,可以实现跨文本档,多窗口,跨域消息传递

### node中间件代理

![img](http://localhost:2333/stackedit-lightapp1.0.6/_v_images/20200716181300173_30521.png)

### nginx反向代理

### Websocket

Websocket是HTML5的一个持久化的协议，它实现了浏览器与服务器的全双工通信，同时也是跨域的一种解决方案。WebSocket和HTTP都是应用层协议，都基于 TCP 协议。但是 WebSocket 是一种双向通信协议，在建立连接之后，WebSocket 的 server 与 client 都能主动向对方发送或接收数据。同时，WebSocket 在建立连接时需要借助 HTTP 协议，连接建立好了之后 client 与 server 之间的双向通信就与 HTTP 无关了。

## 浏览器内核(render进程)多线程

### 主线程

#### JS引擎线程与GUI渲染线程互斥

#### JS引擎线程（V8引擎中JS编译执行过程）

将要执行全局代码时，创建JS执行栈，创建全局执行上下文并进行压栈

当执行到函数时，创建函数执行上下文并进行压栈

V8引擎在拿到执行上下文后对其代码逐行做分词分析以及词法分析

分词分析就是将如 var a = 2; 拆分成 var, a, =, 2, ;,这样的原子符号

词法分析就是指：登记变量声明，函数声明（登记的地方为词法环境）

在分词结束后，V8会做代码解析，将分好的原子符号解析翻译成一个抽象语法树，在这一步时，若发现语法错误，则会报错不再往下执行

最后V8引擎生成CPU可执行的机器码

#### GUI渲染线程

负责渲染浏览器页面

渲染过程

1.解析HTML构建DOM树

2.解析CSS构建CSSOM

3.DOM树和CSSOM结合形成Render树

4.布局Render树，负责元素的尺寸和位置（回流）

5.绘制Render树（重绘）

6.将各层信息发送给GPU进程，GPU将各层合成，显示在页面上

CSS的加载会阻塞渲染

#### 重绘回流与硬件加速

元素的位置和尺寸的改变会引起回流和重绘，元素样式改变会引起重绘，影响浏览器性能

因为GPU是专门为处理图形而设计，所以它在速度和能耗上更有效率。

通过GPU硬件加速的方式（transform、opacity、filters），声明一个新的复合图层，它会单独分配资源，这样就不会影响默认复合图层(标准文档流以及position为absolute，fixed都属于默认复合层)，从而避免重绘与回流

注意:如果a是一个复合图层，而且b在a上面，那么b也会被隐式转为一个复合图层

在GPU渲染字体会导致抗锯齿无效。这是因为GPU和CPU的算法不同。因此如果你不在动画结束的时候关闭硬件加速，会产生字体模糊。

### Web API分线程

#### 定时器管理线程

#### DOM事件响应线程

#### 网络请求线程

## 浏览器数据本地存储方式

### Cookies

由于HTTP是无状态协议，Cookies的出现是为了保存HTTP状态

服务器发送请求set-cookie，浏览器保存Cookies之后同域名下的每次HTTP请求都会携带Cookies的信息

JS也可通过document.cookie读取和设置

Cookies的特点：

1.遵循同源政策

2.Cookies的大小限制在4KB

3.过多的Cookies会导致性能浪费，因为同一域名下的所有请求都会携带Cookies

4.一般由服务器生成，可设置过期时间

5.和服务器通信

### LocalStorage

H5新增的本地存储方案
一个可被用于访问当前源（ origin ）的本地存储空间的 Storage 对象。

```
localStorage.setItem('myCat', 'Tom');
let cat = localStorage.getItem('myCat');
localStorage.removeItem('myCat');
localStorage.clear();
```

LocalStorage的特点：

1.遵循同源政策

2.大小为5M左右

3.可以在Tab之间数据通信

4.保存的数据会一直存在内存，没有过期时间，除非主动清除

5.只用在客户端，不和服务器通信

### SessionStorage

H5新增的本地存储方案

与LocalStorage相比，只用于当前浏览器的一次会话，浏览器关闭，数据清空

```
sessionStorage.setItem('key', 'value');
let data = sessionStorage.getItem('key');
sessionStorage.removeItem('key');
sessionStorage.clear();
```

SessionStorage的特点：

1.遵循同源政策

2.大小为5M左右

3.每次Tab创建各自的SessionStorage

4.浏览器关闭，当前SessionStorage清空

5.只用在客户端，不和服务器通信

### IndexedDB

它是一个运行在浏览器上的非关系型数据库，它的大小是没有存储上限的。

IndexedDB的特点：

1.遵循同源政策

2.存储空间大，没有上限

3.异步操作，不会造成浏览器卡死

4.保存的数据会一直存在内存，没有过期时间，除非主动清除

5.只用在客户端，不和服务器通信

## HTML5 manifest 离线缓存

### 原理

第一次访问时，浏览器会根据manifest文件上的离线存储资源清单进行本地缓存，

之后的每次在线访问，浏览器会比较manifest文件是否修改

如果未修改，则浏览器直接调用离线缓存无需再发送请求

若修改，则浏览器重新下载更新离线存储资源

若是离线状态下访问，浏览器直接使用离线存储的资源

这使得用户在离线状态下也可访问站点的部分资源

### 使用

<meta manifest = ‘demo.appcache’>这个属性指向一个manifest的文件，这个文件指明了当前页面哪些资源需要进行离线缓存

manifest配置文件分为三部分

1.CACHE MANIFEST

指出需要进行manifest的文件，声明的文件都将被下载缓存下来

2.NETWORK

默认为*，表示除了CACHE外的其它所有资源都需要联网请求

3.FALLBACK

表示替代资源，这些资源加载不到就替代加载哪些资源

### 离线缓存更新问题及解决方案

只有当manifest文件改变时，浏览器才会重新下载离线存储资源

但是在重新下载离线存储资源，页面加载已经开始了，若缓存更新尚未完成，浏览器还是会使用旧的缓存的资源。

解决方案：

添加事件监听，当监听到本地缓存更新后，进行重载页面，以达到更新的目的。

```
applicationCache.addEventListener("updateready", function(){
    applicationCache.swapCache();      // 手动更新本地缓存
    location.reload();    //重新加载页面页面
},true);
```

## 强制缓存与协商缓存

浏览器缓存主要分为 memory cache 内存缓存 和 disk cache 磁盘缓存

memory cach： 读取高效，但缓存持续性短，会随着进程的释放而释放，一旦关闭tab页面，内存中的缓存就被释放了

disk cache：读取比 memory cach 慢，但缓存保存在磁盘中。在所有浏览器缓存中，disk cache 覆盖面最大。

### 强制缓存

强制缓存不会向服务器发送请求，直接从缓存中读取资源

#### Expires (HTTP/1.0)

设置缓存文件过期时间，当缓存文件过期需要向服务器再次请求

`Expires: Wed, 25 Sep 2019 08:13:53 GMT`

Expires 是HTTP/1.0的产物，受制于本地时间，如果修改了本地时间可能会造成缓存失效

#### Cache-Control (HTTP/1.1)

Cache-Control是HTTP/1.1的产物，若 Expires 和 Cache-Control 同时存在，Cache-Control 的优先级更高

`Cache-Control: max-age=60`
max-age表示缓存文件过期时间，过期需要重新请求

`Cache-Control: s-maxage=60`
s-maxage只在proxy代理缓存中有效，优先级高于 Expires 和 Cache-Control: max-age

`Cache-Control: public`
public表示该资源为公共资源，可以被缓存在任何可以被缓存的地方（CDN，中间代理服务器，浏览器等等），可多用户共享

`Cache-Control: private`
private表示该资源为私有资源，只可以被浏览器缓存

`Cache-Control: no-store`
no-store表示浏览器每次都从服务器拿资源

`Cache-Control: no-cache`
no-cache表示并不是不缓存，而是客户端缓存内容，但用不用由协商缓存决定

`Cache-Control: max-stale=60`
max-stale表示容忍最大过期时间

`Cache-Control: min-fresh=60`
min-fresh表示容忍的最小新鲜度，如60，表示希望60s内获得新的响应

### 协商缓存

协商缓存是在强制缓存失效后，浏览器携带缓存标识向服务器发送请求，由服务器根据缓存标识确定是否使用缓存的过程

主要有两种结果：
1.协商缓存生效，返回304，使用缓存

2.协商缓存失败，返回200和请求数据，不使用缓存

#### Last-Modified/If-Modified-Since (HTTP/1.0)

当浏览器第一次请求资源时，服务器返回资源的同时，在 Response Header 中会有一个 Last-Modified 的 Header，Header值是这个资源在服务器上最后修改的时间

当浏览器下次请求该资源时，在 Requset Header 中会有一个 If-Modified-Since 的 Header，Header值为上次响应的 Last-Modified 的值

服务器再次收到对该资源的请求，会将 If-Modified-Since 的 Header值与该资源最后修改时间进行对比，若无变化，则响应304和Not Modified，直接从缓存中读取，若有变化，则响应200并返回新的资源

但是Last-Modified存在两个弊端

1.若在本地打开缓存文件，即使没有对缓存文件进行修改，也还是会造成 Last-Modified 的值被修改，服务器不能命中缓存，导致发送相同的资源

2.因为 Last-Modified 只能以秒计时，若在1s内做出对文件的修改，那么服务器会认为缓存命中，不会返回新的资源

既然根据修改时间来决定是否缓存尚有不足，那么可以使用HTTP/1.1的 Etag/If-None-Match 根据文件内容来判断

#### Etag/If-None-Match (HTTP/1.1)

Etag是服务器响应请求时，返回当前资源文件的一个唯一标识，由服务器生成，只要资源有变化，Etag就会重新生成

浏览器在下一次向服务器请求资源时，会将服务器上次返回的 Etag 值放到 Request Header 的 If-None-Match 里

服务器只需要比较 Etag 值是否一致，就能很好地判断资源相对于客户端是不是已经更新过了

如果 Etag 值不匹配，则以常规GET 200回包形式，将新的资源和新的 Etag 响应给客户端

如果 Etag 值匹配，则响应304，客户端直接使用本地缓存

#### 两种方法对比

1.首先在精度和优先级上 Etag 高于 Last-Modified

2.性能上 Etag 逊于 Last-Modified，毕竟一个 Last-Modified 只需要记录时间，而 Etag 要根据算法算出 Hash

### 缓存机制

强制缓存优先于协商缓存进行，若强制缓存失效，则进行协商缓存，协商缓存由服务器决定是否使用缓存

协商缓存成功，响应304直接使用缓存

协商缓存失败，返回200和请求的资源

### 用户行为对缓存策略的影响

1.打开网页，地址栏输入地址：查找 disk cache 中是否有匹配，有则使用，没有则发送网络请求

2.普通刷新F5：因为Tab并没有关闭，所以优先从 memory cache 中加载，其次是 disk cache

3.强制刷新Ctrl+F5：浏览器不使用缓存，请求为Cache-Contrl: no-cache，Pragma: no-cache，服务器直接返回200和最新内容

## 详述输入URL到页面渲染的过程

DNS域名解析 >>> TCP分包 >>> IP寻路 >>> 找到服务器IP >>> TCP三次握手建立连接 >>> 数据传输 >>> TCP四次挥手断开连接 >>>

浏览器解析数据 >>> GUI渲染线程 >>> 解析构建DOM树 >>> 解析构建CSSOM >>> 构建Render树 >>> 布局Render树（回流）>>>

绘制Render树（重绘）>>> 将各层信息传给GPU进程，渲染显示到页面

## 如何实现SEO优化

1.对网站的标题，关键字，描述精心设计

2.使用H5语义化标签

3.减少HTTP请求数量

4.利用浏览器缓存

5.减少重绘重排

## 详述回流和重绘优化

当对DOM元素的大小位置进行改动时，会发生回流，回流一定重绘

当对DOM元素的样式进行改动时，会发生重绘，重绘不一定回流

优化：减少对DOM元素的操作，可以将元素移除DOM树再变更，或者利用GPU硬件加速，构建新的复合层

## 防抖和节流

### 函数防抖

什么是函数防抖？事件在触发N秒后执行回调，如果在N秒内又被触发，则重新计时

```
    var timer = null
    var delay = 1000
    btn.onclick = () => {
      clearTimeout(timer)
      timer = setTimeout(() => {
        console.log('====================================');
        console.log('防抖');
        console.log('====================================');
      }, delay);
    }
```

### 函数节流

什么是函数节流？事件在触发N秒后执行回调，在N秒内再次触发无效

```
    var timer = null
    var delay = 1000
    var last = 0
    var now = null
    btn.onclick = () => {
      now = +new Date()
      if (now - last > delay) {
        clearTimeout(timer)
        timer = setTimeout(() => {
          console.log('====================================');
          console.log('节流');
          console.log('====================================');
        }, delay);
        last = now
      }
    }
```

# HTTP

## TCP三次握手

第一次握手：客户端向服务端发送带有syn的数据包，此时客户端变为半打开状态，服务端接收到syn的数据包，服务端也变为半打开状态

第二次握手：服务端向客户端发送带有syn/ack的数据包，客户端接收到后变为established已建立连接状态

第三次握手：客户端向服务端发送带有ack的数据包，服务端收到后变为established已建立连接状态

## TCP四次挥手

第一次挥手：客户端数据传输完成后向服务端发送带fin的数据包，此时客户端进入半关闭状态，不再向服务端发数据，但可以接收服务端的数据

第二次挥手：服务端收到带fin的数据包后，此时服务端端进入半关闭状态，不再接收客户端数据，但可以向客户端发数据，并向客户端发送带ack的确认数据包

第三次挥手：服务端数据传输完成后，向客户端发送带fin的数据包

第四次挥手：客户端接收到带fin的数据包后，向服务端发送带ack的确认数据包，客户端在两个最长报文寿命周期后关闭，服务端在收到ack报文后关闭

## TCP为什么三次握手四次挥手

三次握手：为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误

四次挥手：TCP全双工通信，客户端和服务端都存在收发两种状态

## TCP的流量控制

通过滑动窗口机制有效控制数据发送方的数据发送速率

发送窗口的上限值应该是拥塞窗口和接收窗口之间的最小值

## TCP的拥塞控制

四种算法：慢开始，快重传，快恢复，拥塞避免

## TCP UDP

首先TCP和UDP都是传输层的协议

### TCP

1.TCP是面向连接的，所以可靠性高，无丢包

2.因为TCP是面向连接的，所以会有延时，实时性较差

3.TCP首部开销20字节，开销大

4.TCP连接只能点到点

使用场景：一般用于文件传输，电子邮件，远程终端接入这类对数据准确性要求高，有连接需求的场景

### UDP

1.UDP是面向非连接的，所以可靠性差，可能有丢包

2.因为UDP是面向非连接的，无连接时间，所以实时性好

3.UDP首部开销8字节，开销小

4.UDP支持一对一，一对多，多对一，多对多相互通信

使用场景：一般用于即时通信，流式多媒体通信这种对于实时性要求较高，对丢包要求较低的场景

## HTTP状态码

### 1XX（信息性状态码）

表示接收的请求正在处理

100：继续

101：切换协议，请求者要求服务器切换协议，服务器已确认并准备切换

### 2XX（成功状态码）

表示请求正常处理完毕

200：成功

204：无内容，服务器成功处理了请求，但没有返回任何内容

206：部分内容，服务器成功处理了部分GET请求

### 3XX（重定向状态码）

表示需要进行附加操作以完成请求

301：永久重定向

302：临时重定向，按原有的方法请求重定向地址

303：查看其他位置，用GET方法定向获取请求的资源

304：未修改，自从上次请求后，请求的网页未修改过。服务器返回此响应，不会返回网页的内容

307：临时重定向，不指定用什么方法请求重定向地址

### 4XX（客户端错误状态码）

表示服务器无法处理请求

400：语法错误，请求报文存在语法错误

401：未授权，未通过认证

403：禁止，服务器拒绝请求

404：未找到，服务器未找到指定资源

405：方法禁用，禁用请求中的指定方法

### 5XX（服务器错误状态码）

表示服务器处理请求出错

500：服务器内部错误，服务器遇到错误无法完成请求

503：服务器目前不可用，超载或停机维护

## HTTP的方法

### GET：获取资源

### POST：主要用于提交数据

### PUT：上传文件

### HEAD：获得报文首部

### DELETE：删除文件

### OPTIONS：询问支持的方法

## GET和POST的区别

1.语义不同：GET用来获取数据，POST用来提交数据

2.数据位置不同：GET数据放在URL中，POST数据数放在请求体中，但二者都属于明文传输

3.数据大小不同：GET数据大小受限于浏览器和服务器，最长2048字节，POST无限制

4.数据类型不同：GET只接受ASCII字符，而POST没有限制

5.编码方式不同：GET只能进行URL编码，而POST支持多种编码

6.缓存不同：GET会被浏览器缓存，而POST不会，除非手动设置

7.分包不同：GET产生一个数据包，同时发送Header和Data，POST产生两个数据包，先发Header，响应100，再发Data
但也不是所有浏览器都会发两个包，FireFox就是例外只发一个包

### get与post的区别

- 表单的method属性设置post时发送的是post请求，其余都是get请求
- get请求通过url地址发送请求参数，参数可以直接在地址栏中显示，安全性较差；post是通过请求体发送请求参数，参数不能直接显示，相对安全
- get请求URL地址有长度限制，根据浏览器的不同，限制字节长度不同，post请求没有长度限制



## Cookie和Session的区别

### Cookie

Cookie是为了解决HTTP协议无状态的特点所作出的努力

浏览器第一次发送请求时，服务器设置set-cookie响应，通知浏览器保存Cookie，之后同一域名下的每次请求都会携带cookie

Cookie一般包括如下内容：

1.Key：设置Cookie的key

2.Value：key对应的value

3.Expires：设置过期时间，如果没有设置，则Cookie为会话期Cookie

3.Max-Age：设置Cookie过期时间，权重比Expires高

4.Domain：设置Cookie在哪个域名中有效，如果没有设置，则为当前域名不含子域名，若设置，则自动包含子域名

5.Path：在哪个路径下有效，包含子目录

6.HttpOnly：当这个值为True的时候，表示浏览器不能通过document.cookie更改Cookie的值，可以避免被XSS攻击更改Cookie的值

7.Secure：当这个值为True的时候，Cookie在HTTP中无效，在HTTPS中有效

8.SameSite：规定浏览器不能在跨域请求中携带Cookie，减少CSRF攻击

### Session

Session也是一种HTTP储存机制， 为无状态的HTTP提供持久机制;

Session是存在服务器的一种 用来存放用户数据的类HashTable结构。

浏览器第一次发送请求时，服务器自动生成了一HashTable和一SessionID来唯一标识这个HashTable，并将其通过响应发送到浏览器。浏览器第二次发送请求会将前一次服务器响应中的SessionID放在请求中一并发送到服务器上，服务器从请求中提取出SessionID，并和保存的所有Session ID进行对比，找到这个用户对应的HashTable。

### 二者区别

1.存储位置不同：
Cookie存在在客户端，Session存放在服务端

2.存储大小不同：
单个Cookie容量4KB，而Session大小没有限制（但为了服务器性能考虑，也不宜存放过多数据）

3.存储数据类型不同：
Cookie只支持ASCII字符串，并需要编码方式存储为Unicode或二进制数据，而Session支持任意类型数据

4.存储有效期不同：
Cookie可以设置过期时间长时间存储，而Session在客户端关闭或Session超时都会失效，但如果Cookie不设置过期时间，那Cookie为会话期Cookie

5.安全性不同：
Cookie存放在客户端，安全性较低，Session存放在服务器，安全性较高

6.对服务器压力不同：
Cookie存放在客户端，对服务器无压力，Session存放在服务器，每个用户都会生成一个Session，Session过多消耗服务器资源

7.域支持范围不同：
Cookie可通过设置Domain跨子域访问，Session不支持跨域名访问

## HTTPS

HTTPS = HTTP +TSL/SSL协议加密

HTTP属于明文传输，所以存在 信息窃听 信息篡改 信息劫持 的问题

而 TSL/SSL 利用非对称加密实现身份认证和密钥协商，对称加密采用协商的密钥对数据加密，基于散列函数验证信息的完整性。

非对称加密：客户端共享公钥，服务端掌握私钥，客户端信息只能服务端解密，客户端向服务器发送唯一信息

对称加密：服务器和客户端共享相同密钥，不同客户端密钥不同，服务端维护多个密钥，密钥协商是安全基础

散列算法：函数不可逆，对输入敏感，输出长度固定

HTTPS的工作流程

1.客户端向服务器发送一个HTTPS请求，默认443

2.服务器把事先配置好的公钥证书发送给客户端

3.客户端验证公钥证书：是否过期，与客户端站点是否匹配，是否被吊销，上一级证书是否有效，递归验证到根证书，验证通过继续，不通过显示警告

4.客户端使用伪随机数生成对称加密使用的对称密钥，然后用证书的公钥加密这个对称密钥，发送给服务器

5.服务器使用自己的私钥解密这个对称密钥，这样客户端和服务器拥有了相同的对称密钥

6.服务器使用对称密钥加密数据发送给客户端

7.客户端通过对称密钥解密服务器响应的数据

## HTTP与HTTPS的区别

1.HTTP明文传输，HTTPS加密传输

2.HTTPS需要SSL/TSL证书

3.HTTP默认端口80，HTTPS默认端口443

4.HTTPS更利于SEO

5.SSL/TSL加密在传输层与应用层之间，HTTP基于应用层

1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全



## HTTP2.0

### HTTP1.1存在的问题

#### 1.TCP数量限制

对于同一域名，TCP连接数的限制6~8个，为了解决数量限制，出现了 域名分片 技术：资源分域，将资源放在不同域名下（如二级子域名下）
缺点：每个TCP连接都需要DNS查询，三次握手等占用额外的CPU和资源

#### 2.线头阻塞问题

每个TCP连接同时只能处理一个请求，如果上一个请求未响应，则会阻塞后续请求响应

#### 3.Header内容过多，没有压缩优化方案

#### 4.为了尽可能减少请求数，需要做合并文件，雪碧图，资源内联等优化工作，造成单个请求延迟过高，且内联的资源不能有效地使用缓存机制

### HTTP2.0的优势

#### 1.二进制分帧

帧是数据传输的最小单位，以二进制传输代替原本的明文传输，无需再使用域名分片，合并文件，雪碧图，资源内联

#### 2.多路复用

每个TCP连接上，可以不断向对方发送数据帧，每帧的 stream identifier 标明这个帧属于哪个流，然后在对方接收时，根据 stream identifier 将同一流的所有帧组成一整块数据

流的概念实现了单连接上多请求-响应并行，解决了HTTP1.1的TCP数量限制，线头阻塞的问题

HTTP2.0只需要建立一个TCP连接

还可以对 stream 指定优先级，优先级越高的越先响应

#### 3.服务端推送

浏览器发送一个请求，服务器主动向浏览器推送与这个请求相关的资源，这样浏览器就不用发送后续请求

相比于HTTP1.1资源内联的优势在于：

1.客户端可以缓存推送的资源

2.客户端可以拒收推送的资源

3.推送资源可以由不同页面共享

4.服务器可以按照优先级推送资源

#### 4.Header压缩

使用HPACK算法压缩Header

# React相关

## React的优点

1.声明式编程

2.组件化编程

3.支持客户端和服务端渲染

4.高效：原因在于虚拟DOM和Diff算法

## 虚拟DOM与Diff算法

虚拟DOM本质上是一个轻量的JavaScript对象，它是真实DOM的一个副本

在setState之后创建新的虚拟DOM树，与旧的虚拟DOM树通过Diff算法进行比较

多次setState合并操作，记录比较差异，最终将差异绘制到真实DOM，进行局部更新

## Diff算法比对思路（在shouldComponentUpdate默认为true，未使用key的情况下）

逐层对树节点进行比对

![img](http://localhost:2333/stackedit-lightapp1.0.6/_v_images/20200721161919639_22038.png)
通过比对移除2节点下的5，在3节点下新增5

![img](http://localhost:2333/stackedit-lightapp1.0.6/_v_images/20200721162203303_1206.png)
若认为只需在2下移除4就错了，diff算法会依次比较兄弟节点，所以应该是2节点下移除4增加5，移除5增加6，移除6

这里就体现出key的作用了，若每个组件没有唯一key，则对于5，6两个组件会重新渲染造成性能浪费

若每个组件都有唯一ID，则只需在2下移除4

若有1000个兄弟节点，在没有key的情况下，你只删除第一个兄弟节点，会造成之后999个节点的重新渲染

![img](http://localhost:2333/stackedit-lightapp1.0.6/_v_images/20200721162948694_13975.png)
对于跨层级移动DOM，移除2，同时包括移除2下4，5，增加3，同时包括增加3下2，2下3，4，移除3

可以看到当一个父节点变化时，其下子节点会全部重新渲染

优化：
组件生命周期setState之后，组件本身会进入shouldComponentUpdate()，默认为true，即使没有数据修改，也会render

对于父组件下的子组件，只比父组件多一个componentWillReceiveProps，后面与父组件相同

1.可主动修改此函数，判断新旧数据是否改变，改变返回true，更新组件，否则返回false，不更新组件

2.使用pureComponent纯组件
纯组件重写了shouldComponentUpdate()，对组件的新/旧state和props中的数据进行浅比较, 如果都没有变化, 返回false, 否则返回true

# 算法相关

## 8大排序算法

```
    const array = [7, 8, 6, 6, 6, 4, 5, 2, 3, 1]

    // 1.冒泡排序：相邻两个元素相比较，若a > b则交换顺序，使得最大的元素冒泡到最顶端 时间复杂度 n²
    function bubble(array) {
      for (let i = 0; i < array.length - 1; i++) {
        for (let j = 0; j < array.length - 1 - i; j++) {
          if (array[j] > array[j + 1]) {
            const temp = array[j]
            array[j] = array[j + 1]
            array[j + 1] = temp
          }
        }
      }
      console.log('====================================');
      console.log('bubble', array);
      console.log('====================================');
    }
    // bubble(array)

    // 2.选择排序：每次循环默认首元素为最小元素，通过依次比较寻找最小元素的下标，最后将首元素与最小元素替换 时间复杂度 n²
    function select(array) {
      for (let i = 0; i < array.length - 1; i++) {
        let minIndex = i
        for (let j = i; j < array.length; j++) {
          if (array[minIndex] > array[j]) {
            minIndex = j
          }
        }
        if (minIndex !== i) {
          const temp = array[minIndex]
          array[minIndex] = array[i]
          array[i] = temp
        }
      }
      console.log('====================================');
      console.log('select', array);
      console.log('====================================');
    }
    // select(array)

    // 3.插入排序：每次将一个新元素插入到有序表中，得到一个length+1新的有序表 时间复杂度 n²
    function insert(array) {
      for (let i = 1; i < array.length; i++) {
        // 要插入的牌
        let key = array[i]
        // 有序表最大下标
        let j = i - 1
        // 要插入的牌和有序表的牌比，找到自己的位置
        while (j >= 0 && key < array[j]) {
          // length + 1
          array[j + 1] = array[j]
          j--
        }
        array[j + 1] = key
      }
      console.log('====================================');
      console.log('insert', array);
      console.log('====================================');
    }
    // insert(array)

    // 4.快速排序：先通过一趟排序将序列排列成左右两部分，左部分均比右部分小，再分别对左右两部分进行排序，直到有序 时间复杂度 nlogn
    function partition(array, low, high) {
      // 基准值
      const key = array[low]
      while (low < high) {
        // 在右边寻找比基准值小的数
        while (low < high && key <= array[high]) {
          high--
        }
        // 比基准值小的数移到左边
        array[low] = array[high]
        // 在左边寻找比基准值大的数
        while (low < high && key >= array[low]) {
          low++
        }
        // 比基准值大的数移到右边
        array[high] = array[low]
      }
      // 基准值排序后的正确位置确定
      array[low] = key
      // 输出基准值的正确位置
      return low
    }

    function quick(array, low, high) {
      if (high <= low) {
        return
      }
      const keyIndex = partition(array, low, high)
      // 比基准值小的左边进行递归快排
      quick(array, low, keyIndex - 1)
      // 比基准值大的右边进行递归快排
      quick(array, keyIndex + 1, high)
    }
    // quick(array, 0, array.length-1)

    // 5.堆排序：1.构造大顶堆 2.堆顶元素与末尾元素互换，最大元素沉底 3.调整顶堆 23232323重复 时间复杂度 nlogn
    function heapAdjust(array, current, size) {
      // 取出当前节点
      const key = array[current]
      // 从当前节点的左子开始
      let left = 2 * current + 1
      for (left; left < size; left = 2 * left + 1) {
        // 找出两子中较大的一个
        if (left + 1 < size && array[left] < array[left + 1]) {
          left++
        }
        // 如果子大于父节点，则父子交换
        if (array[left] > key) {
          array[current] = array[left]
          current = left
          array[left] = key
        } else break
      }
    }

    function heap(array, size) {
      // 构建初始大顶堆，从最后一个非叶子节点开始
      for (let left = parseInt(size / 2 - 1); left >= 0; left--) {
        // 调整顶堆
        heapAdjust(array, left, size)
      }
      // 取堆顶并调整
      for (let nextSize = size - 1; nextSize > 0; nextSize--) {
        const temp = array[0]
        array[0] = array[nextSize]
        array[nextSize] = temp
        heapAdjust(array, 0, nextSize)
      }
    }
    // heap(array, array.length)

    // 6.希尔排序：将序列分为gap组，对每组直接使用插排
    // gap/2，反复执行，当gap为1时，为同一组，使用插排，时间复杂度 nlogn
    function shell(array) {
      for (let gap = parseInt(array.length / 2); gap > 0; gap = parseInt(gap / 2)) {
        for (let i = gap; i < array.length; i++) {
          // 要插入的牌的下标为j
          // 序列最大下标为j-gap
          let j = i
          while (j - gap >= 0 && array[j] < array[j - gap]) {
            const temp = array[j]
            array[j] = array[j -gap]
            array[j -gap] = temp
            j -= gap
          }
        }
      }
      console.log('====================================');
      console.log(array);
      console.log('====================================');
    }
    // shell(array)

    // 7.归并排序：将数组分为左右两组分别排序再合并 时间复杂度 nlogn
    function merge(leftArr, rightArr) {
      const result = []
      while (leftArr.length > 0 && rightArr.length > 0) {
        if (leftArr[0] < rightArr[0]) {
          result.push(leftArr.shift())
        } else {
          result.push(rightArr.shift())
        }
      }
      return result.concat(leftArr).concat(rightArr)
    }
    function merge_sort(array) {
      const length = array.length
      if (length === 1) {
        return array
      }
      const mid = parseInt(length / 2)
      const leftArr = array.slice(0, mid)
      const rightArr = array.slice(mid)
      return merge(merge_sort(leftArr), merge_sort(rightArr))
    }
    // const mergeArr = merge_sort(array)
    // console.log('====================================');
    // console.log(mergeArr);
    // console.log('====================================');

    // 8.计数排序：通过额外的数组countArr记录待排序数组中各元素的个数，countArr的索引为待排序数组的值 时间复杂度 n + k
    function count(array) {
      // 获取待排序数组的最大值，也就是新建数组countArr的最大索引
      let max = array[0]
      for (let i = 1; i < array.length; i++) {
        max = array[0] > array[i] ? array[0] : array[i]
      }
      const countArr = new Array(max + 2).fill(0)

      for (let i = 0; i < array.length; i++) {
        countArr[array[i]]++
      }
      let index = 0
      for (let i = 0; i <= max + 1; i++) {
        for(let j = 0; j < countArr[i]; j++){
          array[index++] = i
        } 
      }
      console.log('====================================');
      console.log(array);
      console.log('====================================');
      
    }
    // count(array)
```

# 全部类型

## **css相关**

**2. BFC优化**

块格式化上下文, 特性:

- 使 BFC 内部浮动元素不会到处乱跑；
- 和浮动元素产生边界。

**3. 盒模型哪两种模式？什么区别？如何设置**

- 标准模式: box-sizing: content-box; 宽高不包括内边距和边框
- 怪异模式: box-sizing: border-box

**4. 常用清除浮动的方法，如不清除浮动会怎样？**

当父元素不给高度的时候，内部元素不浮动时会撑开, 而浮动的时候，父元素变成一条线, 造成塌陷.

- 额外标签法（在最后一个浮动标签后，新加一个标签，给其设置clear：both；）（不推荐）
- 父元素添加overflow:hidden; (触发BFC)
- 使用after伪元素清除浮动（推荐使用）
- 使用before和after双伪元素清除浮动

**5. 删格化的原理**

比如antd的row和col, 将一行等分为24份, col是几就占几份, 底层按百分比实现; 结合媒体查询, 可以实现响应式

**6. 纯css实现三角形**

```
// 通过设置border.box        {            width:0px;            height:0px;
            border-top:50px solid rgba(0,0,0,0);            border-right:50px solid  rgba(0,0,0,0);            border-bottom:50px solid green;            border-left:50px solid  rgba(0,0,0,0);            }
```

**7. 高度不定，宽100%，内一p高不确定，如何实现垂直居中？**

- verticle-align: middle;
- 绝对定位50%加translateY(-50%)
- 绝对定位，上下左右全0，margin:auto

**9.

**10. css菊花图**

四个小圆点一直旋转

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// 父标签animation: antRotate 1.2s infinite linear;// 子标签animation: antSpin 1s infinite linear;@keyframe antSpin {  to {    opacity: 1  }}@keyframe antRotate {  to {    transform: rotate(405)  }}// animation-delay: 逐个延迟0.4s
```

**11. 关于em**

- 
- 
- 
- 
- 

```
 <p style="font-size: 20px">      123      <p style="font-size: 2em;width: 2em">456</p> </p>// 此时子元素的font-size为40px, 宽度为80px(还要乘以子元素font-size的系数)
```

**12. 关于vh, vw**

vw：viewpoint width，视窗宽度，1vw等于视窗宽度的1%。
vh：viewpoint height，视窗高度，1vh等于视窗高度的1%。
vmin：vw和vh中较小的那个。
vmax：vw和vh中较大的那个。

**13. Flex布局**

- flex-direction控制主副轴
- flex-wrap控制换行(默认不换行)
- flex-flow是上两个的结合
- justify-content主轴对齐方式
- align-items交叉轴对齐方式

**14. overflow原理**

- `overflow: hidden`能清除块内子元素的浮动影响. 因为该属性进行超出隐藏时需要计算盒子内所有元素的高度, 所以会隐式清除浮动

- 创建BFC条件(满足一个):

- - float的值不为none；
  - overflow的值不为visible；
  - position的值为fixed / absolute；
  - display的值为table-cell / table-caption / inline-block / flex / inline-flex。

**15. 实现自适应的正方形:**

- 使用vw, vh
- `width`百分比, `height: 0`, `padding-top(bottom): 50%`

**16. 标准模式和怪异模式**

- document.compatMode属性可以判断是否是标准模式，当 document.compatMode为“CSS1Compat”，是标准模式，“BackCompat”是怪异模式。
- 怪异模式是为了兼容旧版本的浏览器, 因为IE低版本document.documentElement.clientWidth获取不到
- 怪异模式盒模型: `box-sizing: border-box`; 标准模式: `box-sizing: content-box`

**17. CSS3实现环形进度条**

两个对半矩形遮罩, 使用`rotate`以及`overflow: hidden`进行旋转

**18. css优先级**

选择器的特殊性值表述为4个部分，用0,0,0,0表示。

- ID选择器的特殊性值，加0,1,0,0。
- 类选择器、属性选择器或伪类，加0,0,1,0。
- 元素和伪元素，加0,0,0,1。
- 通配选择器*对特殊性没有贡献，即0,0,0,0。
- 最后比较特殊的一个标志!important（权重），它没有特殊性值，但它的优先级是最高的，为了方便记忆，可以认为它的特殊性值为1,0,0,0,0。

## JS相关

**1. ES5和ES6继承方式区别**

- ES5定义类以函数形式, 以prototype来实现继承
- ES6以class形式定义类, 以extend形式继承

**2. Generator了解**

ES6 提供的一种异步编程解决方案, Generator 函数是一个状态机，封装了多个内部状态。

- 
- 
- 
- 
- 
- 
- 

```
function* helloWorldGenerator() {  yield 'hello';  yield 'world';  return 'ending';}
var hw = helloWorldGenerator();
```

调用后返回指向内部状态的指针, 调用next()才会移向下一个状态, 参数:

- 
- 
- 
- 
- 
- 
- 
- 

```
hw.next()// { value: 'hello', done: false }hw.next()// { value: 'world', done: false }hw.next()// { value: 'ending', done: true }hw.next()// { value: undefined, done: true }
```

**3. 手写Promise实现**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
var myPromise = new Promise((resolve, reject) => {  // 需要执行的代码  ...  if (/* 异步执行成功 */) {    resolve(value)  } else if (/* 异步执行失败 */) {    reject(error)  }})
myPromise.then((value) => {  // 成功后调用, 使用value值}, (error) => {  // 失败后调用, 获取错误信息error})
```

**4. Promise优缺点**

- 优点: 解决回调地狱, 对异步任务写法更标准化与简洁化
- 缺点: 首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消; 其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部; 第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成).
  极简版promise封装:

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
function promise () {  this.msg = '' // 存放value和error  this.status = 'pending'  var that = this  var process = arguments[0]
  process (function () {    that.status = 'fulfilled'    that.msg = arguments[0]  }, function () {    that.status = 'rejected'    that.msg = arguments[0]  })  return this}
promise.prototype.then = function () {  if (this.status === 'fulfilled') {    arguments[0](this.msg)  } else if (this.status === 'rejected' && arguments[1]) {    arguments[1](this.msg)  }}
```

**5. 观察者模式**

又称发布-订阅模式, 举例子说明.
实现: 发布者管理订阅者队列, 并有新消息推送功能. 订阅者仅关注更新就行

**6. 手写实现bind**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
Function.prototype.bind = function () {   // 保存原函数  var self = this  // 取出第一个参数作为上下文, 相当于[].shift.call(arguments)  var context = Array.prototype.shift.call(arguments)  // 取剩余的参数作为arg; 因为arguments是伪数组, 所以要转化为数组才能使用数组方法  var arg = Array.prototype.slice.call(arguments)  // 返回一个新函数  return function () {    // 绑定上下文并传参    self.apply(context, Array.prototype.concat.call(arg, Array.prototype.slice.call(arguments)))  }}
```

**7. 手写实现4种继承**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
function Father () {}function Child () {}// 1\. 原型继承Child.prototype = new Father()// 2\. 构造继承function Child (name) {  Father.call(this, name)}// 3\. 组合继承function Child (name) {  Father.call(this, name)}Child.prototype = new Father()// 4\. 寄生继承function cloneObj (o) {  var clone = object.create(o)  clone.sayName = ...  return clone}// 5\. 寄生组合继承// 6\. ES6 class extend继承
```

**8. css菊花图**

四个小圆点一直旋转

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// 父标签animation: antRotate 1.2s infinite linear;// 子标签animation: antSpin 1s infinite linear;@keyframe antSpin {  to {    opacity: 1  }}@keyframe antRotate {  to {    transform: rotate(405)  }}// animation-delay: 逐个延迟0.4s
```

**9. http状态码**

- 1**: 服务器收到请求, 需请求者进一步操作
- 2**: 请求成功
- 3**: 重定向, 资源被转移到其他URL了
- 4**: 客户端错误, 请求语法错误或没有找到相应资源
- 5**: 服务端错误, server error
- 304: Not Modified. 指定日期后未修改, 不返回资源

**10. Object.create实现（原型式继承，特点：实例的proto指向构造函数本身）**

**11. async和await：**

- Generator函数的语法糖，将*改成async，将yield换成await。
- 是对Generator函数的改进, 返回promise。
- 异步写法同步化，遇到await先返回，执行完异步再执行接下来的.
- 内置执行器, 无需next()

**12. 算法和数据结构：**

- 算法：
  解决具体问题所需要的解决方法。执行效率最快的最优算法。时间复杂度。输入，输出，有穷性，确定性，可行性。冒泡排序，二叉树遍历，最长回文，二分查找，指针，链表等，堆栈，队列等。力扣，codewar，算法导论。
- 数据结构：
  逻辑结构：集合、线性、树形、图形结构
  物理结构：顺序、链式存储结构

**13. 封装JSONP**

![1573610173410292.png](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
function jsonp ({url, param, callback}) {  return new Promise((resolve, reject) => {    var script = document.createElement('script')    window.callback = function (data) {      resolve(data)      document.body.removeChild('script')    }    var param = {...param, callback}    var arr = []    for (let key in param) {      arr.push(`${key}=${param[key]}`)    }    script.src = `${url}?${arr.join('&')}`    document.body.appendChild(script)  })}
```

**14. 手动实现map(forEach以及filter也类似)**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// for循环实现Array.prototype.myMap = function () {  var arr = this  var [fn, thisValue] = Array.prototype.slice.call(arguments)  var result = []  for (var i = 0; i < arr.length; i++) {    result.push(fn.call(thisValue, arr[i], i, arr))  }  return result}var arr0 = [1, 2, 3]console.log(arr0.myMap(v => v + 1))
// forEach实现(reduce类似)Array.prototype.myMap = function (fn, thisValue) {  var result = []  this.forEach((v, i, arr) => {    result.push(fn.call(thisValue, v, i, arr))  })  return result}var arr0 = [1, 2, 3]console.log(arr0.myMap(v => v + 1))
```

**15. js实现checkbox全选以及反选**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
<body>    <button id="other">反选</button>    <input type="checkbox" id="all" />全选    <input type="checkbox" class="check" />1    <input type="checkbox" class="check" />2    <input type="checkbox" class="check" />3    <script>      var checkbox = document.getElementsByClassName('check')      var checkAll = document.getElementById('all')      var checkOther = document.getElementById('other')      checkAll.onclick = function() {        var flag = true        for (var i = 0; i < checkbox.length; i++) {          if (!checkbox[i].checked) flag = false        }        if (flag) {          for (var i = 0; i < checkbox.length; i++) {            checkbox[i].checked = false          }        } else {          for (var i = 0; i < checkbox.length; i++) {            checkbox[i].checked = true          }        }      }      checkOther.onclick = function() {        for (var i = 0; i < checkbox.length; i++) {          checkbox[i].checked = !checkbox[i].checked        }      }</script>  </body>
```

**16. 对原型链的理解？prototype上都有哪些属性**

- 在js里，继承机制是原型继承。继承的起点是 对象的原型（Object prototype）。
- 一切皆为对象，只要是对象，就会有 proto 属性，该属性存储了指向其构造的指针。
- Object prototype也是对象，其 proto 指向null。
- 对象分为两种：函数对象和普通对象，只有函数对象拥有『原型』对象（prototype）。
- prototype的本质是普通对象。
- Function prototype比较特殊，是没有prototype的函数对象。
- new操作得到的对象是普通对象。
- 当调取一个对象的属性时，会先在本身查找，若无，就根据 proto 找到构造原型，若无，继续往上找。最后会到达顶层Object prototype，它的 proto 指向null，均无结果则返回undefined，结束。
- 由 proto 串起的路径就是『原型链』。
- 通过prototype可以给所有子类共享属性

**17. 为什么使用继承**

通常在一般的项目里不需要,因为应用简单,但你要用纯js做一些复杂的工具或框架系统就要用到了,比如webgis、或者js框架如jquery、ext什么的,不然一个几千行代码的框架不用继承得写几万行,甚至还无法维护。

**18. setTimeout时间延迟为何不准**

单线程, 先执行同步主线程, 再执行异步任务队列

**19. 事件循环述，宏任务和微任务有什么区别？**

- 先主线程后异步任务队列
- 先微任务再宏任务

**20. let const var作用域**

块级作用域, 暂时性死区

**21. 节流和防抖**

- 函数节流是指一定时间内js方法只跑一次。比如人的眨眼睛，就是一定时间内眨一次。这是函数节流最形象的解释。

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// 函数节流   滚动条滚动var canRun = true;document.getElementById("throttle").onscroll = function(){    if(!canRun){        // 判断是否已空闲，如果在执行中，则直接return        return;    }
    canRun = false;    setTimeout(function(){        console.log("函数节流");        canRun = true;    }, 300);};
```

- 函数防抖是指频繁触发的情况下，只有足够的空闲时间，才执行代码一次。比如生活中的坐公交，就是一定时间内，如果有人陆续刷卡上车，司机就不会开车。只有别人没刷卡了，司机才开车。

- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// 函数防抖var timer = false;document.getElementById("debounce").onscroll = function(){    clearTimeout(timer); // 清除未执行的代码，重置回初始化状态
    timer = setTimeout(function(){        console.log("函数防抖");    }, 300);};
```

**22. 实现一个sleep函数**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// 这种实现方式是利用一个伪死循环阻塞主线程。因为JS是单线程的。所以通过这种方式可以实现真正意义上的sleep()。function sleep(delay) {  var start = (new Date()).getTime();  while ((new Date()).getTime() - start < delay) {    continue;  }}
function test() {  console.log('111');  sleep(2000);  console.log('222');}
test()
```

**23. 闭包**

- 概念: 内层函数能够访问外层函数作用域的变量

- 缺点: 引起内存泄漏（释放内存）

- 作用:

- - 使用闭包修正打印值
  - 实现柯里化
  - 实现node commonJs 模块化, 实现私有变量
  - 保持变量与函数活性, 可延迟回收和执行

**24. Immutable.js**

Facebook出品, 倡导数据的不可变性, 用的最多就是List和Map.

**25. js实现instanceof**

- 
- 
- 
- 
- 
- 
- 
- 

```
// 检测l的原型链（__proto__）上是否有r.prototype，若有返回true，否则falsefunction myInstanceof (l, r) {  var R = r.prototype  while (l.__proto__) {    if (l.__proto__ === R) return true  }  return false}
```

**27. ES6的模块引入和CommonJs区别**

**28. 严格模式**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// 严格模式下, 隐式绑定丢失后this不会指向window, 而是指向undefined      'use strict'      var a = 2      var obj = {        a: 1,        b: function() {          // console.log(this.a)          console.log(this)        }      }      var c = obj.b      c() // undefined
```

**29. fetch, axios区别**

**30. typescript缺点**

- 并不是严格意义的js的超集, 与js不完全兼容, 会报错
- 更多的限制, 是一种桎梏
- 有些js第三方库没有dts, 有问题

**31. 构造函数实现原理**

- 构造函数中没有显示的创建Object对象, 实际上后台自动创建了
- 直接给this对象赋值属性和方法, this即指向创建的对象
- 没有return返回值, 后台自动返回了该对象

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
// 模拟构造函数实现var Book = function(name) {          this.name = name;        };
        //正常用法        var java = new Book(‘Master Java’);
        //使用代码模拟，在非IE浏览器中测试，IE浏览器不支持        var python = {};        python.__proto__ = Book.prototype;        Book.call(python, 'Master Python');
```

**32. for in 和 for of区别**

- `for in`遍历数组会遍历到数组原型上的属性和方法, 更适合遍历对象
- `forEach`不支持`break, continue, return`等
- 使用`for of`可以成功遍历数组的值, 而不是索引, 不会遍历原型
- for in 可以遍历到myObject的原型方法method,如果不想遍历原型方法和属性的话，可以在循环内部判断一下,hasOwnPropery方法可以判断某属性是否是该对象的实例属性

**33. JS实现并发控制:**

使用消息队列以及`setInterval`或`promise`进行入队和出队

**34. ajax和axios、fetch的区别**

**35. promise.finally实现**

- 
- 
- 
- 
- 
- 
- 

```
Promise.prototype.finally = function (callback) {  let P = this.constructor;  return this.then(    value  => P.resolve(callback()).then(() => value),    reason => P.resolve(callback()).then(() => { throw reason })  );};
```

## 浏览器网络相关 

**1. reflow(回流)和repaint(重绘)优化**

![1573610281152620.png](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- 浏览器渲染过程: DOM tree, CSS tree --> Render tree --> Paint

- DOM tree根节点为html

- 渲染从浏览器左上角到右下角

- 第一次打开页面至少触发一次重绘和回流, 结构如宽高位置变化时, 触发**reflow回流**;非结构如背景色变化时, 触发**repaint重绘**. 二者都会造成体验不佳

- 如何减少重绘和回流?

- - 通过classname或cssText一次性修改样式, 而非一个一个改
  - 离线模式: 克隆要操作的结点, 操作后再与原始结点交换, 类似于虚拟DOM
  - 避免频繁直接访问计算后的样式, 而是先将信息保存下来
  - 绝对布局的DOM, 不会造成大量reflow
  - p不要嵌套太深, 不要超过六层

**2.一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？**

- 浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
- 服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图象等）；
- 浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM Tree）；
- 载入解析到的资源文件，渲染页面，完成。

**3.localStorage 与 sessionStorage 与cookie的区别总结**

- **共同点**: 都保存在浏览器端, 且同源
- localStorage 与 sessionStorage 统称webStorage,保存在浏览器,不参与服务器通信,大小为5M
- **生命周期不同**: localStorage永久保存, sessionStorage当前会话, 都可手动清除
- **作用域不同**: 不同浏览器不共享local和session, 不同会话不共享session
- **Cookie**: 设置的过期时间前一直有效, 大小4K.有个数限制, 各浏览器不同, 一般为20个.携带在HTTP头中, 过多会有性能问题.可自己封装, 也可用原生

**4.浏览器如何阻止事件传播，阻止默认行为**

- 阻止事件传播(冒泡): e.stopPropagation()
- 阻止默认行为: e.preventDefault()

**5.虚拟DOM方案相对原生DOM操作有什么优点，实现上是什么原理？**

虚拟DOM可提升性能, 无须整体重新渲染, 而是局部刷新.
JS对象, diff算法

**6.浏览器事件机制中事件触发三个阶段**

- **事件捕获阶段**: 从dom树节点往下找到目标节点, 不会触发函数

- **事件目标处理函数**: 到达目标节点

- **事件冒泡**: 最后从目标节点往顶层元素传递, 通常函数在此阶段执行.
  addEventListener第三个参数默认false(冒泡阶段执行),true(捕获阶段执行).
  阻止冒泡见以上方法



**7.什么是跨域？为什么浏览器要使用同源策略？你有几种方式可以解决跨域问题？了解预检请求嘛？**

- 跨域是指一个域下的文档或脚本试图去请求另一个域下的资源
- 防止XSS、CSFR等攻击, 协议+域名+端口不同
- jsonp; 跨域资源共享（CORS）(Access control); 服务器正向代理等

![1573610329460534.png](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- **预检请求**: 需预检的请求要求必须首先使用 OPTIONS 方法发起一个预检请求到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响

**8.了解浏览器缓存机制吗？**

- 浏览器缓存就是把一个已经请求过的资源拷贝一份存储起来，当下次需要该资源时，浏览器会根据缓存机制决定直接使用缓存资源还是再次向服务器发送请求.
- from memory cache ; from disk cache
- 作用: 减少网络传输的损耗以及降低服务器压力。
- 优先级: 强制缓存 > 协商缓存; cache-control > Expires > Etag > Last-modified

**9.为什么操作 DOM 慢?**

DOM本身是一个js对象, 操作这个对象本身不慢, 但是操作后触发了浏览器的行为, 如repaint和reflow等浏览器行为, 使其变慢

**10.什么情况会阻塞渲染？**

- js脚本同步执行
- css和图片虽然是异步加载, 但js文件执行需依赖css, 所以css也会阻塞渲染

**11.如何判断js运行在浏览器中还是node中？**

判断有无全局对象global和window

**12.关于web以及浏览器处理预加载有哪些思考？**

图片等静态资源在使用之前就提前请求
资源使用到的时候能从缓存中加载, 提升用户体验
页面展示的依赖关系维护

**13.http多路复用**

- **Keep-Alive**: Keep-Alive解决的核心问题：一定时间内，同一域名多次请求数据，只建立一次HTTP请求，其他请求可复用每一次建立的连接通道，以达到提高请求效率的问题。这里面所说的一定时间是可以配置的，不管你用的是Apache还是nginx。
- 解决两个问题: 串行文件传输(采用二进制数据帧); 连接数过多(采用流, 并行传输)

**14. http和https：**

- http: 最广泛网络协议，BS模型，浏览器高效。
- https: 安全版，通过SSL加密，加密传输，身份认证，密钥

1. https相对于http加入了ssl层, 加密传输, 身份认证;
2. 需要到ca申请收费的证书;
3. 安全但是耗时多，缓存不是很好;
4. 注意兼容http和https;
5. 连接方式不同, 端口号也不同, http是80, https是443

**15. CSRF和XSS区别及防御**

**16. cookie可设置哪些属性？httponly?**

chrome控制台的application下可查看:

![1573610354419777.png](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

- name　　字段为一个cookie的名称。
- value　　字段为一个cookie的值。
- domain　　字段为可以访问此cookie的域名。
- path　　字段为可以访问此cookie的页面路径。比如domain是abc.com,path是/test，那么只有/test路径下的页面可以读取此cookie。
- expires/Max-Age 　　字段为此cookie超时时间。若设置其值为一个时间，那么当到达此时间后，此cookie失效。不设置的话默认值是Session，意思是cookie会和session一起失效。当浏览器关闭(不是浏览器标签页，而是整个浏览器) 后，此cookie失效。
- Size　　字段 此cookie大小。
- http　　字段 cookie的httponly属性。若此属性为true，则只有在http请求头中会带有此cookie的信息，而不能通过document.cookie来访问此cookie。
- secure　　 字段 设置是否只能通过https来传递此条cookie

**17. 登录后，前端做了哪些工作，如何得知已登录**

- 前端存放服务端下发的cookie, 简单说就是写一个字段在cookie中表明已登录, 并设置失效日期
- 或使用后端返回的token, 每次ajax请求将token携带在请求头中, 这也是防范csrf的手段之一

**18. http状态码**

- 1**: 服务器收到请求, 需请求者进一步操作
- 2**: 请求成功
- 3**: 重定向, 资源被转移到其他URL了
- 4**: 客户端错误, 请求语法错误或没有找到相应资源
- 5**: 服务端错误, server error
- 301: 资源(网页等)被永久转移到其他URL, 返回值中包含新的URL, 浏览器会自动定向到新URL
- 302: 临时转移. 客户端应访问原有URL
- 304: Not Modified. 指定日期后未修改, 不返回资源
- 403: 服务器拒绝执行请求
- 404: 请求的资源(网页等)不存在
- 500: 内部服务器错误

**19. # Http请求头缓存设置方法**

Cache-control, expire, last-modify

**20. 实现页面回退刷新**

- 旧: window.history.back() + window.location.href=document.referrer;
- 新: HTML5的新API扩展了window.history，使历史记录点更加开放了。可以存储当前历史记录点、替换当前历史记录点、监听历史记录点onpopstate, replaceState

**21. 正向代理和反向代理**

- 正向代理:

![1573610397120879.png](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

（1）访问原来无法访问的资源，如google
（2） 可以做缓存，加速访问资源
（3）对客户端访问授权，上网进行认证
（4）代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息

- 反向代理:

![1573610403768391.png](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

（1）保证内网的安全，可以使用反向代理提供WAF功能，阻止web攻击大型网站，通常将反向代理作为公网访问地址，Web服务器是内网。

（2）负载均衡，通过反向代理服务器来优化网站的负载



**22. 关于预检请求**

在非简单请求且跨域的情况下，浏览器会自动发起options预检请求。

**23. 三次握手四次挥手**

- 开启连接用三次握手, 关闭用四次挥手

**24. TCP和UDP协议**

- TCP（Transmission Control Protocol：传输控制协议；面向连接，可靠传输
- UDP（User Datagram Protocol）：用户数据报协议；面向无连接，不可靠传输

**25. 进程和线程的区别**

- 进程：是并发执行的程序在执行过程中分配和管理资源的基本单位，是一个动态概念，竞争计算机系统资源的基本单位。
- 线程：是进程的一个执行单元，是进程内科调度实体。比进程更小的独立运行的基本单位。线程也被称为轻量级进程。
- 一个程序至少一个进程，一个进程至少一个线程。

## vue相关

**1. 生命周期**

![1573610434918482.png](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**2 .双向数据绑定v-model。这个最好也是自己实现一下 理解更深**

通过v-model
VUE实现双向数据绑定的原理就是利用了 Object.defineProperty() 这个方法重新定义了对象获取属性值(get)和设置属性值(set)的操作来实现的。

```
// 依赖收集// 简化版var obj = { }var name//第一个参数：定义属性的对象。//第二个参数：要定义或修改的属性的名称。//第三个参数：将被定义或修改的属性描述符。Object.defineProperty(obj, "data", {  //获取值  get: function () {    return name  },  //设置值  set: function (val) {    name = val    console.log(val)  }})//赋值调用setobj.data = 'aaa'//取值调用getconsole.log(obj.data)
// 详细版 myVue.prototype._obverse = function (obj) { // obj = {number: 0}    var value;    for (key in obj) {  //遍历obj对象      if (obj.hasOwnProperty(key)) {        value = obj[key];        if (typeof value === 'object') {  //如果值是对象，则递归处理          this._obverse(value);        }        Object.defineProperty(this.$data, key, {  //关键          enumerable: true,          configurable: true,          get: function () {            console.log(`获取${value}`);            return value;          },          set: function (newVal) {            console.log(`更新${newVal}`);            if (value !== newVal) {              value = newVal;            }          }        })      }    }  }
```

**3.vue父子组件传递参数**

- 父 -->子: 通过props
- 子 -->父: 通过 $$refs 或 $emit

**4.vue传递参数方法**

- 父子组件传参如上, v-bind : v-on @
- 兄弟组件传参:(通过EventBus事件总线实现)

  


```
// 1\. 新建eventBus.jsimport Vue from 'vue'export default new Vue// 或直接在main.js中初始化EventBus(全局)Vue.prototype.$EventBus = new Vue()
// 2\. 发射与接收// 如果是定义在eventBus.js中import eventBus from 'eventBus.js'eventBus.$emit()eventBus.$on()
// 如果是定义在main.js中this.bus.$emit()this.bus.$on()
// 3\. 移除监听eventBus.$off()
```

**5.vue自定义组件**

可以使用独立可复用的自定义组件来构成大型应用, 采用帕斯卡命名法或横线连接, 通过以上方式进行组件间通信. 每一个组件都是Vue实例, 可以使用生命周期钩子.

**6. vue自定义指令**

- 除核心指令之外的指令, 使用directive进行注册.
- 指令自定义钩子函数: bind, inserted, update, componentUpdated, unbind

**7.vuex组成和原理**

- 组成: 组件间通信, 通过store实现全局存取
- 修改: 唯一途径, 通过commit一个mutations(同步)或dispatch一个actions(异步)
- 简写: 引入mapState、mapGetters、mapActions

**8.vue-router的原理，例如hashhistory和History interface这些东西要弄明白。其实看一下源码就好了，看不懂可以直接看解析的相关技术博客。**

- vue-router用法:
  在router.js或者某一个路由分发页面配置path, name, component对应关系

- - 每个按钮一个value, 在watch功能中使用this.$router.push实现对应跳转, 类似react的this.history.push
  - 或直接用router-link to去跳转, 类似react的link to

- vue-router原理: 通过**hash**和**History interface**两种方式实现前端路由

- - HashHistory: 利用URL中的hash（“#”）;replace()方法与push()方法不同之处在于，它并不是将新路由添加到浏览器访问历史的栈顶，而是替换掉当前的路由
  - History interface: 是浏览器历史记录栈提供的接口，通过back(), forward(), go()等方法，我们可以读取浏览器历史记录栈的信息，进行各种跳转操作. pushState(), replaceState() 这下不仅是读取了，还可以对浏览器历史记录栈进行修改

**9.vue的seo问题**

seo关系到网站排名, vue搭建spa做前后端分离不好做seo, 可通过其他方法解决:

- SSR服务端渲染: 将同一个组件渲染为服务器端的 HTML 字符串.利于seo且更快.
- vue-meta-info, nuxt, prerender-spa-plugin页面预渲染等

**10.预渲染和ssr**
以上

**11.生命周期内create和mounted的区别**

- **created**: 在模板渲染成html前调用，即通常初始化某些数据，然后再渲染成视图。
- **mounted**: 在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作和方法。

**12.监听watch**

对应一个对象，键是观察表达式，值是对应回调。值也可以是methods的方法名，或者是对象，包含选项。在实例化时为每个键调用 $watch()

**13.登录验证拦截(通过router)**

- 先设置requireAuth:

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
routes = [    {        name: 'detail',        path: '/detail',        meta: {            requireAuth: true        }    },    {        name: 'login',        path: '/login'    }]
```

- 再配置router.beforeEach:

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
router.beforeEach((from, to, next) => {    if (to.meta.requireAuth) { // 判断跳转的路由是否需要登录        if (store.state.token) { // vuex.state判断token是否存在            next() // 已登录        } else {            next({                path: '/login',                query: {redirect: to.fullPath} // 将跳转的路由path作为参数，登录成功后跳转到该路由            })        }    } else {       next()    }})
```

**14. v-for key值**

不写key值会报warning, 和react的array渲染类似. 根据diff算法, 修改数组后, 写key值会复用, 不写会重新生成, 造成性能浪费或某些不必要的错误

**15. vue3.0的更新和defineProperty优化**

- 放弃 Object.defineProperty ，使用更快的原生 Proxy (访问对象拦截器, 也成代理器)
- 提速, 降低内存使用, Tree-shaking更友好
- 支持IE11等
- 使用Typescript

**15. vue使用this获取变量**

正常要通过vm.[图片上传失败...(image-6d2f4e-1570591304185)]

root传参取值

**16. jQuery的优缺点，与vue的不同，vue的优缺点？**

- jq优点: 比原生js更易书写, 封装了很多api, 有丰富的插件库; 缺点: 每次升级与之前版本不兼容, 只能手动开发, 操作DOM很慢, 不方便, 变量名污染, 作用域混淆等。
- vue优缺点: 双向绑定, 虚拟DOM, diff算法, MVVM, 组件化, 通信方便, 路由分发等

**17. vue解除双向绑定**

- 

```
let obj = JSON.parse(JSON.stringify(this.temp1));
```

**18. vue异步组件**

为了简化，Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染

- 
- 
- 
- 
- 

```
Vue.component(  'async-webpack-example',  // 这个 `import` 函数会返回一个 `Promise` 对象。  () => import('./my-async-component'))
```

**19. MVC与MVVM**

- model-数据层 view-视图层 controller-控制层
- MVC的目的是实现M和V的分离，单向通信，必须通过C来承上启下
- MVVM中通过VM（vue中的实例化对象）的发布者-订阅者模式实现双向绑定，数据绑定，dom事件监听
- 区别：MVC和MVVM的区别并不是VM完全取代了C，ViewModel存在目的在于抽离Controller中展示的业务逻辑，而不是替代Controller，其它视图操作业务等还是应该放在Controller中实现。也就是说MVVM实现的是业务逻辑组件的重用

**20. vue渐进式**

小到可以只使用核心功能，比如单文件组件作为一部分嵌入；大到使用整个工程，vue init webpack my-project来构建项目；VUE的核心库及其生态系统也可以满足你的各式需求（core+vuex+vue-route）

## react相关

**1. 新旧生命周期**

- **旧**: will, did; mount, update...

- **新**: 16版本之后:

- - `getDerivedStateFromProps`: 虚拟dom之后，实际dom挂载之前, 每次获取新的props或state之后, 返回新的state, 配合didUpdate可以替代willReceiveProps
  - `getSnapshotBeforeUpdate`: update发生的时候，组件更新前触发, 在render之后，在组件dom渲染之前；返回一个值，作为componentDidUpdate的第三个参数；配合componentDidUpdate, 可以覆盖componentWillUpdate的所有用法
  - `componentDidCatch`: 错误处理

- **对比**: 弃用了三个will, 新增两个get来代替will, 不能混用, 17版本会彻底删除. 新增错误处理

**2. react核心**

- 虚拟DOM, Diff算法, 遍历key值
- react-dom: 提供了针对DOM的方法，比如：把创建的虚拟DOM，渲染到页面上 或 配合ref来操作DOM
- react-router

**3. fiber核心(react 16)**

- 旧: 浏览器渲染引擎单线程, 计算DOM树时锁住整个线程, 所有行为同步发生, 有效率问题, 期间react会一直占用浏览器主线程，如果组件层级比较深，相应的堆栈也会很深，长时间占用浏览器主线程, 任何其他的操作（包括用户的点击，鼠标移动等操作）都无法执行。
- 新: 重写底层算法逻辑, 引入fiber时间片, 异步渲染, react会在渲染一部分树后检查是否有更高优先级的任务需要处理(如用户操作或绘图), 处理完后再继续渲染, 并可以更新优先级, 以此管理渲染任务. 加入fiber的react将组件更新分为两个时期（phase 1 && phase 2），render前的生命周期为phase1，render后的生命周期为phase2, 1可以打断, 2不能打断一次性更新. 三个will生命周期可能会重复执行, 尽量避免使用。

**4. 渲染一个react**

- 分为首次渲染和更新渲染
- 生命周期, 建立虚拟DOM, 进行diff算法
- 对比新旧DOM, 节点对比, 将算法复杂度从O(n^3)降低到O(n)
- key值优化, 避免用index作为key值, 兄弟节点中唯一就行

**5. 高阶组件**

高阶组件就是一个函数，且该函数(wrapper)接受一个组件作为参数，并返回一个新的组件。
高阶组件并不关心数据使用的方式和原因，而被包裹的组件也不关心数据来自何处.

- react-dnd: 根组件, source, target等
  `export default DragSource(type, spec, collect)(MyComponent)`
- 重构代码库使用HOC提升开发效率

**6. hook(v16.7测试)**

在无状态组件(如函数式组件)中也能操作state以及其他react特性, 通过useState

**7. redux和vuex以及dva：**

- redux: 通过store存储，通过action唯一更改，reducer描述如何更改。dispatch一个action
- dva: 基于redux，结合redux-saga等中间件进行封装
- vuex：类似dva，集成化。action异步，mutation非异步

**8. react和vue的区别**

- **数据是否可变**: react整体是函数式的思想，把组件设计成纯组件，状态和逻辑通过参数传入，所以在react中，是单向数据流，推崇结合immutable来实现数据不可变; vue的思想是响应式的，也就是基于是数据可变的，通过对每一个属性建立Watcher来监听，当属性变化的时候，响应式的更新对应的虚拟dom。总之，react的性能优化需要手动去做，而vue的性能优化是自动的，但是vue的响应式机制也有问题，就是当state特别多的时候，Watcher也会很多，会导致卡顿，所以大型应用（状态特别多的）一般用react，更加可控。
- **通过js来操作一切，还是用各自的处理方式**: react的思路是all in js，通过js来生成html，所以设计了jsx，还有通过js来操作css，社区的styled-component、jss等; vue是把html，css，js组合到一起，用各自的处理方式，vue有单文件组件，可以把html、css、js写到一个文件中，html提供了模板引擎来处理。
- **类式的组件写法，还是声明式的写法**: react是类式的写法，api很少; 而vue是声明式的写法，通过传入各种options，api和参数都很多。所以react结合typescript更容易一起写，vue稍微复杂。
- **扩展不同**: react可以通过高阶组件（Higher Order Components--HOC）来扩展，而vue需要通过mixins来扩展。
- **什么功能内置，什么交给社区去做**: react做的事情很少，很多都交给社区去做，vue很多东西都是内置的，写起来确实方便一些，
  比如 redux的combineReducer就对应vuex的modules，
  比如reselect就对应vuex的getter和vue组件的computed，
  vuex的mutation是直接改变的原始数据，而redux的reducer是返回一个全新的state，所以redux结合immutable来优化性能，vue不需要。

**9. react单向数据流怎么理解**

React是单向数据流，数据主要从父节点传递到子节点（通过props）。如果顶层（父级）的某个props改变了，React会重渲染所有的子节点。

**10. React算法复杂度优化**

react树对比是按照层级去对比的， 他会给树编号0,1,2,3,4.... 然后相同的编号进行比较。所以复杂度是n，这个好理解。

关键是传统diff的复杂度是怎么算的？传统的diff需要出了上面的比较之外，还需要跨级比较。他会将两个树的节点，两两比较，这就有n^2的复杂度了。然后还需要编辑树，编辑的树可能发生在任何节点，需要对树进行再一次遍历操作，因此复杂度为n。加起来就是n^3了。

**11. React优点**

声明式, 组件化, 一次学习, 随处编写. 灵活, 丰富, 轻巧, 高效

## 移动端相关

**1. 移动端兼容适配**

- <meta name="viewport" content="width=device-width, initial-scale=1.0">

- rem, em, 百分比

- 框架的栅格布局

- media query媒体查询

- 手淘团队的一套flexible.js, 自动判断dpr进行整个布局视口的放缩

**2. flexible如何实现自动判断dpr**

判断机型, 找出样本机型去适配. 比如iphone以6为样本, 宽度375px, dpr是2

**3. 为什么以iPhone6为标准的设计稿的尺寸是以750px宽度来设计的呢？**

iPhone6的满屏宽度是375px，而iPhone6采用的视网膜屏的物理像素是满屏宽度的2倍，也就是dpr(设备像素比)为2, 并且设计师所用的PS设计软件分辨率和像素关系是1:1。所以为了做出的清晰的页面，设计师一般给出750px的设计图，我们再根据需求对元素的尺寸设计和压缩。

**4. 如何处理异形屏iphone X**

- `safe area`: 默认放置在安全区域以避免遮挡, 但会压缩
- 在meta中添加`viewport-fit=cover`: 告诉浏览器要讲整个页面渲染到浏览器中，不管设备是圆角与否，这个时候会造成页面的元素被圆角遮挡
- `padding: constant(env)`: 解决遮挡问题

**5. 移动端首屏优化**

- 采用服务器渲染ssr
- 按需加载配合webpack分块打包, 通过entry和commonChunkPlugin
- 很有必要将script标签➕异步
- 有轮播图 最好给个默认 另外要处理图片懒加载
- 打包线上也要注意去掉map 文件
- 组件, 路由懒加载
- webpack的一切配置 肯定是必须的
- 压缩图片 tinypng.com/
- 建议还是用webpack的图片压缩插件
- 骨架屏
- Loading页面

**6. PWA全称Progressive Web App，即渐进式WEB应用**

一个 PWA 应用首先是一个网页, 可以通过 Web 技术编写出一个网页应用. 随后添加上 App Manifest 和 Service Worker 来实现 PWA 的安装和离线等功能
解决了哪些问题？

- 可以添加至主屏幕，点击主屏幕图标可以实现启动动画以及隐藏地址栏
- 实现离线缓存功能，即使用户手机没有网络，依然可以使用一些离线功能
- 实现了消息推送
  它解决了上述提到的问题，这些特性将使得 Web 应用渐进式接近原生 App。

**7. 离线包方案**

现在 web 页面在移动端的地位越来越高，大部分主流 App 采用 native + webview 的 hybrid 模式，加载远程页面受限于网络，本地 webview 引擎，经常会出现渲染慢导致的白屏现象，体验很差，于是离线包方案应运而生。动态下载的离线包可以使得我们不需要走完整的 App 审核发布流程就完成了版本的更新

**8. 自适应和响应式布局的区别**

1. 自适应布局通过检测视口分辨率，来判断当前访问的设备是：pc端、平板、手机，从而请求服务层，返回不同的页面；响应式布局通过检测视口分辨率，针对不同客户端在客户端做代码处理，来展现不同的布局和内容。
2. 自适应布局需要开发多套界面，而响应式布局只需要开发一套界面就可以了。
3. 自适应对页面做的屏幕适配是在一定范围：比如pc端一般要大于1024像素，手机端要小于768像素。而响应式布局是一套页面全部适应。
4. 自适应布局如果屏幕太小会发生内容过于拥挤。而响应式布局正是为了解决这个问题而衍生出的概念，它可以自动识别屏幕宽度并做出相应调整的网页设计。

## 插件及工具相关

**1. babel和polyfill**

- `Babel`: Babel 是一个广泛使用的 ES6 转码器，可以将 ES6 代码转为 ES5 代码。注意：Babel 默认只转换新的 JavaScript 句法（syntax），而不转换新的 API
- `Polyfill`: Polyfill的准确意思为，用于实现浏览器并不支持的原生API的代码。

**2. jpg, jpeg和png区别**

- jpg是jpeg的缩写, 二者一致
- PNG就是为取代GIF而生的, 无损压缩, 占用内存多
- jpg牺牲图片质量, 有损, 占用内存小
- PNG格式可编辑。如图片中有字体等，可利用PS再做更改。JPG格式不可编辑

**3. git rebase和merge区别**

![1573610487386284.png](https://mmbiz.qpic.cn/mmbiz_png/eXCSRjyNYcauzxVGNtGRqt7BBG47iafrsut6GyOUhBePQ7279FufFsBpdbJ0icluicR4Wpy5thp0z8vmicQrrhWEhw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![1573610492758533.png](https://mmbiz.qpic.cn/mmbiz_png/eXCSRjyNYcauzxVGNtGRqt7BBG47iafrs2zKBIicr2fLiaQmcnl2pB6FYe6bYtpmWRu2lQ9LvSIJ5z16I63Ms9fwA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 前端性能优化

1. 减少HTTP请求（合并css、js，雪碧图/base64图片）
2. 压缩（css、js、图片皆可压缩,使用webpack uglify和 svg）
3. 样式表放头部，脚本放底部
4. 使用CDN（这部分，不少前端都不用考虑，负责发布的兄弟可能会负责搞好）
5. http缓存
6. bosify图片压缩: 根据具体情况修改图片后缀或格式 后端根据格式来判断存储原图还是缩略图
7. 懒加载, 预加载
8. 替代方案: 骨架屏, SSR
9. webpack优化

## 原生通信

**1.JSBridge通信原理, 有哪几种实现的方式？**

JsBridge给JavaScript提供了调用Native功能，Native也能够操控JavaScript。这样前端部分就可以方便使用地理位置、摄像头以及登录支付等Native能力啦。JSBridge构建 Native和非Native间消息通信的通道，而且是 双向通信的通道。

- JS 向 Native 发送消息 : 调用相关功能、通知 Native 当前 JS 的相关状态等。
- Native 向 JS 发送消息 : 回溯调用结果、消息推送、通知 JS 当前 Native 的状态等。

**2.实现一个简单的 JSBridge，设计思路？**

## 算法相关

**1. 二分查找和冒泡排序**

- 二分查找: 递归(分左右, 传递start,end参数)和非递归(使用while(l < h))
- 冒泡排序: 两个for循环

**2. 快速排序**



```
function quickSort (arr) {  if (arr.length < 2) return arr  var middle = Math.floor(arr.length / 2)  var flag = arr.splice(middle, 1)[0]  var left = [],        right = []  for (var i = 0; i < arr.length; i++) {    if (arr[i] < flag) {      left.push(arr[i])    } else {      right.push(arr[i])    }  }  return quickSort(left).concat([flag], quickSort(right))}
```

**3. 最长公共子串**



```
function findSubStr(str1, str2) {        if (str1.length > str2.length) {          [str1, str2] = [str2, str1]        }        var result = ''        var len = str1.length        for (var j = len; j > 0; j--) {          for (var i = 0; i < len - j; i++) {            result = str1.substr(i, j)            if (str2.includes(result)) return result          }        }      }      console.log(findSubStr('aabbcc11', 'ppooiiuubcc123'))
```

**4. 最长公共子序列(LCS动态规划)**

另一篇

```
// dp[i][j] 计算去最大长度，记住口诀：相等左上角加一，不等取上或左最大值function LCS(str1, str2){        var rows =  str1.split("")        rows.unshift("")        var cols =  str2.split("")        cols.unshift("")        var m = rows.length        var n = cols.length        var dp = []        for(var i = 0; i < m; i++){            dp[i] = []            for(var j = 0; j < n; j++){                if(i === 0 || j === 0){                    dp[i][j] = 0                    continue                }
                if(rows[i] === cols[j]){                    dp[i][j] = dp[i-1][j-1] + 1 //对角＋1                }else{                    dp[i][j] = Math.max( dp[i-1][j], dp[i][j-1]) //对左边，上边取最大                }            }            console.log(dp[i].join(""))//调试        }        return dp[i-1][j-1]    }//!!!如果它来自左上角加一，则是子序列，否则向左或上回退。//findValue过程，其实就是和 就是把T[i][j]的计算反过来。// 求最长子序列function findValue(input1,input2,n1,n2,T){    var i = n1-1,j=n2-1;    var result = [];//结果保存在数组中    console.log(i);    console.log(j);    while(i>0 && j>0){        if(input1[i] == input2[j]){            result.unshift(input1[i]);            i--;            j--;        }else{            //向左或向上回退            if(T[i-1][j]>T[i][j-1]){                //向上回退                i--;            }else{                //向左回退                j--;            }        }
    }
    console.log(result);}
```

**5. 数组去重，多种方法**

- 双for循环, splice剔除并i--回退
- indexOf等于index
- filter indexOf === index
- 新数组indexOf === index
- 使用空对象等

**6. 实现一个函数功能：sum(1,2,3,4..n)转化为 sum(1)(2)(3)(4)…(n)**



```
// 使用柯里化 + 递归function curry ( fn ) {  var c = (...arg) => (fn.length === arg.length) ?          fn (...arg) : (...arg1) => c(...arg, ...arg1)  return c}
```

**7. 反转二叉树**

- 

```
var invertTree = function (root) {  if (root !== null) {    [root.left, root.right] = [root.right, root.left]    invertTree(root.left)    invertTree(root.right)  }  return root}
```

**8. 贪心算法解决背包问题**

- 

```
var items = ['A','B','C','D']var values = [50,220,60,60]var weights = [5,20,10,12]var capacity = 32 //背包容积
greedy(values, weights, capacity) // 320
function greedy(values, weights, capacity) {        var result = 0        var rest = capacity        var sortArray = []        var num = 0        values.forEach((v, i) => {          sortArray.push({            value: v,            weight: weights[i],            ratio: v / weights[i]          })        })        sortArray.sort((a, b) => b.ratio - a.ratio)        sortArray.forEach((v, i) => {          num = parseInt(rest / v.weight)          rest -= num * v.weight          result += num * v.value        })        return result      }
```

**9. 输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。**

- 

```
function FindNumbersWithSum(array, sum){    var index = 0    for (var i = 0; i < array.length - 1 && array[i] < sum / 2; i++) {        for (var j = i + 1; j < array.length; j++) {            if (array[i] + array[j] === sum) return [array[i], array[j]]        }        //index = array.indexOf(sum - array[i], i + 1)       // if (index !== -1) {       //     return [array[i], array[index]]        //}    }    return []
```

**10. 二叉树各种(层序)遍历**

深度广度遍历



```
// 根据前序和中序重建二叉树/* function TreeNode(x) {    this.val = x;    this.left = null;    this.right = null;} */function reConstructBinaryTree(pre, vin){    var result = null    if (pre.length === 1) {        result = {            val: pre[0],            left: null,            right: null        }    } else if (pre.length > 1) {        var root = pre[0]        var vinRootIndex = vin.indexOf(root)        var vinLeft = vin.slice(0, vinRootIndex)        var vinRight = vin.slice(vinRootIndex + 1, vin.length)        pre.shift()        var preLeft = pre.slice(0, vinLeft.length)        var preRight = pre.slice(vinLeft.length, pre.length)        result = {            val: root,            left: reConstructBinaryTree(preLeft, vinLeft),            right: reConstructBinaryTree(preRight, vinRight)        }    }    return result}
// 递归// 前序遍历function prevTraverse (node) {  if (node === null) return;
  console.log(node.data);  prevTraverse(node.left);  prevTraverse(node.right);}
// 中序遍历function middleTraverse (node) {  if (node === null) return;
  middleTraverse(node.left);  console.log(node.data);  middleTraverse(node.right);}
// 后序遍历function lastTraverse (node) {  if (node === null) return;
  lastTraverse(node.left);  lastTraverse(node.right);  console.log(node.data);}
// 非递归// 前序遍历function preTraverse(tree) {        var arr = [],          node = null        arr.unshift(tree)        while (arr.length) {          node = arr.shift()          console.log(node.root)          if (node.right) arr.unshift(node.right)          if (node.left) arr.unshift(node.left)        }      }
// 中序遍历function middleTraverseUnRecursion (root) {  let arr = [],      node = root;
  while (arr.length !== 0 || node !== null) {    if (node === null) {      node = arr.shift();      console.log(node.data);      node = node.right;    } else {      arr.unshift(node);      node = node.left;    }  }
}
// 广度优先-层序遍历// 递归var result = []var stack = [tree]var count = 0var bfs = function () {  var node = stack[count]  if (node) {    result.push(node.value)    if (node.left) stack.push(node.left)    if (node.right) stack.push(node.right)    count++    bfs()  }}bfs()console.log(result)// 非递归function bfs (node) {  var result = []  var queue = []  queue.push(node)  while (queue.length) {    node = queue.shift()    result.push(node.value)    node.left && queue.push(node.left)    node.right && queue.push(node.right)  }  return result}
```

**11. 各种排序**

```
// 插入排序function insertSort(arr) {        var temp        for (var i = 1; i < arr.length; i++) {          temp = arr[i]          for (var j = i; j > 0 && temp < arr[j - 1]; j--) {            arr[j] = arr[j - 1]          }          arr[j] = temp        }        return arr      }      console.log(insertSort([3, 1, 8, 2, 5]))
// 归并排序function mergeSort(array) {        var result = array.slice(0)        function sort(array) {          var length = array.length          var mid = Math.floor(length * 0.5)          var left = array.slice(0, mid)          var right = array.slice(mid, length)          if (length === 1) return array          return merge(sort(left), sort(right))        }        function merge(left, right) {          var result = []
          while (left.length || right.length) {            if (left.length && right.length) {              if (left[0] < right[0]) {                result.push(left.shift())              } else {                result.push(right.shift())              }            } else if (left.length) {              result.push(left.shift())            } else {              result.push(right.shift())            }          }          return result        }        return sort(result)      }      console.log(mergeSort([5, 2, 8, 3, 6]))
// 二分插入排序function twoSort(array) {        var len = array.length,          i,          j,          tmp,          low,          high,          mid,          result        result = array.slice(0)        for (i = 1; i < len; i++) {          tmp = result[i]          low = 0          high = i - 1          while (low <= high) {            mid = parseInt((high + low) / 2, 10)            if (tmp < result[mid]) {              high = mid - 1            } else {              low = mid + 1            }          }          for (j = i - 1; j >= high + 1; j--) {            result[j + 1] = result[j]          }          result[j + 1] = tmp        }        return result      }      console.log(twoSort([4, 1, 7, 2, 5]))
```

**12. 使用尾递归对斐波那契优化**

递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。



```
// 传统递归斐波那契, 会造成超时或溢出function Fibonacci (n) {  if ( n <= 1 ) {return 1};
  return Fibonacci(n - 1) + Fibonacci(n - 2);}
Fibonacci(10) // 89Fibonacci(100) // 超时Fibonacci(500) // 超时
// 使用尾递归优化, 可规避风险function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {  if( n <= 1 ) {return ac2};
  return Fibonacci2 (n - 1, ac2, ac1 + ac2);}
Fibonacci2(100) // 573147844013817200000Fibonacci2(1000) // 7.0330367711422765e+208Fibonacci2(10000) // Infinity
```

**13. 两个升序数组合并为一个升序数组**



```
function sort (A, B) {  var i = 0, j = 0, p = 0, m = A.length, n = B.length, C = []  while (i < m || j < n) {    if (i < m && j < n) {      C[p++] = A[i] < B[j] ? A[i++] : B[j++]    } else if (i < m) {      C[p++] = A[i++]    } else {      C[p++] = B[j++]    }  }  return C}
```





