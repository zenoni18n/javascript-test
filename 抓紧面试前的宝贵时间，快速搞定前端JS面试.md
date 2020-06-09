# 抓紧面试前的宝贵时间，快速搞定前端JS面试

## 基础

1. 前端两个标准 W3C标准 html css dom bom 等应用标准和ECMA262标准 es6 原型闭包等语法标准

2. JS的数据类型有几种？7种。**undefined、Function、Number、String、Symbol(es6新增)、Boolean、objectd** 
   
   ![image-20200523140357844](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523140357844.png)
   
   ![image-20200523140521863](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523140521863.png)

   Object 中包含了哪几种类型？
   其中包含了Data、function、Array等。这三种是常规用的。
   注意：null高程中明确说了是基本数据类型，虽然用 typeof返回的是Object ，但instanceof Object 结果是false
   typeof 能返回的去掉null加上function 也是7种
   
   **值类型和引用类型的区别**
   
   https://blog.csdn.net/linxiaoduo/article/details/80117628
   
3. 3.使用const定义常量，而且定义一次以后不能再进行更改，否者会报错。但是要注意， 如果给常量定义的是对象，只要该对象指向在内存中的地址不发生改变， 数据可以随便改的（这涉及到计算机的传值和传址）；

4. 列举三种强制类型转换和两种隐式类型转换？
   parseInt(),parseFloat(),Number()
   ==,!!
   !!唯一的作用就是把值转化为布尔值，其实对代码逻辑来说没什用，即使不用不会有任何问题，只是看起来统一了数据类型为布尔值，建议不用。实际上Boolean(a)和!!a效果一样

5. 什么时候用===，什么时候用==？

   ```
   // 什么情况下用 == 或者 ===
   // 除了 == null 之外，其他的一律用 ===
   const obj = {x: 20};
   if (obj.x == null) {}
   // 相当于：
   // if(obj.x === null || obj.x === undefined) {}
   ```

6. if语句和逻辑计算

   ![image-20200516171608235](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200516171608235.png)

7. 引用类型两道题

   **typeof只能识别不了array，object，null ，都是object,obj1和obj2地址一样，所以改哪个都影响x1只是单纯的赋值，和地址没关系**
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

8. 手写深拷贝

   ```
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
**实例对象的隐式原型指向其构造函数的显式原型，Function的隐式原型指向自身的显式原型-----北梦**

> **instanceof** 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。

Student.prototype instanceof People  //true
People.prototype instanceof Object    //true

4. **prototype**：
   	(1).简单的说它就是一个属性且属性的值为地址，指向一个对象。
      	而它通常为函数的属性。
      	(2).它主要用来共享属性和方法（常用于构造函数创建实例对象给实例对象共享属性）
      	(3).主要用来被访问
      	(4).判断**自有属性和原型属性** 一般用hasOwnProperty()
   **proto**：
   	(1).基本所有的对象有该属性
   	(2).用来构建原型链访问构造方法中的显示原型

5. **hasOwnProperty**表示是否有自己的属性。这个方法会查找一个对象实例是否有某个属性，**但是不会去查找它的原型链**。

xialuo.hasOwnProperty('eat')  //false    xialuo.hasOwnProperty('hasOwnProperty')  //false    在object上

```
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

typeof Function.prototype.__proto__) === "object";//true

Function.prototype.__proto__=== Object.prototype;//true
```



## 作用域和闭包

### 作用域和自由变量

- 自由变量：在全局中定义了一个变量a，然后在函数中使用了这个a，这个a就可以称之为自由变量，可以这样理解，**凡是跨了自己的作用域的变量都叫自由变量**。一个自由变量在当前作用域没有定义，但被使用了，则向上级作用域一层一层依次寻找，直至找到为止，如果到全局作用域都没找到，则报错 xx is not defined。
  注意：所有的自由变量的查找，是在函数定义的地方function（）向上级作用域查找，不是在执行的地方fn（）！

  ```
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

### 闭包

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

```
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

相同：

(1)都是用来改变函数的this对象的指向的。
(2)第一个参数都是this要指向的对象。
(3)都可以利用后续参数传参。

不同：

call和apply都是对函数的直接调用，而bind方法返回的仍然是一个函数，因此后面还需要()来进行调用才可以

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
console.log( c.get('a') )
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

1. 拆解url

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

```
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
    })
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
   事件委托又叫事件代理

3. JavaScript中matches(matchesSelector)：
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

1. HTTP状态码
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

2. 什么是同源策略？
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

5. 跨域访问

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

6. ajax的常用插件
   (1)jquery
   (2)fetch
   (3)axios 基于XMLHttpRequest，多用于三大框架

## 存储

1. cookie的特点
   (1).本身用于浏览器和server通讯
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

mv 文件名+tab补全  ../../ 移动文件

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

1. 一般来说，页面资源分为3类：html代码、媒体文件，如图片、视频等、
   javascript、css
2. 页面加载基础知识解释：
   (1).DNS英文全称:Domain Name Service 中文全称:域名解析服务: 域名–>IP地址
   为什么要域名？仅仅是因为 IP 不好记吗？
   不是的，同一个域名对应的IP在不同的区域是不一样的，因为大型网站做分区域的IP均衡代理。你在北京访问百度和你在深圳访问百度的域名是一样的都是baidu.com，但是DNS解析出来的IP是不一样的，如果你在北京直接访问深圳的IP就会慢很多
   在浏览器访问网站的时候，实际还是访问 IP，域名解析服务（DNS）会根据地域去解析成不同的 IP 让你的网站访问的更快
   综上所述，使用域名不仅仅是因为容易记住，而且是网页访问更快，因为DNS会解析出来的 IP 距离你很近。
   (2).浏览器根据 IP 地址向服务器发起http请求
   浏览器只是发起方，真正的核心模块还是操作系统，操作系统里有一些可以发起网络请求的服务，调用操作系统的服务，然后操作系统去发起http请求。这里面还有建立连接的三次握手过程.
   (3).服务器处理http请求，并返回给浏览器
   至于服务器怎么处理http请求这里不讲，返回的页面资源就可能有html代码，媒体文件，如图片、视频等，javascript、css等等。

### 渲染过程

1. 请求的是页面返回html代码
   (1).根据HTML代码生成DOM Tree（文本代码生成树结构）
   (2).根据CSS生成CSSOM
   (3).将DOM Tree和CSSOM整合形成Render Tree（渲染树）
   只根据DOM Tree是无法渲染的，其标签的CSS属性是在CSSOM Tree里面的，可以将Render Tree理解为挂着CSS属性的DOM Tree
   **渲染过程（二）**
   **(1).根据Render Tree渲染页面**
   **(2).遇到script则暂停渲染，优先加载并执行JS代码，完成再继续**
   **(3).直到把Render Tree渲染完成**

2. **JS操作和渲染页面操作是共用一个线程的，因为JS可能改变DOM结构而改变Render Tree的结构，所以遇到script就暂停渲染，否则渲染了可能没用，Render Tree变了(js要放最后)**

3. **Reflow（回流）：浏览器要花时间去渲染，当它发现了某个部分发生了变化影响了布局，那就需要倒回去重新渲染。**
   **Repaint（重绘）：如果只是改变了某个元素的背景颜色，文字颜色等，不影响元素周围或内部布局的属性，将只会引起浏览器的repaint，重画某一部分。**
   **Reflow要比Repaint更花费时间，也就更影响性能。所以在写代码的时候，要尽量避免过多的Reflow**

4. **window.onload和DOMContentLoaded**

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



## 面试

![image-20200523142052639](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523142052639.png)

![image-20200523142111543](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523142111543.png)

如：小程序方面不是很熟

## 题目讲解

### 常见的语法糖

指计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用

https://www.jianshu.com/p/bc2e172cb322



### var和let const的区别

![image-20200523142336481](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200523142336481.png)

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

### 闭包式什么？什么特性？什么影响？

![image-20200524004648434](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524004648434.png)

![image-20200524004701671](C:\Users\Zenon\AppData\Roaming\Typora\typora-user-images\image-20200524004701671.png)

```
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





