# Ajax
### 创建XMLHttpRequest对象
```
//普通版本
        var xmlhttp;
        if (window.XMLHttpRequest) {
          xmlhttp = new XMLHttpRequest();
        } else {
          xmlhttp = new ActiveXObject("Microsoft.XMLHHTP");
        } 
```

### 封装通用的xhr对象，兼容各个版本

```
//封装通用的xhr对象，兼容各个版本
      function createXHR() {
        //全面版本
        //判断浏览器是否将XMLHttpRequest作为本地对象实现 针对IE7 firefox等
        if (typeof XMLHttpRequest != "undefined") {
          return new XMLHttpRequest();
        } else if (typeof ActiveXObject != "undefined") {
          //将所有可能出现的ActiveXObject版本放在一个数组中
          var xhrArr = [
            "Microsoft.XMLHHTP",
            "MSXML2.XMLHHTP.6.0",
            "MSXML2.XMLHHTP.5.0",
            "MSXML2.XMLHHTP.4.0",
            "MSXML2.XMLHHTP.3.0",
            "MSXML2.XMLHHTP.2.0"
          ];
          //遍历创建XMLHttpRequest对象
          var len = xhrArr.length;
          for (var i = 0; i < len; i++) {
            try {
              //创建XMLHttpRequest对象
              xhr = new ActiveXObject(xhrArr[i]);
              break;
            } catch (ex) {}
          }
          return xhr;
        } else {
          throw new Error("no xhr object availabel");
        }
      }
      //XMLHttpRequest对象
      var xhr = createXHR();
      var data;
      console.log(xhr);
      //响应XMLHttpRequest对象状态变化的函数,onreadystatechange在readystatechange属性发生改变时触发
      xhr.onreadystatechange = function() {
        //异步调用成功,响应内容解析完成 可以在客户端调用
        if (xhr.readyState === 4) {
          if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            //获得服务器返回的数据
            /*   data = eval("(" + xhr.responseText + ")"); */
            data = JSON.parse(xhr.responseText);
            console.log(data);
            console.log(data.code);
            //渲染数据到页面中
            renderDataToDom();
          }
        }
      };
      function renderDataToDom() {}
      //jquery的$.ajax();
```
### 创建http请求 发送请求  添加http头
```
      //创建http请求
      xhr.open("get", "./server/slider.json", true);
      //发送请求
      xhr.send(null);
      /* xhr.send({ user: "hzx", id: 6 });
      //设置http头部信息
      xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); */
```
### jquery的$.ajax()
```
 $.ajax({
        url: "./server/slider.json", //请求地址
        type: "post", //请求方式
        async: true, //同步异步
        dataType: "json", //服务端返回数据格式
        success: function(imgData) {
          //请求成功的回调
          console.log(imgData );
        }
      });
```
### GET与POST的区别
与post相比，GET更简单也更快，并且在大部分情况下都能用，然而，在以下情况下，必须使用post请求：
1. 无法使用缓存文件（更新服务器哈桑的文件或数据库）
2. 向服务器发送大量数据（post没有数据量限制）
3. 发送包含未知字符的用户输入时，POST比GET更稳定也更可靠
### 异步与同步的区别
1. 同步：提交请求->等待服务器处理->处理完毕返回 这个期间客户端浏览器不能干任何事
2. 异步：请求通过事件触发->服务器处理（这是浏览器仍然可以作其他事情）->处理完毕

# JSON
### JSON的语法规则
可以表示三种类型的值
1. 简单值：与javascript相同的语法  可以表示字符串， 数值 布尔值， null（字符串必须用双引号， 不能单引号，  数值必须十进制表示， 且不能使用NaN和Infinity，  但是不支持undefined）
2. 对象：同一个对象不能出现两个同名属性
3. 数组

### JSON对象的两个方法
1. JSON.stringify()
用于将一个值转换为字符串，该字符串符合json格式 并且可以被JSON.parse()还原
2. JSON.parse()
用于将json字符串转化为对象
# 跨域
跨域是指从一个域名的网页去请求另一个域名的资源， 严格一点的定义是：只要  协议 域名 端口有任何一个的不同 就被当作是跨域

### 同源策略
域名 协议 端口均相同

### 解决跨域
- 跨域资源共享(CORS)
- 使用JSONP(常用)
- 修改document.domain
- 使用 window.name
### JSONP的原理
直接用XMLHttpRequest请求不同域上的数据时，是不可以的，但是在页面上引入不同域上的js脚本文件却是可以的 jsonp正是利用这个特性来实现的
 
通过script标签引入js文件=>js文件载入成功后=>执行在url参数中指定的函数

### 原生封装jsonp
```
//封装jsonp
      function getJSONP(url, callback) {
        if (!url) {
          return;
        }
        //声明数组用来随机生成函数名
        var a = ["a", "b", "c", "d", "e", "f", "g", "i", "h", "j"],
          r1 = Math.floor(Math.random() * 10),
          r2 = Math.floor(Math.random() * 10),
          r3 = Math.floor(Math.random() * 10),
          name = "getJSONP" + a[r1] + a[r2] + a[r3];
        cbname = "getJSONP." + name;
        console.log(cbname);
        //判断URL地址中是否含有？号
        if (url.indexOf("?") === -1) {
          url += "?jsonp=" + cbname;
        } else {
          url += "&jsonp=" + cbname;
        }
        console.log(url);
        //动态创建script标签
        var script = document.createElement("script");
        //定义被脚本执行的回调函数
        getJSONP[name] = function(data) {
          try {
            callback && callback(data); //callback为真 就执行callback函数
          } catch (e) {
          } finally {
            //最后删除该函数及script标签
            delete getJSONP[name];
            script.parentNode.removeChild(script);
          }
        };
        //定义script的src
        script.src = url;
        document.getElementsByTagName("head")[0].appendChild(script);
      }
      getJSONP("http://class.imooc.com/api/jsonp", function(data) {
        console.log(data);
      });
```

