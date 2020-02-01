1.普通函数this指向window

```
function fn(){
    console.log(this)
}
```
2.对象方法的函数this指向对象obj

```
var obj={
    sayHi:function(){
        console.log(this)
    }
}
```
3.构造函数this 指向实例对象(hzx)
原型对象里面的this 指向的也是实例对象(hzx)


```
function Person(){
    console.log(this)
}
Person.prototype.sing=function(){}
var hzx=new Person()
```
4.绑定事件函数this指向的是函数的调用者(btn这个按钮对象)

```
var btn=document.querySelector('button')
btn.onclick=function(){
console.log(this)
}
```
5.定时器函数 this指向window

```
window.setTimeout(function(){
    console.log(this)
})
```
6.立即执行函数  this指向window

```
(function(){
console.log(this)
})()
```
### 改变函数内部this指向

#### 1.call方法 
##### call() 可以调用函数  可以改变函数内的this指向

```
var obj={
    name:'andy'
}
function fn(a,b){
    console.log(this)
    console.log(a+b)
}
fn.call(obj,1,2)
```
#####  主要作用可以实现继承

```
function Father(uname,age,sex){
    this.uname=uname;
    this.age=age;
    this.sex=sex;
}
function Son(uname,age,sex){
    Father.call(this,uname,age,sex)
}
var hzx=new Son('hzx','18','男')
console.log(hzx)
```
#### 2.apply方法
也是调用函数 第二个可以改变函数内部的this指向

但是他的参数必须是数组(伪数组也行)

apply的主要应用 比如可以利用apply 借助于Math求最大值 

```
var arr=[1,66,3,99,4]
console.log(Math.max.apply(Math,arr))
console.log(Math.min.apply(Math,arr))
```


```
fun.apply(thisArg,[argsArray])
```
-   thisArg:在fun函数运行时指定的this值
- argsArry: 传递的值，必须包含在数组里面
- 返回值就是函数的返回值，因为它就是调用函数


```
var obj={
    name:'andy'
}
function fn(arr){
    console.log(this)
    console.log(arr)
    }
fn.apply(obj，['0','1','2','3'])
```
#### 3.bind方法

bind()方法不会调用函数，但是能改变函数内部this指向

```
fun.bind(thisArg,arg1,arg2)
```
-   thisArg:在fun函数运行时指定的this值
- arg1,arg2: 传递的其他参数
- 返回由指定的this值和初始化参数改造的原函数拷贝


```
var obj={
    name:'andy'
}
function fn(a,b){
    console.log(this)
    console.log(a+b)
}

var f=fn.bind(obj,1,2); //不会执行 产生了原函数改变this之后新的函数
 f();
```
如果有的函数不需要立即调用 但是又想改变这个函数的this指向此时用bind

有一个按钮 当点击之后 就禁用这个按钮 3秒之后开启这个按钮

```
<button>点击</button>

var btn=document.querySelector('button');
btn.onclick=function(){
    this.disabled=true;//这个this指向的是btn这个按钮
    
    setTimeout(function(){
       // this.disabled=false;//定时器函数里面的this 指向的window
        this.disabled=false;
    }.bind(this),3000)
}

```

### call apply bind 总结
#### 相同点：
都可以改变函数内部的this指向

#### 区别点：
1.call和apply会调用函数，并且改变函数内部this指向

2.call和apply传递的参数不一样，call传递参数aru1，aru2形式 而apply必须是数组形式[arg]

3.bind不会调用函数 可以改变函数内部this指向





