# Cross-Origin Resource Sharing (CORS)

> 转译自：https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#Preflighted_requests

Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to tell browsers to give a web application running at one origin, access to selected resources from a different origin. A web application executes a cross-origin HTTP request when it requests a resource that has a different origin (domain, protocol, or port) from its own.

跨域资源共享(Cross-Origin Resource Sharing, CORS) 是一种机制，它使用额外的 HTTP 头来告诉浏览器，让运行在一个域（origin）上的 Web 应用被准许访问来自不同域服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域 HTTP 请求。

译注：「源」和「域」都关联单词 origin，有混用不分的情况，为了和「跨域」对应，本文尽量统一译为「域」，但部分术语仍按照习惯称谓翻译，如「同源策略」。

An example of a cross-origin request: the front-end JavaScript code served from https://domain-a.com uses XMLHttpRequest to make a request for https://domain-b.com/data.json.

跨域请求的一个示例：在 https://domain-a.com 的前端 JavaScript 代码使用 XMLHttpRequest 来请求 https://domb.com/data.json

For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts. For example, XMLHttpRequest and the Fetch API follow the same-origin policy. This means that a web application using those APIs can only request resources from the same origin the application was loaded from, unless the response from other origins includes the right CORS headers.

出于安全原因，浏览器限制脚本发起的跨域 HTTP 请求。 例如，XMLHttpRequest 和 Fetch API 遵循同源策略。这意味着使用这些 API 的 Web 应用程序只能从加载应用程序的同一个域请求 HTTP 资源，除非响应包含了正确 CORS 响应头。

![Cross-Origin-Resource-Sharing-1](/pic/Cross-Origin-Resource-Sharing-1.png)

The CORS mechanism supports secure cross-origin requests and data transfers between browsers and servers. Modern browsers use CORS in APIs such as XMLHttpRequest or Fetch to mitigate the risks of cross-origin HTTP requests.

跨域资源共享机制支持浏览器和服务器端之间以安全的跨域请求来进行数据传输。现代浏览器支持在 API 中（例如 XMLHttpRequest 或 Fetch ）使用 CORS，以降低跨域 HTTP 请求的风险。

## Who should read this article?

**谁应该读这篇文章？**

Everyone, really.

真的，每个人都应该。

More specifically, this article is for web administrators, server developers, and front-end developers. Modern browsers handle the client side of cross-origin sharing, including headers and policy enforcement. But the CORS standard means servers have to handle new request and response headers. Another article for server developers discussing cross-origin sharing from a server perspective (with PHP code snippets) is supplementary reading.

更具体地来讲，这篇文章适用于 web 管理员、后端和前端开发者。现代浏览器处理跨域资源共享的客户端部分，包括 HTTP 头和相关策略的实施。但是 CORS 标准意味着服务器需要处理新的请求头和响应头。另一篇针对服务器开发人员的文章是 [从服务器的角度（使用 PHP 代码片段）讨论跨源共享](https://developer.mozilla.org/en-US/docs/Web/HTTP/Server-Side_Access_Control) 的，可作为补充阅读。

## What requests use CORS?

**什么请求需要 CORS？**

This cross-origin sharing standard can enable cross-site HTTP requests for:

跨域资源共享标准（cross-origin sharing standard）允许在下列场景中使用跨域 HTTP 请求：

- Invocations of the XMLHttpRequest or Fetch APIs, as discussed above.

前文提到的由 XMLHttpRequest 或 Fetch 发起的跨域 HTTP 请求。

- Web Fonts (for cross-domain font usage in @font-face within CSS), so that servers can deploy TrueType fonts that can only be cross-site loaded and used by web sites that are permitted to do so.

Web 字体（CSS 中通过 `@font-face` 使用跨域字体资源），因此，服务器端可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用。

- WebGL textures.

WebGL 贴图

- Images/video frames drawn to a canvas using drawImage().

使用 drawImage() 将 Images/video 画面绘制到 canvas

- CSS Shapes from images.

CSS Shapes from images.

This article is a general discussion of Cross-Origin Resource Sharing and includes a discussion of the necessary HTTP headers.

本文概述了跨域资源共享机制及其所涉及的 HTTP 头。

## Functional overview

**功能概述**

The Cross-Origin Resource Sharing standard works by adding new HTTP headers that let servers describe which origins are permitted to read that information from a web browser. Additionally, for HTTP request methods that can cause side-effects on server data (in particular, HTTP methods other than GET, or POST with certain MIME types), the specification mandates that browsers "preflight" the request, soliciting supported methods from the server with the HTTP OPTIONS request method, and then, upon "approval" from the server, sending the actual request. Servers can also inform clients whether "credentials" (such as Cookies and HTTP Authentication) should be sent with requests.

跨域资源共享标准通过新增 HTTP 头，允许服务器端声明哪些源站可以通过浏览器访问哪些资源的权限。另外，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 方法，或者搭配某些 MIME 类型的 POST 请求），规范要求浏览器必须首先使用 OPTIONS 请求方法发起一个预检请求（preflight request），从而获知服务器端是否允许该跨域请求。服务器「批准」之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证数据）。

译注：文中 `preflight request` 均译为「预检请求」

CORS failures result in errors, but for security reasons, specifics about the error are not available to JavaScript. All the code knows is that an error occurred. The only way to determine what specifically went wrong is to look at the browser's console for details.

CORS 失败会导致错误，但出于安全原因，在 JavaScript 代码层面无法获知错误的详细信息。要确定哪里出了问题，只能查看浏览器的控制台以获得详细信息。

Subsequent sections discuss scenarios, as well as provide a breakdown of the HTTP headers used.

接下来的内容将讨论使用场景，并剖析该机制所涉及的 HTTP 头。

## Examples of access control scenarios

**访问控制场景**

We present three scenarios that demonstrate how Cross-Origin Resource Sharing works. All these examples use XMLHttpRequest, which can make cross-site requests in any supporting browser.

我们使用三个场景来演示跨域资源共享的工作原理。这些例子都使用 XMLHttpRequest 对象，它可以在任何支持的浏览器中发出跨域请求。

A discussion of Cross-Origin Resource Sharing from a server perspective (including PHP code snippets) can be found in the Server-Side Access Control (CORS) article.

[Server-Side Access Control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Server-Side_Access_Control) 这篇文章从服务器的角度（包含 PHP 代码片段）讨论了跨域资源共享。

## Simple requests

**简单请求**

Some requests don’t trigger a CORS preflight. Those are called “simple requests” in this article, though the Fetch spec (which defines CORS) doesn’t use that term. A “simple request” is one that meets all the following conditions:

某些请求不会触发 CORS 预检。本文称这样的请求为「简单请求」，该术语并不属于 Fetch 规范（其中定义了 CORS）。若请求满足所有下述条件，则该请求可视为「简单请求」：

译注：为便于阅读，将原文的几项条件改为小标题形式

（1）One of the allowed methods:

使用了其中一个方法：

- GET
- HEAD
- POST

（2）Apart from the headers automatically set by the user agent (for example, Connection, User-Agent, or the other headers defined in the Fetch spec as a “forbidden header name”), the only headers which are allowed to be manually set are those which the Fetch spec defines as a “CORS-safelisted request-header”, which are:

除了自动设定头的用户代理（例如，连接、用户代理，或其他 Fetch 规范中定义的「禁止的头名称」），Fetch 规范定义了对 CORS 安全的头集合，不得人为设置该集合之外的其他头。该集合为：

- Accept
- Accept-Language
- Content-Language
- Content-Type (but note the additional requirements below)
- DPR
- Downlink
- Save-Data
- Viewport-Width
- Width

（3）The only allowed values for the Content-Type header are:

Content-Type 的值仅限于下列三者之一：

- application/x-www-form-urlencoded
- multipart/form-data
- text/plain

（4）No event listeners are registered on any XMLHttpRequestUpload object used in the request; these are accessed using the XMLHttpRequest.upload property.

请求中的任意 XMLHttpRequestUpload 对象均没有注册任何事件监听器；XMLHttpRequestUpload 对象可以使用 XMLHttpRequest.upload 属性访问。

（5）No ReadableStream object is used in the request.

请求中没有使用 ReadableStream 对象。

> Note: These are the same kinds of cross-site requests that web content can already issue, and no response data is released to the requester unless the server sends an appropriate header. Therefore, sites that prevent cross-site request forgery have nothing new to fear from HTTP access control.

注意: 这些跨域请求与浏览器发出的其他跨域请求并无二致。如果服务器未返回正确的响应头，则请求方不会收到任何数据。因此，那些不允许跨域请求的网站无需为 HTTP 访问控制特性担心。

> Note: WebKit Nightly and Safari Technology Preview place additional restrictions on the values allowed in the Accept, Accept-Language, and Content-Language headers. If any of those headers have ”nonstandard” values, WebKit/Safari does not consider the request to be a “simple request”. What values WebKit/Safari consider “nonstandard” are not documented, except in the following WebKit bugs:

注意：WebKit Nightly 和 Safari Technology Preview 为 Accept, Accept-Language, 和 Content-Language 头的值添加了额外的限制。如果这些头的值是「非标准」的，WebKit/Safari 就不会将这些请求视为「简单请求」。WebKit/Safari 并没有在文档中列出哪些值是「非标准」的，不过我们可以在这里找到相关讨论：

- Require preflight for non-standard CORS-safelisted request headers Accept, Accept-Language, and Content-Language

- Allow commas in Accept, Accept-Language, and Content-Language request headers for simple CORS

- Switch to a blacklist model for restricted Accept headers in simple CORS requests

> No other browsers implement these extra restrictions, because they’re not part of the spec.

其它浏览器并不支持这些额外的限制，因为它们不属于规范的一部分。

For example, suppose web content at https://foo.example wishes to invoke content on domain https://bar.other. Code of this sort might be used in JavaScript deployed on foo.example:

比如说，假如站点 http://foo.example 的网页应用想要访问 http://bar.other 的资源。http://foo.example 的网页中可能包含类似于下面的 JavaScript 代码：

```js
const xhr = new XMLHttpRequest();
const url = "https://bar.other/resources/public-data/";

xhr.open("GET", url);
xhr.onreadystatechange = someHandler;
xhr.send();
```

This performs a simple exchange between the client and the server, using CORS headers to handle the privileges:

这在客户端和服务器之间执行一个简单的交换，使用 CORS 头来处理跨域特权：

![Cross-Origin-Resource-Sharing-2](/pic/Cross-Origin-Resource-Sharing-2.png)

Let's look at what the browser will send to the server in this case, and let's see how the server responds:

让我们看看在这种情况下浏览器会发送什么给服务器，服务器又是如何响应的：

```
GET /resources/public-data/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: https://foo.example
```

The request header of note is Origin, which shows that the invocation is coming from https://foo.example.

注意的请求头是 Origin，它显示调用来自https://foo.example。

```
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 00:23:53 GMT
Server: Apache/2
Access-Control-Allow-Origin: *
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/xml

[…XML Data…]
```

In response, the server sends back an Access-Control-Allow-Origin header. The use of the Origin header and of Access-Control-Allow-Origin show the access control protocol in its simplest use. In this case, the server responds with Access-Control-Allow-Origin: \*, which means that the resource can be accessed by any domain. If the resource owners at https://bar.other wished to restrict access to the resource to requests only from https://foo.example, they would send:

响应中携带了响应头字段 Access-Control-Allow-Origin。使用 Origin 和 Access-Control-Allow-Origin 就能完成最简单的访问控制。本例中，服务端返回的 `Access-Control-Allow-Origin: *` 表明该资源可以被任意域访问。如果服务端仅允许来自 http://foo.example 的访问，该头字段的内容如下：

```
Access-Control-Allow-Origin: https://foo.example
```

Now no domain other than https://foo.example can access the resource in a cross-site manner. To allow access to the resource, the Access-Control-Allow-Origin header should contain the value that was sent in the request's Origin header.

现在，除了 http://foo.example，其它域均不能访问该资源。要允许对资源的访问 Access-Control-Allow-Origin 应当为 `*` 或者包含由 Origin 头字段所指明的域名。

## Preflighted requests

**预检请求**

Unlike “simple requests” (discussed above), "preflighted" requests first send an HTTP request by the OPTIONS method to the resource on the other domain, to determine if the actual request is safe to send. Cross-site requests are preflighted like this since they may have implications to user data.

与上面讨论的「简单请求」不同，「需预检的请求」首先使用 OPTIONS 方法发起一个预检请求到服务器，以确定实际的请求是否可以安全发送。「预检请求」的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

The following is an example of a request that will be preflighted:

如下是一个需要执行预检请求的 HTTP 请求：

```js
const xhr = new XMLHttpRequest();
xhr.open("POST", "https://bar.other/resources/post-here/");
xhr.setRequestHeader("X-PINGOTHER", "pingpong");
xhr.setRequestHeader("Content-Type", "application/xml");
xhr.onreadystatechange = handler;
xhr.send("<person><name>Arun</name></person>");
```

The example above creates an XML body to send with the POST request. Also, a non-standard HTTP X-PINGOTHER request header is set. Such headers are not part of HTTP/1.1, but are generally useful to web applications. Since the request uses a Content-Type of application/xml, and since a custom header is set, this request is preflighted.

上面的代码使用 POST 请求发送一个 XML 文档，该请求包含了一个自定义的请求头（X-PINGOTHER: pingpong）。另外，该请求的 Content-Type 为 application/xml。因此，该请求需要首先发起「预检请求」。

![Cross-Origin-Resource-Sharing-3](/pic/Cross-Origin-Resource-Sharing-3.png)

(Note: as described below, the actual POST request does not include the Access-Control-Request-\* headers; they are needed only for the OPTIONS request.)

（注意：如下所述，实际的 POST 请求不包括 `Access-Control-Request-*` 头，它们只用于 OPTIONS 请求。）

Let's look at the full exchange between client and server. The first exchange is the preflight request/response:

让我们来看看客户端和服务器之间的完整交换。第一个交换是预请求/响应：

```
OPTIONS /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: http://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type


HTTP/1.1 204 No Content
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
```

Once the preflight request is complete, the real request is sent:

一旦预请求完成，真正的请求将发送：

```
POST /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
X-PINGOTHER: pingpong
Content-Type: text/xml; charset=UTF-8
Referer: https://foo.example/examples/preflightInvocation.html
Content-Length: 55
Origin: https://foo.example
Pragma: no-cache
Cache-Control: no-cache

<person><name>Arun</name></person>


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:40 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 235
Keep-Alive: timeout=2, max=99
Connection: Keep-Alive
Content-Type: text/plain

[Some GZIP'd payload]
```

Lines 1 - 10 above represent the preflight request with the OPTIONS method. The browser determines that it needs to send this based on the request parameters that the JavaScript code snippet above was using, so that the server can respond whether it is acceptable to send the request with the actual request parameters. OPTIONS is an HTTP/1.1 method that is used to determine further information from servers, and is a safe method, meaning that it can't be used to change the resource. Note that along with the OPTIONS request, two other request headers are sent (lines 9 and 10 respectively):

上面的第 1-10 行用 OPTIONS 方法表示预检请求。浏览器根据上面 JavaScript 代码段使用的请求参数确定是否需要发送此请求，以便服务器能够响应是否可以使用实际的请求参数发送请求。OPTIONS 是 HTTP/1.1 定义的方法，用于确定来自服务器的进一步信息，并且是一个安全的方法，这意味着它不能用于更改资源。注意，除了 OPTIONS 请求外，还发送了另外两个请求头：

```
Access-Control-Request-Method: POST（原文没看到这个头？）
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
```

The Access-Control-Request-Method header notifies the server as part of a preflight request that when the actual request is sent, it will be sent with a POST request method. The Access-Control-Request-Headers header notifies the server that when the actual request is sent, it will be sent with a X-PINGOTHER and Content-Type custom headers. The server now has an opportunity to determine whether it wishes to accept a request under these circumstances.

Access-Control-Request-Method 头（原文没看到这个头？）告知服务器，实际请求将使用 POST 方法。Access-Control-Request-Headers 头告知服务器，实际请求将携带两个自定义请求字段：X-PINGOTHER 与 Content-Type。服务器据此决定，该实际请求是否被允许。

Lines 13 - 22 above are the response that the server sends back indicating that the request method (POST) and request headers (X-PINGOTHER) are acceptable. In particular, let's look at lines 16-19:

第 14~26 行为预检请求的响应，表明服务器将接受后续的实际请求。重点看这几行：

```
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
```

The server responds with Access-Control-Allow-Methods and says that POST and GET are viable methods to query the resource in question. Note that this header is similar to the Allow response header, but used strictly within the context of access control.

Access-Control-Allow-Methods 头表明服务器允许客户端使用 POST, GET 和 OPTIONS 方法发起请求。该字段与 `HTTP/1.1 Allow: response header` 类似，但仅限于在需要访问控制的场景中使用。

The server also sends Access-Control-Allow-Headers with a value of "X-PINGOTHER, Content-Type", confirming that these are permitted headers to be used with the actual request. Like Access-Control-Allow-Methods, Access-Control-Allow-Headers is a comma separated list of acceptable headers.

Access-Control-Allow-Headers 头表明服务器允许请求中携带字段 X-PINGOTHER 与 Content-Type。与 Access-Control-Allow-Methods 一样，Access-Control-Allow-Headers 的值为逗号分割的列表。

Finally, Access-Control-Max-Age gives the value in seconds for how long the response to the preflight request can be cached for without sending another preflight request. In this case, 86400 seconds is 24 hours. Note that each browser has a maximum internal value that takes precedence when the Access-Control-Max-Age is greater.

最后，Access-Control-Max-Age 头表明该响应的有效时间为 86400 秒，也就是 24 小时。在有效时间内，浏览器无须为同一请求再次发起预检请求。请注意，浏览器自身维护了一个最大有效时间，如果该头的值超过了最大有效时间，将不会生效。

## Preflighted requests and redirects

**预检请求与重定向**

Not all browsers currently support following redirects after a preflighted request. If a redirect occurs after a preflighted request, some browsers currently will report an error message such as the following.

并不是所有浏览器支持针对于预检请求的重定向。如果一个预检请求发生了重定向，浏览器将报告错误：

> The request was redirected to 'https://example.com/foo', which is disallowed for cross-origin requests that require preflight

> Request requires preflight, which is disallowed to follow cross-origin redirect

The CORS protocol originally required that behavior but was subsequently changed to no longer require it. However, not all browsers have implemented the change, and so still exhibit the behavior that was originally required.

CORS 协议最初要求这种行为，但后来被更改为不再需要这种行为。然而，并不是所有的浏览器都实现了这种改变，因此仍然表现出最初需要时的行为。

Until browsers catch up with the spec, you may be able to work around this limitation by doing one or both of the following:

在浏览器的实现赶上规范之前，你可以通过以下一种或两种方式来绕过这个限制：

- Change the server-side behavior to avoid the preflight and/or to avoid the redirect

在服务端去掉对预检请求的重定向；

- Change the request such that it is a simple request that doesn’t cause a preflight

将实际请求变成一个简单请求。

If that's not possible, then another way is to:

如果上面两种方式难以做到，我们仍有其他办法：

1. Make a simple request (using Response.url for the Fetch API, or XMLHttpRequest.responseURL) to determine what URL the real preflighted request would end up at.

发出一个简单请求（使用 Response.url 或 XHR.responseURL）以判断真正的预检请求会返回什么地址。

2. Make another request (the “real” request) using the URL you obtained from Response.url or XMLHttpRequest.responseURL in the first step.

发出另一个请求（真正的请求），使用在上一步通过 Response.url 或 XMLHttpRequest.responseURL 获得的 URL。

However, if the request is one that triggers a preflight due to the presence of the Authorization header in the request, you won’t be able to work around the limitation using the steps above. And you won’t be able to work around it at all unless you have control over the server the request is being made to.

不过，如果请求是由于存在 Authorization 字段而引发了预检请求，则这一方法将无法使用。这种情况只能由服务端进行更改。

## Requests with credentials

**附带身份凭证的请求**

The most interesting capability exposed by both XMLHttpRequest or Fetch and CORS is the ability to make "credentialed" requests that are aware of HTTP cookies and HTTP Authentication information. By default, in cross-site XMLHttpRequest or Fetch invocations, browsers will not send credentials. A specific flag has to be set on the XMLHttpRequest object or the Request constructor when it is invoked.

Fetch 与 CORS 的一个有趣的特性是，可以基于 HTTP cookies 和 HTTP 认证信息发送身份凭证。一般而言，对于跨域 XMLHttpRequest 或 Fetch 请求，浏览器不会发送身份凭证信息。如果要发送凭证信息，需要设置 XMLHttpRequest 的某个特殊标志位。

In this example, content originally loaded from http://foo.example makes a simple GET request to a resource on http://bar.other which sets Cookies. Content on foo.example might contain JavaScript like this:

本例中，http://foo.example 的某脚本向 http://bar.other 发起一个 GET 请求，并设置 Cookies：

```js
const invocation = new XMLHttpRequest();
const url = "http://bar.other/resources/credentialed-content/";

function callOtherDomain() {
  if (invocation) {
    invocation.open("GET", url, true);
    invocation.withCredentials = true;
    invocation.onreadystatechange = handler;
    invocation.send();
  }
}
```

Line 7 shows the flag on XMLHttpRequest that has to be set in order to make the invocation with Cookies, namely the withCredentials boolean value. By default, the invocation is made without Cookies. Since this is a simple GET request, it is not preflighted, but the browser will reject any response that does not have the Access-Control-Allow-Credentials: true header, and not make the response available to the invoking web content.

第 7 行将 XMLHttpRequest 的 withCredentials 标志设置为 true，从而向服务器发送 Cookies。因为这是一个简单 GET 请求，所以浏览器不会对其发起「预检请求」。但是，如果服务器端的响应中未携带 `Access-Control-Allow-Credentials: true`，浏览器将不会把响应内容返回给请求的发送者。

![Cross-Origin-Resource-Sharing-4](/pic/Cross-Origin-Resource-Sharing-4.png)

Here is a sample exchange between client and server:

客户端与服务器端交互示例如下：

```
GET /resources/access-control-with-credentials/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Referer: http://foo.example/examples/credential.html
Origin: http://foo.example
Cookie: pageAccess=2


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:34:52 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Set-Cookie: pageAccess=3; expires=Wed, 31-Dec-2008 01:34:53 GMT
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 106
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain


[text/plain payload]
```

Although line 10 contains the Cookie destined for the content on http://bar.other, if bar.other did not respond with an Access-Control-Allow-Credentials: true (line 17) the response would be ignored and not made available to web content.

即使第 10 行指定了 Cookie 的相关信息，但是，如果 bar.other 的响应中缺失 `Access-Control-Allow-Credentials: true`（第 17 行），则响应内容不会返回给请求的发起者。

## Credentialed requests and wildcards

**附带身份凭证的请求与通配符**

When responding to a credentialed request, the server must specify an origin in the value of the Access-Control-Allow-Origin header, instead of specifying the "\*" wildcard.

对于附带身份凭证的请求，服务器不得设置 Access-Control-Allow-Origin 的值为 `*`。

Because the request headers in the above example include a Cookie header, the request would fail if the value of the Access-Control-Allow-Origin header were "_". But it does not fail: Because the value of the Access-Control-Allow-Origin header is "http://foo.example" (an actual origin) rather than the "_" wildcard, the credential-cognizant content is returned to the invoking web content.

这是因为请求头中携带了 Cookie 信息，如果 Access-Control-Allow-Origin 的值为 `*`，请求将会失败。而将 Access-Control-Allow-Origin 的值设置为 http://foo.example，则请求将成功执行。

Note that the Set-Cookie response header in the example above also sets a further cookie. In case of failure, an exception—depending on the API used—is raised.

另外，响应首部中也携带了 Set-Cookie 字段，尝试对 Cookie 进行修改。如果操作失败，将会抛出异常。

## Third-party cookies

**第三方 cookies**

Note that cookies set in CORS responses are subject to normal third-party cookie policies. In the example above, the page is loaded from foo.example, but the cookie on line 20 is sent by bar.other, and would thus not be saved if the user has configured their browser to reject all third-party cookies.

注意，CORS 响应中设置的 cookie 受正常的第三方 cookie 策略约束。在上面的例子中，页面是从 foo 加载的。例如，第 20 行中的 cookie 是由 bar 发送的。如果用户将浏览器配置为拒绝所有第三方 cookie，则不会保存。

## The HTTP response headers

**HTTP 响应头**

This section lists the HTTP response headers that servers send back for access control requests as defined by the Cross-Origin Resource Sharing specification. The previous section gives an overview of these in action.

本节列出了规范所定义的响应头。上一小节中，我们已经看到了它们在实际场景中是如何工作的。

## Access-Control-Allow-Origin

A returned resource may have one Access-Control-Allow-Origin header, with the following syntax:

响应头可以携带一个 Access-Control-Allow-Origin 字段，其语法如下：

```
Access-Control-Allow-Origin: <origin> | *
```

Access-Control-Allow-Origin specifies either a single origin, which tells browsers to allow that origin to access the resource; or else — for requests without credentials — the "\*" wildcard, to tell browsers to allow any origin to access the resource.

Access-Control-Allow-Origin 指定一个单一的来源，它告诉浏览器允许该来源访问资源；或者，对于没有凭证的请求，使用 `*` 通配符告诉浏览器允许任何源访问资源。

For example, to allow code from the origin https://mozilla.org to access the resource, you can specify:

例如，下面的字段值将允许来自 http://mozilla.com 的请求：

```
Access-Control-Allow-Origin: https://mozilla.org
Vary: Origin
```

If the server specifies a single origin (that may dynamically change based on the requesting origin as part of a white-list) rather than the "\*" wildcard, then the server should also include Origin in the Vary response header — to indicate to clients that server responses will differ based on the value of the Origin request header.

如果服务端指定了具体的域名（that may dynamically change based on the requesting origin as part of a white-list）而非 `*`，那么响应首部中的 Vary 字段的值必须包含 Origin。这将告诉客户端：服务器对不同的源站返回不同的内容。

## Access-Control-Expose-Headers

The Access-Control-Expose-Headers header lets a server whitelist headers that browsers are allowed to access.

Access-Control-Expose-Headers 头让服务器把允许浏览器访问的头放入白名单，例如：

```
Access-Control-Expose-Headers: <header-name>[, <header-name>]*
```

For example, the following:

具体的例子如下：

```
Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header
```

…would allow the X-My-Custom-Header and X-Another-Custom-Header headers to be exposed to the browser.

这样浏览器就能够通过 getResponseHeader 访问 X-My-Custom-Header 和 X-Another-Custom-Header 响应头了。

## Access-Control-Max-Age

The Access-Control-Max-Age header indicates how long the results of a preflight request can be cached. For an example of a preflight request, see the above examples.

Access-Control-Max-Age 头指定了预检请求的结果能够被缓存多久，可参考在前面提到的 preflight 例子。

```
Access-Control-Max-Age: <delta-seconds>
```

The delta-seconds parameter indicates the number of seconds the results can be cached.

delta-seconds 参数表示预检请求的结果在多少秒内有效。

## Access-Control-Allow-Credentials

The Access-Control-Allow-Credentials header Indicates whether or not the response to the request can be exposed when the credentials flag is true. When used as part of a response to a preflight request, this indicates whether or not the actual request can be made using credentials. Note that simple GET requests are not preflighted, and so if a request is made for a resource with credentials, if this header is not returned with the resource, the response is ignored by the browser and not returned to web content.

Access-Control-Allow-Credentials 头指定了当浏览器的 credentials 设置为 true 时是否允许浏览器读取 response 的内容。当用在对预检请求的响应中时，它指定了实际的请求是否可以使用 credentials。请注意：简单 GET 请求不会被预检；如果对此类请求的响应中不包含该字段，这个响应将被忽略掉，并且浏览器也不会将相应内容返回给网页。

```
Access-Control-Allow-Credentials: true
```

Credentialed requests are discussed above.

上文已经讨论了附带身份凭证的请求。

## Access-Control-Allow-Methods

The Access-Control-Allow-Methods header specifies the method or methods allowed when accessing the resource. This is used in response to a preflight request. The conditions under which a request is preflighted are discussed above.

Access-Control-Allow-Methods 头用于预检请求的响应。其指明了实际请求所允许使用的 HTTP 方法。

```
Access-Control-Allow-Methods: <method>[, <method>]*
```

An example of a preflight request is given above, including an example which sends this header to the browser.

上面给出了一个预检请求的示例，包括一个将这个头发送到浏览器的示例。

## Access-Control-Allow-Headers

The Access-Control-Allow-Headers header is used in response to a preflight request to indicate which HTTP headers can be used when making the actual request.

Access-Control-Allow-Headers 头用于响应预检请求，以指示在发出实际请求时可以使用哪些 HTTP 头。

```
Access-Control-Allow-Headers: <header-name>[, <header-name>]*
```

## The HTTP request headers

**HTTP 请求头**

This section lists headers that clients may use when issuing HTTP requests in order to make use of the cross-origin sharing feature. Note that these headers are set for you when making invocations to servers. Developers using cross-site XMLHttpRequest capability do not have to set any cross-origin sharing request headers programmatically.

本节列出了可用于发起跨域请求的头。请注意，这些头无须手动设置。 当开发者使用 XMLHttpRequest 对象发起跨域请求时，它们已经被设置就绪。

## Origin

The Origin header indicates the origin of the cross-site access request or preflight request.

Origin 头表明预检请求或实际请求的源站。

```
Origin: <origin>
```

The origin is a URI indicating the server from which the request initiated. It does not include any path information, but only the server name.

origin 参数的值为源站 URI。它不包含任何路径信息，只是服务器名称。

> Note: The origin value can be null, or a URI.

注意：有时候将该头设置为空字符串是有用的，例如，当源站是一个 data URL 时。

Note that in any access control request, the Origin header is always sent.

注意，在所有访问控制请求（Access control request）中，Origin 头总是被发送。

## Access-Control-Request-Method

The Access-Control-Request-Method is used when issuing a preflight request to let the server know what HTTP method will be used when the actual request is made.

Access-Control-Request-Method 头用于预检请求。其作用是，将实际请求所使用的 HTTP 方法告诉服务器。

```
Access-Control-Request-Method: <method>
```

Examples of this usage can be found above.

相关用法参考前文例子。

## Access-Control-Request-Headers

The Access-Control-Request-Headers header is used when issuing a preflight request to let the server know what HTTP headers will be used when the actual request is made.

Access-Control-Request-Headers 头用于预检请求。其作用是，将实际请求所携带的头告诉服务器。

```
Access-Control-Request-Headers: <field-name>[, <field-name>]*
```

Examples of this usage can be found above.

相关用法参考前文例子。

## Specifications

**规范**

|                       Specification                        |     Status      |                    Comment                    |
| :--------------------------------------------------------: | :-------------: | :-------------------------------------------: |
| [Fetch CORS](https://fetch.spec.whatwg.org/#cors-protocol) | Living Standard | New definition; supplants CORS specification. |

## Browser compatibility

**浏览器兼容性**

|        |       Browser       | Version |
| :----: | :-----------------: | :-----: |
|   PC   |       Chrome        |    4    |
|   PC   |        Edge         |   12    |
|   PC   |       Firefox       |   3.5   |
|   PC   |  Internet Explorer  |   10    |
|   PC   |        Opera        |   12    |
|   PC   |       Safari        |    4    |
| 移动端 |   Android webview   |    2    |
| 移动端 | Chrome for Android  |   Yes   |
| 移动端 | Firefox for Android |    4    |
| 移动端 |  Opera for Android  |   12    |
| 移动端 |    Safari on iOS    |   3.2   |
| 移动端 |  Samsung Internet   |   Yes   |

## Compatibility notes

**兼容性注意事项**

Internet Explorer 8 and 9 expose CORS via the XDomainRequest object, but have a full implementation in IE 10

IE 10 提供了对规范的完整支持，但在较早版本（8 和 9）中，CORS 机制是借由 XDomainRequest 对象完成的。

## See also

**其他参考文献**

- [CORS errors](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors)
- [Enable CORS: I want to add CORS support to my server](https://enable-cors.org/server.html)
- [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Will it CORS?](https://httptoolkit.tech/will-it-cors) - an interactive CORS explainer & generator
- [Using CORS with All (Modern) Browsers](https://www.telerik.com/blogs/using-cors-with-all-modern-browsers)
- [Cross-Origin Resource Sharing From a Server-Side Perspective (PHP, etc.)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Server-Side_Access_Control)
- [How to run Chrome browser without CORS](https://alfilatov.com/posts/run-chrome-without-cors/)
- [Stack Overflow answer with “how to” info for dealing with common problems:](https://stackoverflow.com/questions/43871637/no-access-control-allow-origin-header-is-present-on-the-requested-resource-whe/43881141#43881141)
  - How to avoid the CORS preflight
  - How to use a CORS proxy to get around “No Access-Control-Allow-Origin header”
  - How to fix “Access-Control-Allow-Origin header must not be the wildcard”
