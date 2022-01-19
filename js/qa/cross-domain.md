![cross-domain](../images/qa.jpg)

# 跨域

## 什么是同源

如果两个 URL 的协议、域名、端口号、完全一致，那么这两个 URL 就是同源的。

## 什么是跨域

浏览器规定不同源的页面之间，不准互相访问数据。但有时不同源的页面之间能够互相访问数据，对于开发某些浏览器应用程序也是至关重要的。正是因为这个原因跨域诞生了。

## JSONP 跨域

JSONP 是 JSON with padding 的简写。

假设有两个网站，A.com 和 B.com，当 B 网站想要访问 A 网站的数据时，就得执行以下步骤：

1. A.com 将数据写到 /friends.js（可以是任意名字的 js 文件）
2. B.com 用 script 标签引用 /friends.js
3. /friends.js 执行 B.com 事先定义好的 window.xxx 函数，即执行 `window.xxx({firend:[...]})`
4. 然后 B.com 就通过 window.xxx 获取到了数据，这里 window.xxx 就是一个回调函数。

B.com 的客户端示例代码：

```jsx
function jsonp(url) {
  return new Promise((resolve, reject) => {
    const random = 'BJSONPCallbackName' + Math.random()
    window[random] = (data) => {
      resolve.call(null, data)
    }
    const script = document.createElement('script')
    script.src = `${url}?callback=${random}`
    script.onload = () => {
      script.remove()
    }
    script.onerror = () => {
      reject('request was unsuccessful')
    }
    document.body.appendChild(script)
  })

}

jsonp('http://A.com:8888/friends.js')
  .then(console.log, console.log)
```

A.com 的服务器端示例代码：

```jsx
if (path === '/friends.js') {
  if (request.headers["referer"].indexOf("http://B.com:9999") === 0) {
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/javascript;charset=utf-8')
    const string = `window['{{xxx}}']( {{data}} )` // friends.js
    const data = fs.readFileSync('./public/friends.json').toString()
    const string2 = string.replace('{{data}}', data).replace('{{xxx}}', query.callback)
    response.write(string2)
    response.end()
  }
}
```

JSONP 的优点：支持 IE 同时可以跨域。

JSONP 的缺点：因为它使用的是 script 标签，所有不如 AJAX 那样精确，如无法获取请求头也无法获取状态码，同时只能使用 GET 请求。

## CORS 跨域

CORS（Cross-Origin Resource Sharing，跨域资源共享）定义了在必须访问跨域资源时，浏览器与服务器应该如何沟通。CORS 背后的基本思想，就是使用自定义的 HTTP
头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。

比如一个简单的使用 GET 或 POST 发送的请求，他没有自定义的头部，而主体内容是 text/plain。在发送该请求时，需要给它附加一个额外的 Origin
头部，其中包含请求页面的源信息（协议、域名和端口），以便服务器根据这个头部信息决定是否给予响应。下面是 Origin 头部的一个示例：

```text
Origin: http://www.wheatfields.online
```

如果服务器认为这个请求可以接受，就在 Access-Control-Allow-Origin 头部中回发相同的源信息（如果是公共资源，可以回发”*”）。例如：

```text
Access-Control-Allow-Origin: http://www.wheatfields.online
```

如果没有这个头部，或者有这个头部但源信息不匹配，服务器就会驳回请求。正常情况下，服务器会处理请求。注意，请求和响应都不包含 cookie 信息。