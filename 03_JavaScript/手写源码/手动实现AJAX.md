# AJAX

## 定义

Ajax（Asynchronous Javascript And XML），即是异步的JavaScript和XML，使用`XMLHttpRequest`对象与服务器通信，也是浏览器与服务器之间的一种异步通信方式。由于具有异步特性，可以在不重刷新页面的情况下与服务器通信，交换数据或更新页面。

## 实现

```js
function ajax(url,method="GET", data=null, async=true) {
  // 声明`XMLHttpRequest` // 在`IE5`和`IE6`中需要使用`ActiveX`对象
  let XHR = XMLHttpRequest;
  // 创建`XMLHttqRequest`对象
  XHR = new XMLHttpRequest();
  // 设置请求状态改变时执行的函数
  XHR.onreadystatechange = function() {
      // `XHR.responseText`为响应体
      if (XHR.readyState === 4) console.log(`响应状态:${XHR.status}`, "FINISH");
  }
  // 初始化请求参数
  XHR.open(method,url, async);
  // 发起请求
  XHR.send(data);
}

ajax("https://www.baidu.com");
ajax("https://www.baidu.com", "POST", "A=1&B=2");
```

## XHR

操作步骤：

1、创建`XMLHttpRequest`对象，用于和服务器间交换数据。2、配置请求方法和请求url地址。

3、监听loadend事件，用于接收服务器响应结果。4、向发起服务器请求。

```js
const xhr = new XMLHttpRequest();
xhr.open('请求方法', '请求url网址');
xhr.addEventListener('loadend', ()=>{
   //响应结果
    console.log(xhr.response);
});
xhr.send();  //发起请求
```

带参数的数据提交

```js
//传递给服务器的内容类型，是json字符串
xhr.setRequestHeader('Content-Type', 'application/json');
//对象转换为json字符串
const user = {username:'czs', password:'123456'};
const userStr = JSON.stringify(user);
//发送请求体数据
xhr.send(userStr);
```

### 创建XMLHttpRequest 对象

```js
var xhr;
if (window.XMLHttpRequest) {
    xhr=new XMLHttpRequest();   //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
}
else {
    xhr=new ActiveXObject("Microsoft.XMLHTTP"); // IE6, IE5 浏览器执行代码
}
```

### xhr请求

```js
//向服务器发送请求
xhr.open("GET","ajax_info.txt",true);
xhr.send();
/**
 * open(method, url, async);  
 *      method：get或post；url：文件在服务器上位置；async：true（异步）或false（同步）
 * send(string);   string：仅用于post请求传递参数
 */
```

**GET 还是POST？**

通常GET请求相比POST请求，更简单更快。以下情况使用POST请求。

- 不愿使用缓存文件（更新服务器上的文件或数据库）
- 向服务器发送大量数据（POST 没有数据量限制）
- 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

### xhr响应

`responseText`    获得字符串形式的响应数据。

`responseXML`      获得 XML 形式的响应数据。

### onreadystatechange事件

每当 readyState 改变时，就会触发 onreadystatechange 事件。

| 属性               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。 |
| readyState         | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。0: 请求未初始化；1: 服务器连接已建立；2: 请求已接收3: 请求处理中；4: 请求已完成，且响应已就绪 |
| status             | 200: "OK" 404: 未找到页面                                    |

### 实例

```js
const xhr = new XMLHttpRequest();
//get请求
xhr.open("GET","/try/ajax/demo_get2.php?fname=Henry&lname=Ford",true);
xhr.send();
//post请求
//传递给服务器的内容类型，是json字符串
xhr.setRequestHeader('Content-Type', 'application/json'); 
//对象转换为json字符串
const user = {username:'czs', password:'123456'};
const userStr = JSON.stringify(user);
//发送请求体数据
xhr.send(userStr);
//获取响应结果 - 第一种
xhr.addEventListener('loadend', ()=>{
   //响应结果
    console.log(xhr.response);
});
//获取响应结果 - 第二种
xhr.onreadystatechange = function() {
    if (xhr.readyState==4 && xhr.status==200) {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
```

## Promise

`promise`对象用于表示一个异步操作的最终完成（或失败）及其结果值。

```js
// 1.创建promise对象  pending待定状态
const p = new Promise((resolve, reject) => {
    //执行异步代码
    setTimeout(() => {
        // pending状态 => resolve() => 'fulfilled状态-已兑现' => then()
        resolve('模拟AJAX请求-成功结果');
        // pending状态 => reject() => 'reject状态-已拒绝' => catch()
        reject('模拟AJAX请求-失败结果');
    }, 2000);
})
p.then(res => {
    console.log(res);
});
p.catch(res => {
    console.log(res);
});
```

基于XMLHttpRequest和Promise封装axios

```js
function request(config) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        //判断是否有请求参数传递
        if (config.params) {
            const paramsObj = new URLSearchParams(config.params);
            const queryString = paramsObj.toString();  //将对象转换为字符串表示形式
            config.url += `?${queryString}`;
        }
        xhr.open(config.method || 'GET', config.url);
        xhr.addEventListener('loadend', () => {
            if (xhr.status >= 200 && xhr.status < 300) {
                resolve(JSON.parse(xhr.response));  //json转对象
            }
            else {
                reject(new Error(xhr.response));
            }
        });
        if (config.data) {
            const jsonStr = JSON.stringify(config.data);  //对象转json字符串
            xhr.setRequestHeader('Content-Type', 'application/json');
            xhr.send(jsonStr);
        }
        else {
            xhr.send();  //发起请求
        }
    })
}
request({
    url: "http://ajax-api.itheima.net/api/area",
    params: {
        pname: '河北省',
        cname: '石家庄市'
    }
}).then(res => {
    console.log(res);
})
```



