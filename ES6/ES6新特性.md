# let&&const
- ES5:  
  1.只有全局作用域变量和函数作用域变量,没有块级作用域的概念   
  2.“变量提升”（当程序进入一个新的函数时，会将该函数中所有的变量的声明放在函数开始的位置。仅仅会提升变量的声明，不会提升变量的赋值）  
## 1、let定义块级作用域变量
1. 没有变量的提升，必须先声明后使用  
2. let声明的变量，不能与前面的let，var，conset声明的变量重名
```js
// 用var声明的变量会影响各个块里的i
var arr = [];
for(var i = 0; i < 10; i++) {
    arr[i] = function() {
        console.log(i);
    }
} 
arr[8]();//结果:10

// 用let声明的变量为块级作用域，各个块之间互不影响
var arr = [];
for(let i = 0; i < 10; i++) {
    arr[i] = function() {
        console.log(i);
    }
} 
arr[8]();//结果:8
```
```js
var a = 1;
(function() {
    // 函数中声明a时变量提升,但还没有赋值
    console.log(a);
    var a = 2;
})();//结果:undefined

var a = 1;
(function() {
    // a变量不会被提升
    console.log(a);
    let a = 2;
})();//结果:报错a未定义
```
```js
{
    var a = 1;
    let a = 2;
} //报错,a已经用var声明过了

function say(word) {
    let word = 'hello';//报错,用let重新声明word参数
    console.log(word);
}
say('hi');
```
## 2、const定义只读变量(常量特性)
1. 常量特点:不可修改  
```js
const Name = '张三';
Name = '李四';//报错,企图修改常量Name

// 如果常量是一个对象,其属性是可以被修改的
const Person = {'name': '张三'};
Person.age = 20;//不会报错
Person = {};//报错，企图给常量赋新值
```
2. 属于块级作用域,不存在变量提升,必须先声明后使用,不可重发声明同一个变量,这些与let关键字一样  
3. 声明后必须要赋值  
```js
const NAME;//报错,只声明不赋值
```
# 关于解构赋值
1. 数组的解构赋值
```js
var [e, [f, g], k] = [1, [2, 3], 5];
console.log(e, f, g, k);//1 2 3 5

var [a, b, c] = [1, 2];
console.log(a);//1
console.log(c);//undefined

var [a, b, c = 3] = [1, 2];
console.log(a);//1
console.log(c);//3

var [a, b, c = 3] = [1, 2, 4];
console.log(a);//1
console.log(c);//4(会将初始值覆盖，若新值为undefined,则不会被覆盖)
```
2. 对象的解构赋值
```js
var jike = {"name": "tom", "age": "23", "sex": "男"};
var {name, age, sex} = jike;
console.log(name, age, sex)//tom 23 男

var {a, b, c} = {"a": 1, "c": 2, "b": 3}
console.log(b);//3

var {a} = {"b": 2};
console.log(a);//undefined

var {b: a} = {"b": 2};
console.log(a);//2

var {a, b = 2} = {"a": 1};
console.log(b);//2
```
3. 字符串的解构赋值  
```js
var [a, b, c] = "123";
console.log(a);//1
```
4. 解构赋值的用途  
- 交换变量值  
```js 
var x = 1;
var y = 2;
[x, y] = [y, x];
console.log(x);//2
console.log(y);//1
```
- 提取函数返回的多个值  
``` js
function demo() {
    return {"name": "张三", "age": 12};
}
var {name, age} = demo();
console.log(name);//张三
```
- 定义函数参数
``` js
function demo({a, b, c}) {
    console.log("姓名: " + a);
    console.log("身高: " + b);
    console.log("体重: " + c);
}
demo({a: "张三", b: "1.72cm", c: "80kg"});
```
- 函数参数的默认值  
``` js
function demo({name = "张三"}) {
    console.log("姓名: " + name);
}
demo({});//姓名：张三
```
# 字符串的扩展(常用)
1. 模板字符串  
    用反引号标识符(`)代替单双引号,并且不用(+)来进行字符串的拼接,而是将变量放入${}即可.  
    ${}中可以放任意js的表达式,对象属性以及函数的调用
``` js
let name = "Jacky";
let career = "doctor";
let str = `He is ${name},he is a ${career}`;
console.log(str);//He is Jacky,he is a doctor
```
2. 标签模板:专门处理模板字符串的函数.  
``` js
var name = "张三";
var height = 1.8;
tagFn`他叫${name},身高${height}米.`;
function tagFn(arr, v1, v2) {
    console.log(arr);//["他叫",",身高","米."]
    console.log(v1);//张三
    console.log(v2);//1.8
}
```
3. repeat()函数:将目标字符串重复n次，返回一个新的字符串，不影响原字符串.  
``` js
var name1 = "张三";
var name2 = name1.repeat(3);
console.log(name1);//张三
console.log(name2);//张三张三张三
```
4. includes()函数:判断字符串中是否含有指定的子字符串,返回布尔值,第二个参数选填,表示开始搜索的位置.  
``` js
var a = "abc";
a.includes("a");//true
a.includes("a",1);//false(从第二个字符开始搜索)
```
5. startsWith() && endsWith()函数,判断指定的子字符串是否出现在字符串的开头或末尾.   
第二个参数选填,表示开始搜索的位置或针对前n个字符.

6. String.raw函数:返回字符串最原始的样貌,忽略其中的转义符等.常用来作为一个模板字符串的处理函数.
``` js
console.log(String.raw`hello\nworld`);//hello\nworld
```
