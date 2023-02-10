# 跨域访问

`CORS`(Cross-Origin Resource Sharing,跨源资源共享)是一个系统，它由一系列的`HTTP标头`组成，这些HTTP标头决定了浏览器是否要阻止前端JavaScript代码获取跨源请求的响应。

[同源安全策略](same_origin_policy.md)默认阻止"跨源"获取资源。但是CORS给了web服务器可以选择，允许跨源请求访问他们资源的权限。

## CORS标头

* Access-Control-Allow-Origin   指示响应的资源是否可以给定的来源共享。
* Access-Control-Allow-Credential    指示当请求的凭证标记为*true*时，是否可以公开对请求响应。
* Access-Control-Allow-Headers  用在对预检请求的响应中，指示实际的请求中可以使用哪些 HTTP 标头
* Access-Control-Allow-Methods  指定对预检的响应中，哪些HTTP方法允许访问请求的资源
* Access-Control-Expose-Headers   通过列出标头的名称，指示哪些标头可以作为响应的一部分公开
* Access-Control-Max-Age    指示预检请求的结果能缓存多久
* Access-Control-Request-Headers  用于发起一个预检请求，告知服务器正式请求会使用哪些 HTTP 标头
* Access-Control-Request-Method   用于发起一个预检请求，告知服服务器方式请求会使用哪一种 [HTTP请求方法](http_request_method.md)
* Origin   指示获取资源的请求是从什么源发起的

出于安全性考虑，浏览器限制脚本内发起的跨源HTTP请求，如果想要服务器允许跨源请求，那么在服务器端就需要针对具体的情况分别对`Access-Control-Allow-Origin`，`Access-Control-Allow-Headers`，`Access-Control-Allow-Methods`和`Access-Control-Allow-Credential`来进行设置。对进行跨源设置的资源通常是受[同源策略](./same_origin_policy.md)的保护的资源。在现代的浏览器中支持在API容器(如 XMLHttpRequest 或 [Fetch](./fetch.md)) 使用 CORS。
