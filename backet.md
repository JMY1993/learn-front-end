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
数字 0、空字符串 ""、null、undefined 和 NaN 都会被转换成 false。因为他们被称为 “falsy” 值