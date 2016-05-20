#html5的web存储
[TOC]
在客户端存储数据

HTML5 提供了两种在客户端存储数据的新方法：
 
    1. localStorage - 没有时间限制的数据存储
    2. sessionStorage - 针对一个 session 的数据存储

cookie 不适合大量数据的存储

HTML5 使用 JavaScript 来存储和访问数据。
##localStorage

`localStorage`方法存储的数据没有时间限制。第二天、第二周或下一年之后，数据依然可用。

实例1:
```javascript
localStorage.lastname="Smith";
document.write(localStorage.lastname);
```

实例2,对用户访问页面的次数进行计数：
```javascript
if (localStorage.pagecount)
  {
  localStorage.pagecount=Number(localStorage.pagecount) +1;
  }
else
  {
  localStorage.pagecount=1;
  }
document.write("Visits "+ localStorage.pagecount + " time(s).");
```

##sessionStorage

`sessionStorage` 方法针对一个`session`进行数据存储。当用户关闭浏览器窗口后，数据会被删除。

实例3:
```javascript
sessionStorage.lastname="Smith";
document.write(sessionStorage.lastname);
```

实例4,对用户在当前 session 中访问页面的次数进行计数：

```javascript
if (sessionStorage.pagecount)
  {
  sessionStorage.pagecount=Number(sessionStorage.pagecount) +1;
  }
else
  {
  sessionStorage.pagecount=1;
  }
document.write("Visits "+sessionStorage.pagecount+" time(s) this session.");
```
