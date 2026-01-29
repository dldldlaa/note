# js高级

### 垃圾回收机制

内存分配 

内存使用

内存回收



内存泄漏 ：某种原因导致未释放，或者无法释放。

算法说明：

栈：操作系统自动分配释放函数的参数值，局部变量。基本数据类型放到栈中。

堆：由程序员释放，程序员不释放，由垃圾回收机制释放。复杂数据类型放到堆中。



引用计数法和标记清除法。

引用计数法  如果对象嵌套引用会导致无法回收。  对象=null 

标记清除法：

根对象出发，遍历所有可达的对象，标记为存活，这是标记

遍历整个堆内存，将未被标记为存活的对象判定为垃圾，回收其占用的内存空间。





### 闭包

内层函数 + 外层函数的变量。

作用：外部也能访问函数内部的变量。

应用实例  

```js
     function outer() {

            let num = 10;

            function inner() {
                num++;
                console.log(`第${num}次调用函数`);
            }

            return inner;
        }

        const fun = outer();
        fun();
```

num 作为局部变量  不会轻易被修改，保证安全性。

存在内存泄露的风险。



### 箭头函数

```js
//普通写法
const　fn= function(){

}

//箭头函数
const fn1=()=>{
    
}
//只有一个小括号可以省略小括号
const fn1 = x =>{
    
}
fn1(1);

//只有一行代码  省略大括号
const fn2 =(x,y) => console.log(x,y);
fn2(1,2);
```

没有this  去找上一层去找。  需要用到this 不适用箭头函数

```js
const a=["Hydrogen", "Helium", "Lithium", "Beryllium"];
        const a2 =a.map(function(s){
            return s.length;
        })

        const a3=a.map((s)=> s.length);  //语法更为简洁
```

### 日期

```
//  创建一个日期
const newDate= new Date(); 
```

### 正则表达式

匹配字符组合的一种模式。

```js
   const str = "你好我在学习Java和前端";
        const reg = /java/
        console.log(reg.test(str));
```

元字符 ：特殊的字符 

边界符： ^ 开始   $结束

量词 ：设置某个模式出现的次数 

```
*  0次或者多次
+   1次或者多次
？   0次或者1次

{n}  n次
{n, }  n次及以上
{n,m}  n-m次
```

字符类

```
[] 匹配字符 
```



```
   console.log(/^哈$/.test('哈哈'))  //fasle
```

### 对象

属性

行为，方法

```js
// 增删改查
let obj ={
name:"ali",
age:19,
gender:"男"
}
//查 1
obj.age
// 带有字符串 
obj["good-name"]  
// 改 增 
obj.age= 22
//删
delete obj.age

```

#### 遍历对象

```
for　(let k in obj){
console.log(obj[k])   //这么写是因为 k为字符串类型！
}
```

```js
for (let i=0;i<student.length;i++){
for (let key in student[i]){
console.log(student[i][key])
}
}
```

