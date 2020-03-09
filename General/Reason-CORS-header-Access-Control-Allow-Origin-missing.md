# Reason: CORS header 'Access-Control-Allow-Origin' missing

> 转译自：https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors/CORSMissingAllowOrigin

## Reason

> Reason: CORS header 'Access-Control-Allow-Origin' missing

## What went wrong?

The response to the CORS request is missing the required Access-Control-Allow-Origin header, which is used to determine whether or not the resource can be accessed by content operating within the current origin.

对 CORS 请求的响应缺少必需的 Access-Control-Allow-Origin 头，它用于确定在当前源内操作的资源是否可以访问。

If the server is under your control, add the origin of the requesting site to the set of domains permitted access by adding it to the Access-Control-Allow-Origin header's value.

如果服务器在你的控制之下，可将请求站点的源添加到允许访问的域集，方法是将其作为值添加到 Access-Control-Allow-Origin 头。

For example, to allow a site at https://amazing.site to access the resource using CORS, the header should be:

例如，要允许 https://amazing.site 上的站点使用 CORS 访问资源，这个 http 头应该为：

```
Access-Control-Allow-Origin: https://amazing.site
```

You can also configure a site to allow any site to access it by using the _ wildcard. You should only use this for public APIs. Private APIs should never use _, and should instead have a specific domain or domains set. In addition, the wildcard only works for requests made with the crossorigin attribute set to anonymous, and it prevents sending credentials like cookies in requests.

你还可以使用 `*` 通配符配置站点使得允许任何站点访问它。你应该只将它用于公共的 API。私有 API 永远不应使用 `*`，而应设置特定的一个域或一些域。此外，通配符仅适用于将 crossorigin 属性设置为 `c` 的请求。

```
Access-Control-Allow-Origin: *
```

> Warning: Using the wildcard to allow all sites to access a private API is a bad idea.

警告: 使用通配符允许所有站点访问私有 API 是坏主意。

To allow any site to make CORS requests without using the \* wildcard (for example, to enable credentials), your server must read the value of the request's Origin header and use that value to set Access-Control-Allow-Origin, and must also set a Vary: Origin header to indicate that some headers are being set dynamically depending on the origin.

要允许任何网站发出 CORS 请求不使用 \* 通配符（for example, to enable credentials），你的服务器必须读取请求的 Origin 头的值来设置 Access-Control-Allow-Origin，还必须设置不同：Origin 头表明一些头被动态设置根据原点。

The exact directive for setting headers depends on your web server. In Apache, add a line such as the following to the server's configuration (within the appropriate <Directory>, <Location>, <Files>, or <VirtualHost> section). The configuration is typically found in a .conf file (httpd.conf and apache.conf are common names for these), or in an .htaccess file.

设置头的确切指令取决于 web 服务器。例如，在 Apache 服务器中，将下面一行添加到服务器的配置中（在相应的 `<Directory>`，`<Location>`，`<Files>` 或 `<VirtualHost>` 部分中）。 配置通常位于.conf 文件中（httpd.conf 和 apache.conf 是这些文件的通用名称），或者位于.htaccess 文件中。

```
Header set Access-Control-Allow-Origin 'origin-list'
```

For Nginx, the command to set up this header is:

对于 Nginx，设置此 http 头的指令是：

```
add_header 'Access-Control-Allow-Origin' 'origin-list'
```

## See also

- CORS errors
- Glossary: CORS
- CORS introduction
