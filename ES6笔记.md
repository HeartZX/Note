# let const var的区别
var | let| const
---|---|---
函数级作用域 | 块级作用域  | 块级作用域
变量提升 |不存在变量提升 | 不存在变量提升
值可更改 |值可更改 | 值不可更改（复杂类型数据不能直接赋值 但是可以修改 列如数组，对象等 可以添加元素，修改键值）

var命令会发生“变量提升”现象，即变量可以在声明之前使用，值为undefined

# 暂时性死区
使用let声明变量时，只要变量在还没有声明完成前使用，就会报错
。

ES6 规定暂时性死区和let、const语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。这样的错误在 ES5 是很常见的，现在有了这种规定，避免此类错误就很容易了。

总之，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

# ES6中的类和对象
用class关键字声明一个类，之后以这个类来实例化对象


```
class Person{
//类的共有属性放到constructor里面
          constructor(name){
              this.name=name
          }
          //类的方法  多个函数方法之间不需要写逗号
          eat(){
              
          }
      }
      var hzx=new Person('hzx');
      console.log(hzx)
```
类里面的constructor函数，可以接受传递过来的参数，同时返回实例对象，只要new生成实例时，就会自动调用这个函数，如果不写这个函数，类也会自动生成这个函数的

# 类的继承 extends

```
class Father {
      constructor() {}
      money() {
        console.log(100000);
      }
    }
    class Son extends Father {}
    var son = new Son();
    son.money();
```
### super关键字
用于访问和调用对象父类上的函数。可以调用父类的构造函数，也可以调用父类的普通函数

```
  class Father {
      constructor(x, y) {
        this.x = x;
        this.y = y;
        this.say();
      }
      sum() {
        console.log(this.x + this.y);
      }
      say() {
        return "我是爸爸";
      }
    }
    class Son extends Father {
      constructor(x, y) {
        super(x, y); //调用了父类中的构造函数
      }
      say() {
        console.log(super.say() + "的儿子");
        //super.say()调用了父类中的普通函数
      }
    }
    var son = new Son(1, 2);
    son.sum();
    son.say();
```
注意：子类在构造函数中使用super，必须放到this前面（必须先调用父类的构造方法，再使用子类构造方法）

### 三个注意点
1. 类没有变量提升，所以必须先定义类，才能通过类实例化对象
2. 类里面的共有属性和方法一定要加this使用
3. 类里的this指向问题： constructor里面的this指向的是 创建的实例对象，普通函数里的this 指向的是这个方法的调用者


