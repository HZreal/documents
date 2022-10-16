# Http

## jsonp

概念：浏览器通过script标签中src属性向服务端发送执行函数的名字，服务端根据函数名、服务端数据，拼接成一个函数调用的字符串给客户端script标签执行，这称之为JSONP

特点：

- 不属于axios请求，因为没有使用XMLHttpRequest对象
- 仅支持get请求，不支持post、put、delete

