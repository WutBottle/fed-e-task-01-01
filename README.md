### 1. 请说出下列执行的最终结果并解释为什么？
```js
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i)
  }
} 
a[6]()
```
var会导致变量提升为全局变量，循环体结束时i=10，所以结果打印为 10

### 2. 请说出下列执行的最终结果并解释为什么？
```js
var tmp = 123;

if (true) {
  console.log(tmp)
  let tmp;
}
```
 Cannot access 'tmp' before initialization at <anonymous>:4:15
 因为let使得if内部出现暂时性死区不受外部var影响，而if内部的tmp还未被声明，
 所以出现Uncaught ReferenceError

### 3. 结合 ES6 新语法，用最简单的方法找出数组中的最小值
```js
var arr = [12, 34, 32, 89, 4]
```
```js
// 方法一
Math.min.apply(null,array);

// 方法二
array.sort()[0];

// 方法三
let temp = array[0]; 
const min = array.map(item => {
  if (temp > item)
    temp = item;
})

// 方法四
Math.min(...arr);
```

### 4. 请说明 var，let，const 三种声明变量的方式之间的具体差别？
1. var 定义的变量作用域为函数作用域，let 和 const 的作用域为块级作用域
2. var 存在变量提升，let 和 const 无变量提升，var 提升为 undefined，let 和 const 报错
3. var 可以重复声明同名变量，let 和 const 不行，会报错
4. const 声明常量，意思是声明后指向的内存地址不再改变，其内部属性和方法依然可以变化、增减

### 5. 请说出下列执行的最终结果并解释为什么？
```js
var a = 10;
var obj = {
  a: 20,
  fn(){
    setTimeout(() => {
      console.log(this.a)
    })
  }
}
obj.fn()
```
因为当前this指针指向的是obj的执行上下文里，并且setTimeOut用的是箭头
函数，所以指针指向obj，那么this.a就是20，所以输出20. 

### 6. 简述 Symbol 类型的用途？
在ES5中，对象属性名都是字符串容易造成属性名冲突。为了避免这种情况的发生，ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。

#### 应用1：使用Symbol来作为对象属性名(key)
```js
const PROP_NAME = Symbol()
const PROP_AGE = Symbol()
let obj = {
  [PROP_NAME]: "拉钩教育"
}

obj[PROP_NAME] // '拉钩教育'
```
Symbol类型的key是不能通过Object.keys()或者for...in来枚举的，它未被包含在对象自身的属性名集合(property names)之中。
当使用JSON.stringify()将对象转换成JSON字符串的时候，Symbol属性也会被排除在输出内容之外。
专门获取以Symbol方式定义的对象属性的Symbol API：
// 使用Object的API
Object.getOwnPropertySymbols(obj) // [Symbol(name)]
// 使用新增的反射API
Reflect.ownKeys(obj) // [Symbol(name), 'age', 'title']
#### 应用2：使用 Symbol 来替代常量
如下，保证常量唯一
```js
const TYPE_AUDIO = 'AUDIO'
const TYPE_VIDEO = 'VIDEO'
const TYPE_IMAGE = 'IMAGE'
```

#### 应用3：使用 Symbol 定义类的私有属性/方法
如下，将密码作为私有属性，只有约定好的方法可以验证
```js
const WEIGHT = Symbol()

class FeMale {
  constructor(height, wight) {
    this.height = height
    this[WEIGHT] = wight
  }

  checkWeight(w) {
      return this[WEIGHT] === w
  }
}

export default FeMale
```

### 7. 说说什么是深拷贝，什么是浅拷贝？
对象类型在赋值的过程中其实是复制了地址，从而会导致改变了一方其他也都被改变的情况。使用浅拷贝来解决这个情况。所谓浅拷贝其实是拷贝了所有属性值到新的对象，如果属性还是对象的话，拷贝的依然是该属性的地址。这时候就要用深拷贝来处理了。也就是说，深拷贝的情况下，即使属性是对象，也不会影响被拷贝的那个对象了。

浅拷贝的方式有如下：
Object.assign、展开运算符 ...

深拷贝通常可以通过 JSON.parse(JSON.stringify(object)) 来解决

但是该方法也是有局限性的：
- 会忽略 undefined
- 会忽略 Symbol
- 不能序列化函数
- 不能解决循环引用的对象

深拷贝需要考虑很多边界情况，可以参考 lodash 的深拷贝方法

### 8. 谈谈你是如何理解 JS 异步编程的，Event Loop 是做什么的，什么是宏任务，什么是微任务？
JS 异步编程简单点说是为了解决同步代码阻止塞进程的一种编程方式。

Event Loop是一个 js 的事件执行模型，node 和 浏览器还不太一样。。。

宏任务和微任务都是一些在 Event Loop 中的异步任务

宏任务包括 setTimeout、setInterval等等

微任务包括 process.nextTick、Promise、MutationObserver等等

一般执行流程是：
1. 首先执行同步任务；
2. 微任务队列中所有的任务都会被依次取出来执行，直到清空；
3. 宏任务一次只从消息队列中取一个任务执行，执行完后就去执行微任务队列中的任务。

### 9. 将下面异步代码使用 Promise 改进
```js
setTimeout(function(){
  var a = "hello";
  setTimeout(function(){
    var b = "lagou";
    setTimeout(function(){
      var a = "I 💗 U";
      console.log(a + b + c);
    },10)
  },10)
},10)
```
```js
Promise.resolve('hello')
  .then(res => res + 'lagou')
  .then(res => {
    setTimeout(() => {
      console.log(res + 'I 💗 U')
    }, 520)
  })
```

### 10. 请简述 TypeScript 和 JavaScript 的关系？
TypeScript 是 Javascript 的超集，实现以面向对象编程的方式使用 Javascript。最后要编译成 Javascript 才能运行。

TypeScript 在代码层面对 Javascript 的动态性和弱类型进行了强化。

对于学过c++这类面向对象的强类型编程语言，会更容易理解TypeScript的设计思想

### 11. 请谈谈你所认为 TypeScript 的优缺点？
#### 优点
1. TypeScript 增加了代码的可读性和可维护性
2. TypeScript 非常包容
3. TypeScript 拥有活跃的社区
#### 缺点
1. 有一定的学习成本，需要理解接口（Interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）等前端工程师可能不是很熟悉的概念
2. 短期可能会增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期维护的项目，TypeScript 能够减少其维护成本
3. 集成到构建流程需要一些工作量
4. 可能和一些库结合的不是很完美


