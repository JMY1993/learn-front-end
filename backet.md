# null undefined 区别
null
特殊的 null 值不属于上述任何一种类型, 它构成了一个独立的类型，只包含 null 值

undefined
特殊值 undefined 和 null 一样自成类型。undefined 的含义是 未被赋值。如果一个变量已被声明，但未被赋值，那么它的值就是 undefined
# 如果想要获得一些关于浏览器和其他引擎的兼容性信息，请看：

- http://caniuse.com —— 每个功能都列有一个支持信息表格
例如想看哪个引擎支持现代加密（cryptography）函数：http://caniuse.com/#feat=cryptography。
- https://kangax.github.io/compat-table —— 一份列有语言功能以及引擎是否支持这些功能的表格。

# typeof操作符
typeof xx
typeof(xx)

1. typeof null 的结果是 "object"。这其实是不对的。官方也承认了这是 typeof 运算符的问题，现在只是为了兼容性而保留了下来。当然，null 不是一个 object。null 有自己的类型，它是一个特殊值。再次强调，这是 JavaScript 语言的一个错误。
2. typeof alert 的结果是 "function"，因为 alert 在 JavaScript 语言中是一个函数。我们会在下一章学习函数，那时我们会了解到，在 JavaScript 语言中没有一个特别的 “function” 类型。函数隶属于 object 类型。但是 typeof 会对函数区分对待。
这不是很正确的做法，但在实际编程中非常方便。

# 类型转换
- String()
- Number()

undefined: NaN //!!!
null: 0
true or false: 1 or 0
string: 去掉首尾空格后的纯数字字符串中含有的数字。*如果剩余字符串为空，则转换结果为 0*。否则，将会从剩余字符串中“读取”数字。当类型转换出现 error 时返回 NaN。

```js
alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN（从字符串“读取”数字，读到 "z" 时出现错误）
alert( Number("1 03") );      // NaN
alert( Number(true) );        // 1
alert( Number(false) );       // 0
```
- Boolean()
0, null, undefined, NaN, ""	=> false
其它都是true
```js
console.log(Boolean("0")," ",Boolean(" ")) //true true !!!
//因为“ ”和“0”都是非空字符串
```
# 运算符和运算元
- 二元运算符 “+” 和其它
  + 二元“+” 可以做加法，也可以连接字符串
    ```js
    1+1;   //2
    1+"1"; //"11"
    "1"+1; //"11" !!!字符串+数字会把数字转换为字符串，然后进行字符串连接
    1+1+"1"//"21" !!!js是从左到右运算的，所以会先计算 `1+1 //2`，然后计算`2+"1"//"21"`
    ```
  + 二元“-”及其它二元运算符会把非数字运算符转化为数字
    ```js
    2-"1"; //1
    ```

  + 一元运算符“+”及数字转化
    相当于调用了`Number()`
    ```js
    +"21"; //21
    //要计算“21“+”21“的算数值
    +"21" + +"21"; // 42
    ```
- 幂运算
  **
  ```js
  2**3; //8
  ```

- 几个例子
```js
"" + 1 + 0;     //"10"
"" - 1 + 0;     //-1
true + false;   //1
6 / "3";        //2
"2" * "3";      //6
4 + 5 + "px";   //"9px"
"$" + 4 + 5;    //"$45"
"4" - 2;        //2
"4px" - 2;      //NaN
7 / 0;          //Infinity
"  -9  " + 5;   //" -9 5"
"  -9  " - 5;   //-14
null + 1;       //1
undefined + 1;  //NaN
" \t \n" - 2;   //NaN !!!字符串转换为数字时，会忽略字符串的首尾处的空格字符。在这里，整个字符串由空格字符组成，包括 \t、\n 以及它们之间的“常规”空格。因此，类似于空字符串，所以会变为 0
```
# 值的比较
## 字符串比较
在比较字符串的大小时，JavaScript 会使用Unicode 编码顺序进行判定。
因此小写字母永远>大写字母。

- 首先比较两个字符串的首位字符大小。
- 如果一方字符较大（或较小），则该字符串大于（或小于）另一个字符串。算法结束。
- 否则，如果两个字符串的首位字符相等，则继续取出两个字符串各自的后一位字符进行比较。
- 重复上述步骤进行比较，直到比较完成某字符串的所有字符为止。
- 如果两个字符串的字符同时用完，那么则判定它们相等，否则未结束（还有未比较的字符）的字符串更大。

## 当对*不同类型*的值进行比较时，JavaScript 会首先将其转化为数字（number）再判定大小
相当于运行了Number()

## 对null和undefined比较
- null == undefined
  true
- null === undefined
  false
- 与0的比较
  ```js
  alert( null > 0 );  // false
  alert( null == 0 ); // false
  alert( null >= 0 ); // true

  //因为相等性检查 == 和普通比较符 > < >= <= 的代码逻辑是相互独立的。进行值的比较时，null 会被转化为数字，因此它被转化为了 0。这就是为什么（3）中 null >= 0 返回值是 true，（1）中 null > 0 返回值是 false。
  //另一方面，undefined 和 null 在相等性检查 == 中不会进行任何的类型转换，它们有自己独立的比较规则，所以除了它们之间互等外，不会等于任何其他的值。这就解释了为什么（2）中 null == 0 会返回 false。

  //undefined 不应该被与其他值进行比较：
  alert( undefined > 0 ); // false，undefined被转换成了NaN
  alert( undefined < 0 ); // false，同上
  alert( undefined == 0 ); // false: undefined==null，只与null非全等
  ```
- 规避值的比较可能产生的错误
  + 除了严格相等 === 外，其他凡是有 undefined/null 参与的比较，我们都需要额外小心。
  + 除非你非常清楚自己在做什么，否则永远不要使用 >= > < <= 去比较一个可能为 null/undefined 的变量。
  + 对于取值可能是 null/undefined 的变量，请按需要*分别检查它的取值情况*。

# if (…) 语句会计算圆括号内的表达式，并将计算结果转换为布尔型。
相当于对括号内返回值执行`Boolean()`
数字 0、空字符串 ""、null、undefined 和 NaN 都会被转换成 false。因为他们被称为 “falsy” 值。

# 逻辑运算符
|| ：返回为真值的操作数
&& ：返回为假值的操作数
!  ：布尔非运算

- 短路计算
  - `a || b || c || d`
  返回第一个真值，如果都是假，返回最后一个值
  - `a && b && c && d`
  返回第一个假值，如果都是真，返回最后一个值

- 代替if
  - `(predicate) || exp`;
  predicate为假，执行exp；
  - `(predicate) && exp`;
  predicate为真，执行exp；

- 转换Boolean值
  `!!value` 相当于 `Boolean(value)`

  !!! 这里一起记忆一下`+value`转换为数字的用法

- 几个例子
  ```js
  console.log(console.log(1) || 2 || console.log(3)) //控制台先打印出1，然后是2
  //console.log()返回值是undefined，是一个falsy值，所以会进一步向后取之，取到2后停止
  ```
    [使用prompt的登陆验证](https://zh.javascript.info/task/check-login)
  ```js
  //使用prompt进行登陆校验
  'use strict'
   function loginCheck(){
    let userName = prompt("Enter your user name please:");
    if (!userName){
        alert("Canceled!");
        return 0;
    } else if (!(userName === "Admin")) {
        alert("I don't know you");
        return 0;
    }

    let passwd = prompt("Enter your password please:");
    
    if (!passwd){
        alert("Canceled!");
        return 0;
    } else if (!(passwd == "TheMaster")) {
        alert("Wrong password");
        return 0;
    }

    alert("Welcome!");
   return 0;
   }

# 控制结构
- break
- continue
## label: break或continue后面如果接标签，标签必须在语句之上定义。
## switch: 比较switch和case的value是否*严格相等*
  + 注意`defaut`的使用
  + 注意每个case的`break`，没有break就会继续执行下一个case

- 一个例子
[输出素数（prime）](https://zh.javascript.info/task/list-primes)
```js
function isPrime(n) {
    for (let divider = 2 ; divider < n ; divider++) {
        if (n%divider === 0) {
            return false;
        }
    }
    return true;
}

function listPrime(n) {
    for (let prime = 2 ; prime <= n ; prime++) {
        if (isPrime(prime)) {
            console.log(`Found ${prime}!`);
        }
    }
    return 0;
}
```

# 函数与函数表达式
## 语法区别
  函数的花括号后面不需要`;`
  函数表达式的花括号后面需要`;`
## 可见性区别
  *严格模式下*，当一个函数声明在一个代码块内时，它在该代码块内的任何位置都是可见的。但在代码块外不可见。
  如果把函数表达式赋值给一个在代码块外声明的变量时，可以在代码块外调用

  注意这里说的代码块是花括号的范围，不是*作用域*的概念。

  例子：
  ```js
  let age = prompt("What is your age?", 18);

  // 有条件地声明一个函数
  if (age < 18) {

    function welcome() {
      alert("Hello!");
    }

  } else {

    function welcome() {
      alert("Greetings!");
    }

  }

  // ……稍后使用
  welcome(); // Error: welcome is not defined
  ```
  而
  ```js
  let age = 16; // 拿 16 作为例子

  if (age < 18) {
    welcome();               // \   (运行)
                            //  |
    function welcome() {     //  |
      alert("Hello!");       //  |  函数声明在声明它的代码块内任意位置都可用
    }                        //  |
                            //  |
    welcome();               // /   (运行)

  } else {

    function welcome() {
      alert("Greetings!");
    }
  }

  // 在这里，我们在花括号外部调用函数，我们看不到它们内部的函数声明。


  welcome(); // Error: welcome is not defined
  ```

# 对象的键
## `[key]` 的形式用来表示中间有空格的键名和变量名键名
## 计算属性->对象字面量中使用方括号
  ```js
  let fruit = prompt("Which fruit to buy?", "apple");

  let bag = {
    [fruit]: 5, // 属性名是从 fruit 变量中得到的
  };

  alert( bag.apple ); // 5 如果 fruit="apple"
  ```
## 属性值的简写
  ```js
  function makeUser(name, age) {
    return {
      name, // 与 name: name 相同
      age,  // 与 age: age 相同
      // ...
    };
  }
  //也可以与正常键值对的方式混用
  let user = {
    name,  // 与 name:name 相同
    age: 30
  };  
  ```
## in操作符左边是属性名（通常是一个带引号的字符串）
## 检测对象是否含有某个字符串
- `obj.key === undefined`
- `key in obj`

第一种方法存在检测不到的情况，如下例：
```js
let obj = {
  test: undefined
};

alert( obj.test ); // 显示 undefined，所以属性不存在？

alert( "test" in obj ); // true，属性存在！
```
# for...in 循环，*整数属性*会被进行排序，其他属性则按照创建的顺序显示
这里的“整数属性”指的是一个可以在不作任何更改的情况下转换为整数的字符串（包括整数到整数）。
也就是说，对于key，有Number(key) = key = String( Number(key) )。
比如，如果有obj["+49"]，key是"+49"。Number(key)=49，加号没有了，不是整数属性。
比如，如果有obj["1.2"]，1.2不是整数。

# 对象的相等判断，全等===和非全等==没有区别。只有在两个变量引用指向*同一个对象*时，才相等。
# 对象的浅拷贝Object.assign(obj, [src1, src1, src1, ...])，所有的src的属性都拷贝给obj，键名重复的，后面的会覆盖前面的
# 对象深拷贝，lodash库，`_.cloneDeep(obj)`
## 深拷贝算法
# 检查空对象
[检查空对象](https://zh.javascript.info/task/is-empty)
```js
function isEmpty(obj) {
  for (let key in obj){
    return false;
  }
  return true;
}

# JS垃圾回收（GC）的原理 （描述其原理）
从根开始遍历所有根变量的引用再标记这些被引用的变量，再遍历这些标记的变量，直到所有可达（reachable）的引用都被标记。
标记完成后，删除所有未被标记的变量。