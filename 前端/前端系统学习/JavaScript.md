## JavaScript变量类型



## JavaScript函数

### 普通函数

```javascript
function say(speak) {
    console.log(`you speak ${speak}`)
    return;
}

say('hello world!')  // 输出内容：you speak hello world!
```



```javascript
function say(speak) {
    console.log(`you speak ${speak}`)
    console.log(arguments)
    return;
}

say('hello world!')
/*
输出内容：
you speak hello world!
{ "0": "hello world!" }
*/
```

在普通函数中，参数是可以以数组的形式存在的，传入函数中的参数都会被存到一个数组中，在使用的时候可以通过这个数组进行访问。



### 箭头函数和普通函数的参数数组对比（arguments vs args）

arguments：是旧时代的产物，类数组，仅存在于普通函数，不推荐新代码使用

args（rest参数）：ES6正式方案，真数组，语义清晰，箭头函数和普通函数都可用。

> **arguments**是ES5时代的“隐式参数集合”，剩余参数（...rest）是ES6引入的“标准参数集合”。
>
> 两者都可以拿到“多余参数”，但是本质、能力、使用场景完全不同。



### 参数

#### 1. 剩余参数





### module 1：函数基础形态

#### 1.1 函数声明 vs 函数表达式

##### 1. 函数声明

```javascript
// 写法：function 关键字 + 函数名
function sayHi() {
  console.log("你好");
}
```

**特点：**

- 会**提升（hoisting）** → 可以在声明**之前**调用
- 必须有函数名
- 全局 / 块级作用域都能用

```javascript
// 可以提前调用！不会报错
sayHi(); 

function sayHi() {
  console.log("你好");
}
```



##### 2. 函数表达式（Function Expression）

```javascript
// 把函数赋值给变量
const sayHi = function() {
  console.log("你好");
};
```

**特点：**

- **不会提升** → 必须先声明，后调用

- 可以是匿名函数（没有名字）

- 更灵活，常用于回调、立即执行函数



> **总结：**
>
> - **函数声明：** 可以提前调用，必须有名字；
> - 函数表达式：不可以提前调用，更加灵活



#### 1.2 形参 / 实参

##### 1. 形参

形参 <==> 形式上的参数

函数定义是写在括号里面的“占位符”，相当于变量。

```javascript
// a 和 b 就是 形参
function add(a, b) {
  return a + b;
}
```



##### 2. 实参

实参 <==> 实际传入的参数

调用函数时真正传进入的值。

```javascript
// 10 和 20 就是 实参
add(10, 20); 
```



> **总结：**
>
> - 形参：函数定义时用 ---> 占位
> - 实参：函数调用时用 ---> 真实数据/传值



#### 1.3 返回值 return

return 是函数向外输出结果的唯一通道。

##### 1. 基本用法

```javascript
function add(a, b) {
  // return 后面的值就是函数的返回值
  return a + b; 
}

// 调用后可以拿到结果
let sum = add(2, 3); 
console.log(sum); // 5
```



##### 2. 两个核心规则

1. **函数遇到return 会立刻停止执行**

```javascript
function test() {
  return 1;
  console.log("我不会执行"); // 永远不运行
}
```

**所以，return通常是放在函数的最底部或者逻辑停止处**

2. **没有return  ==> 默认返回undefined**

```javascript
function sayHi() {
  console.log("hi");
}

console.log(sayHi()); // undefined
```



> 总结：
>
> - return = 函数的输出口
> - 不写return ==> 输出undefined
> - 写了return ==> 输出return后面的值



#### 1.4 函数调用方式

##### 1. 直接调用（最常用）

直接写函数名 + 括号

```javascript
function fn() {}
fn(); 
```

##### 2. 作为对象方法调用

函数放在对象里面，叫方法

```javascript
const obj = {
  name: "小明",
  sayName: function() {
    console.log(this.name);
  }
};

obj.sayName(); // 小明
```

##### 3. 构造函数调用（new）

用来创建对象

```javascript
function Person(name) {
  this.name = name;
}

const p = new Person("小红"); 
```



##### 4. 通过call / apply 调用

手动指定函数内部的 `this` 指向

```javascript
function fn() {
  console.log(this);
}

fn.call({ name: "手动指定this" });
```



### module 2：函数的参数系统

#### 2.1 基本参数

最基础、最传统的参数写法，就是定义时写形参，调用时传实参。

**语法：**

```javascript
function 函数名(参数1, 参数2, ...) {
  // 函数体
}
```

**特点：**

- 行参数量和实参数量可以不匹配
- 少传：未传的参数值为undefined
- 多传：多余实参不会报错，但是会被忽略（可以通过arguments获取）

```javascript
function sum(a, b) {
  return a + b;
}

sum(1, 2);    // 正常：3
sum(1);       // 少传：1 + undefined = NaN
sum(1,2,3,4); // 多传：依然返回 3
```



#### 2.2 默认参数

当调用时没传参数/传了undefined，自动使用预设值。

```javascript
function 函数名(参数 = 默认值) { ... }
```

**示例：**

```javascript
function greet(name = "游客") {
  console.log(`你好，${name}`);
}

greet();        // 你好，游客
greet("张三");   // 你好，张三
greet(undefined); // 你好，游客
```

**注意：**

- 只有严格不传或传undefined才会触发默认值
- 传null、0、''不会触发默认值



#### 2.3 剩余参数...rest（ES6）

用于接收不确定个数的参数，打包成一个真数组。

**语法：**

```javascript
function 函数名(...参数名) { ... }
```

**示例：求和任意多个数字**

```javascript
function sum(...nums) {
  return nums.reduce((total, cur) => total + cur, 0);
}

sum(1);        // 1
sum(1, 2);     // 3
sum(1,2,3,4);  // 10
```

**特点：**

- `...rest` 必须是**最后一个形参**

- 返回的是**真正的数组**，可以直接用 `map/filter/reduce`

- 语义清晰，现代 JS 优先使用



#### 2.4 arguments（了解即可，ES5的，尽量使用ES6语法）

函数内部自带的**类数组对象**，会收集**所有传入的实参**。

**示例：**

```javascript
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

sum(1,2,3); // 6
```

**特点与缺点**

- 是**类数组**，不是真数组，不能直接用数组方法

- 箭头函数**没有 arguments**

- 代码可读性差，现在基本被 `...rest` 替代

与rest对比

- `arguments`：ES5、类数组、箭头函数无

- `...rest`：ES6、真数组、语义清晰、推荐使用  

> 总结：
>
> 1. **基本参数**：最基础，少传为 `undefined`
>
> 2. **默认参数**：`param = val`，没传时自动兜底
>
> 3. **剩余参数 `...rest`**：接收任意参数 → 真数组，现代首选
>
> 4. **arguments**：旧式类数组，收集所有实参，尽量不用





### module 3：函数的this指向

#### 3.1 this的四种绑定规则

##### 1. 默认绑定（最普通，优先级最低）

**规则：**函数独立调用，没有被任何对象、方法、new包裹 ==》 this = window（严格模式下是undefined）。

```javascript
function fn() {
  console.log(this); 
}

fn(); // 独立调用 → 默认绑定 → this = window
```

**严格模式下：**

```javascript
'use strict'
function fn() {
  console.log(this); // undefined
}
fn();
```

**没人管我 → 我指向 window /undefined**



##### 2. 隐式绑定（对象方法调用）

**规则：** 函数作为对象的方法调用 ==》 this指向前面的那个对象

**示例：**

```javascript
const obj = {
  name: "小明",
  sayHi: function() {
    console.log(this.name); 
  }
};

obj.sayHi(); // obj. 调用 → this = obj → 输出：小明
```

**重点：隐式丢失（面试常考）**

把对象方法赋值给变量再调用，会**丢失 this**，变回默认绑定！

```javascript
const fn = obj.sayHi;
fn(); // 独立调用 → this = window → 输出 undefined
```

> 谁点我，我指向谁



##### 3. 显示绑定（call/apply/bind）（手动指定，优先级更高）

**规则：** 用call()、apply()、bind()**强制指定this指向** ---> 优先级高于隐式绑定。

三个方法区别（超级好记）

- call：立即执行，参数逐个传递
- apply：立即执行，参数数组传递
- bind：不执行，返回新韩淑，永久绑定this

```javascript
function fn() {
  console.log(this.name);  // 在这个情况下，函数的this是默认绑定，=window
}

const obj = { name: "小红" };

fn.call(obj);   // 小红
fn.apply(obj);  // 小红

const newFn = fn.bind(obj);
newFn();        // 小红
```

你让我指向谁，我就指向谁，强制生效





##### 4. new绑定

**规则：** 使用new函数()创建实例 ---> this指向新创建的对象。

**示例：**

```javascript
function Person(name) {
  this.name = name; // this 指向新对象
}

const p = new Person("小刚");
console.log(p.name); // 小刚
```

new 我 → 我指向新对象



#### 3.2 箭头函数的this

**箭头函数没有自己的 this！它的 this = 外层作用域的 this（继承而来）**

**特点**

1. **不遵循四条绑定规则**
2. **不能当构造函数（不能 new）**
3. **没有 arguments**
4. **this 一旦确定，永远不变，call/apply/bind 都改不了**

```javascript
const obj = {
  name: "箭头函数",
  fn: function() {
    // 普通函数：this = obj
    
    setTimeout(() => {
      // 箭头函数：继承外层 fn 的 this → 指向 obj
      console.log(this.name); 
    }, 1000);
  }
};

obj.fn(); // 输出：箭头函数
```

**对比普通函数（this会丢）**

```javascript
setTimeout(function() {
  console.log(this); // 独立调用 → window
}, 1000);
```

**箭头函数没有 this，继承外层的，永远不变**



> 总结：
>
> 1. **默认绑定**：独立调用 → `window/undefined`
>
> 2. **隐式绑定**：对象调用 → `指向调用对象`
>
> 3. **显示绑定**：`call/apply/bind` → `强制指向`
>
> 4. **new 绑定**：`new` → `指向新对象`
>
> 5. **优先级**：`new > 显示 > 隐式 > 默认`
>
> 6. **箭头函数**：`无this，继承外层，不可修改`

### module 4：函数的高级用法

#### 4.1 函数作为参数（回调函数）

**核心概念：**

把一个函数当作参数，传递给另一个函数，在合适的时机被调用 —— 这个传进去的函数就叫做回调函数（callback）。

**一句话记忆：** 我现在不执行，你晚点帮我执行。

```javascript
// 定时器：把匿名函数当作参数传给 setTimeout
setTimeout(function() {
  console.log("1秒后执行"); // 这就是回调函数
}, 1000);
```



```javascript
// 主函数：接收一个函数当参数
function doSomething(callback) {
  console.log("先做事");
  callback(); // 调用传进来的函数
}

// 定义要传进去的函数
function myCallback() {
  console.log("我是回调，被调用了");
}

// 把函数当作参数传进去
doSomething(myCallback);
```



```javascript
doSomething(() => {
  console.log("箭头回调！");
});
```

**实战场景**

- 定时器 `setTimeout/setInterval`

- 异步请求 `ajax/fetch/axios`

- 数组方法 `forEach/map/filter`

- 事件监听 `btn.onclick = function(){}`



#### 4.2 函数作为返回值（高阶函数）

**概念：** 满足下面任意一个就是高阶函数：

1. 接收**函数作为参数**
2. 返回一个新函数

一句话记忆：**函数生产函数 —— 工厂模式。**

**示例：函数返回函数**

```javascript
// 高阶函数：返回一个新函数
function createGreet(greeting) {
  // 返回一个新函数
  return function(name) {
    console.log(greeting + ", " + name);
  };
}

// 调用高阶函数，得到一个新函数
const sayHi = createGreet("你好");
const sayHello = createGreet("Hello");

// 使用新函数
sayHi("小明"); // 你好, 小明
sayHello("小红"); // Hello, 小红
```

**为什么要用？**

- **复用逻辑**：批量生成功能相似的函数
- **保留参数**：把 “提前传好的值” 存起来（这就是闭包的雏形）



#### 4.3 闭包（重点）

**函数能够记住并访问它所在的词法作用域，即使函数在当前作用域以外执行。**

大白话：**函数 “带走” 了自己出生时的环境，走到哪都能访问里面的变量**。

**经典闭包代码：**

```javascript
function outer() {
  // 外部函数的变量
  let count = 0;

  // 内部函数（形成闭包）
  return function inner() {
    count++; // 访问外部函数的变量
    console.log("count：" + count);
  };
}

// 接收返回的 inner 函数
const fn = outer();

fn(); // 1
fn(); // 2
fn(); // 3
```

**闭包的三个条件**

1. 函数**嵌套**（函数里写函数）
2. 内部函数**引用**外部函数的变量 / 参数
3. 内部函数在外部函数**以外被执行**

**闭包能干什么？**

1. **私有化变量**（不让外部随便改）
2. **持久化保存数据**（变量不会被销毁）
3. **工厂函数批量生成逻辑**

**实战：闭包实现私有计数器**

```javascript
function createCounter() {
  let num = 0; // 私有变量，外部访问不到

  return {
    add: () => ++num,
    sub: () => --num,
    get: () => num
  };
}

const counter = createCounter();
counter.add(); // 1
counter.add(); // 2
console.log(counter.get()); // 2
```

注意：闭包可能造成内存泄漏

- 闭包会保留外部作用域，变量**不会自动销毁**
- 不用时记得**手动释放**：`fn = null`

> 总结：
>
> 1. **回调函数**：函数当参数 → 晚点执行
>
> 2. **高阶函数**：函数当参数 / 函数当返回值
>
> 3. **闭包**：内部函数带走外部作用域，**记住变量、私有化、持久化**

 **注意：** 闭包造成内存泄露这个问题，在现代浏览器中，普通闭包：不用管，浏览器自动挥手。大数据闭包：



