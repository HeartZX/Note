### v-cloak指令的用法

1.提供样式
[v-cloak]{
    display:none;
}

2.在插值表达式所在的标签中添加v-cloak指令

可以解决页面刷新时 插值表达式会显示花括号闪动的问题

背后的原理就是 先通过样式隐藏内容，然后在内存中进行值的替换 替换好了之后再显示最终的结果


### MVVM设计思想<br>
M model<br>
V view<br>
VM View-Model

### 事件绑定-参数传递:
1.如果事件直接绑定函数名称，那么默认会传递事件对象作为事件函数的第一个参数<br>
2.如果事件绑定函数调用，那么事件对象必须作为最后一个参数显示传递，并事件对象的名称必须是 $event<br>
$event 函数中传递事件对象:<br>
Handle('123','456' ,$event)<br>

### 事件绑定-事件修饰符
//阻止冒泡<br>
v-on:click.stop<br>
//阻止默认行为<br>
v-on:click.prevent<br>

### 按键修饰符
### 自定义按键修饰符

### 属性绑定
v-bind 动态属性绑定 <br>
v-model 指令的本质 底层原理分析:<br>

```
<input v-bind:value='msg' v-on:input='msg=$event.target.value'>
```


### 样式绑定
赋值反向的值

```
this.isActive=!this.isActive;
```


####  class样式处理
 
 1.对象语法

```
<input type="text" v-bind:class='{active:isActive}' />
```

  
 2.数组语法

```
<input type="text" v-bind:class="[activeClass,XXX]"/>
```

 
####  样式绑定相关语法细节
 1.对象绑定和数组绑定可以结合使用
 
 2.clas绑定的值可以简化操作 写在data里
 
 3.默认的class如何处理？ 默认的class会被保留
 
####  style样式处理

 内联样式style
 
 <input type="text" v-bind:style='{}' />
 
####  分支循环结构

根据条件去判断 是否渲染到页面

v-if

v-else-if

v-else

v-show 实际上渲染了 只是display设成了none


### 循环结构
v-for


```
<li v-for="(item,index) in fruits">{{item+'---'+index}} </li>
```


key的作用：帮助Vue区分不同的元素 从而提高性能


```
 <li ：key='item.id' v-for="(item,index) in fruits">{{item+'---' +index}} </li>
```

v-for 遍历对象

```
   <div v-for="(value,key,index) in person">
        {{index+'--'+key+'--'+value}}
      </div>
```
v-if和v-for结合使用
```
<div v-if='value==12' v-for="(value,key,index) in object">
        {{index+'--'+key+'--'+value}}
      </div>
```

#### 表单域修饰符

number：转化为数值

trim：去掉开始和结尾的空格

lazy：将input事件切换为change事件

```
<input v-model.number='age' type='number'>
```
#### 自定义指令

全局指令
```
Vue.directive('focus',{
            inserted:function(el){
             el.focus()
            }
        })
```

带参数指令的用法

```
Vue.directive("color", {
        bind: function(el, binding) {
          el.style.backgroundColor = binding.value;
        }
      });
```

局部指令

```
directives:{
    color:{
        bind:function(el, binding) {
          el.style.backgroundColor = binding.value;
        }
    }
}
```

### 计算属性

##### 计算属性缓存 vs 方法 区别

 computed
 计算属性是基于他们的依赖进行缓存的
 
 methods 方法不存在缓存
 
###   侦听属性
watch 应用场景： 数据变化时执行异步或开销比较大的操作

```
watch: {
    firstName: function (val) {
    //val表示变化之后的值
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
```

### 过滤器

格式化数据 比如将字符串格式化为首字母大写 将日期格式化为指定的格式等

全局
```
Vue.filter('upper',function(val){
        return val.charAt(0).toUpperCase()+val.slice(1)
      })
```
局部
```
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```


### 组件

组件中的data写的必须是函数

```
      Vue.component("button-counter", {
        data: function() {
          return {
            count: 0
          };
        },
        methods: {
          handle: function() {
            this.count += 2;
          }
        },
        template: '<button @click="handle ">点击了{{count}}次</button>'
      });
```
组件命名方式 
如果使用驼峰命名组件 只能在字符串模板中使用 但是普通的标签模板中 必须使用短横线的方式

局部组件
只能在注册他的父组件中使用


### 父组件向子组件传值

子组件通过props接收传递过来的值

```
     Vue.component("menu-item", {
        props:['menuTitle','content'],
        data: function() {
          return {
            msg: "我是子组件"
          };
        },
        template: "<div>{{msg+'---'+menuTitle+'-----'+content}}</div>"
      });
```

父组件通过属性将值传递给子组件

```
 <menu-item :menu-title="rootMsg" :content='pContent'></menu-item>
```
#### props属性名规则
在props中使用驼峰形式，模板中需要使用短横线形式

字符串形式的模板中没有这个限制


### 子组件向父组件传值

props传递数据原则：单向数据流 

只允许父组件向子组件传递
子组件不能操作父组件中的数据

而是通过自定义事件向父组件传递信息

```
 <button @click='$emit("entarge",5)'>点击</button>
```


父组件中监听子组件的事件
 
```
<menu-item @entarge='handle($event)' ></menu-item>
```
 
###  非父子组件间传值（兄弟组件之间数据传递）


### 组件插槽的作用

父组件向子组件传递内容
 
 插槽位置
```
   Vue.component("alert-box", {
        template: `
          <div>
            <strong>ERROR:</strong>
            <slot>默认内容</slot>  
          </div>
          `
      });
```
插槽内容 父组件中

```
<alert-box>有危险</alert-box>
      <alert-box></alert-box>
```

### 具名插槽

```
 Vue.component("base-layout", {
        template: `
          <div>
            <header>
                <slot name='header'></slot>      
           </header>
           <main>
                <slot></slot>      
           </main>
           <footer>
                <slot name='footer'></slot>      
           </footer>
          </div>
          `
      });
```

```
      <base-layout>
        <p slot="header">我是标题</p>
        <p>我是内容</p>
        <p slot="footer">我是底部</p>
      </base-layout>

      <base-layout>
        <template slot="header">
          <p>我是标题1</p>
          <p>我是标题2</p>
        </template>
        <template>
          <p>我是内容1</p>
          <p>我是内容2</p>
        </template>
        <template slot="footer">
          <p>我是底部1</p>
          <p>我是底部2</p>
        </template>
      </base-layout>
```
### 作用域插槽

应用场景：父组件对子组件的内容进行加工处理

子组件中定义插槽
    
```
Vue.component("fruit-list", {
        props: ["list"],
        template: `
        <div>
            <li :key='item.id' v-for='item in list'>
              <slot :info='item'>{{item.name}}</slot> 
                </li>
        </div>
        `
      });
```

父组件插槽内容

```
      <fruit-list :list="fruitList">
          <template slot-scope='slotProps'>
              <strong class="current" v-if='slotProps.info.id==2'>{{slotProps.info.name}}</strong>
               <span v-else>{{slotProps.info.name}} </span>
          </template>
      </fruit-list>
```
### Vuex

state 提供唯一的公共数据源 所有共享的数据都要统一放到store的state中进行存储


```
export default new Vuex.Store({
  state: {
    count:0
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```
组件访问state中数据的第一种方式

```
this.$store.state.count
```
组件访问state中数据的第二种方式

```
//1.从vuex中按需导入mapstate函数
import {mapstate} from 'vuex'
```
通过刚才导入的mapStat函数 将当前组件需要的全局数据，映射为当前组件的computed计算属性：

```
//2.将全局数据 映射为当前组件的计算属性
computed：{
    ...mapState(['count'])
}
```
用Mutation去操作store中数据 可以集中监控所有数据的变化

```
mutations: {
    add(state){
       //变更状态
       state.count++
    }
  }
```
```
	methods: {
		addHandler() {
            //触发mutations的第一种方式
			this.$store.commit('add')
		}
	}
```
可以在触发mutations时传递参数

```
mutations: {
    addN(state,step){
       //变更状态
       state.count+=step
    }
  },
```

```
	methods: {
		addHandler() {
            //触发mutations的传递参数
			this.$store.commit('addN',3)
		}
	}
```

触发Mutaions的第二种方法

```
//1.从vuex中按需导入mapMutations函数
import {mapMutations} from 'vuex'
```
通过刚才导入的mapMutations函数，将需要的mutations函数 映射为当前组件的methods方法

```
//2.将指定的mutations函数 映射为当前组件的methods函数
    methods:{
        ...mapMutations(['subN']),
        subHandler(){
            this.subN(2)
        }
    }
```
Action用于处理异步任务
如果通过异步操作变更数据 必须通过Action，而不能使用Mutation 但是在Action中还是要通过触发Mutation的方式间接变更数据

```
  actions: {
    //用于处理异步任务  不能直接修改state中数据
    //必须通过context.commit()触发mutations
    addAsync(context){
        setTimeout(()=>{
          context.commit('add')
        },1000)
    }
  }
```
```
 addAsyncHandler(){
            //dispatch函数 专门用来触发action
            this.$store.dispatch('addAsync')
        }
```
触发actions的第二种方式  映射方法 
```
methods:{
     
        ...mapActions(['subAsync']),
        subAsyncHandler(){
            this.subAsync(4)
        }
    }
```

### Getter
使用getter的第一种方式

```
this.$store.getters.名称
```
使用getter的第二种方式

```
import {mapGetters} from 'vuex'

computed:{
        //三个点表示展开运算符
        ...mapGetters(['showNum'])
    }

```



