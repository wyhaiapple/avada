在http.conf中把mod_expires的开关打开（即把这一行的注释取消，加载expires模块）


# ----------------------------------------------------------------------
# Expires headers (for better cache control)
Add Expires headers
There are 21 static components without a far-future expiration date.

添加这些头的最简单的方法是一个.htaccess文件，它将一些配置添加到你的服务器。如果资产托管在您不受控制的服务器上，您无法做任何事情。
请注意，一些托管提供商不会让你使用.htaccess文件，所以检查他们的条款，如果它似乎不工作。

HTML5Boilerplate项目有一个很好的.htaccess文件，覆盖必要的设置。看到文件的相关部分在他们的Github repository

这些是重要的位

# ----------------------------------------------------------------------

# These are pretty far-future expires headers.
# They assume you control versioning with filename-based cache busting
# Additionally, consider that outdated proxies may miscache
# www.stevesouders.com/blog/2008/08/23/revving-filenames-dont-use-querystring/

# If you don't use filenames to version, lower the CSS and JS to something like
# "access plus 1 week".

<IfModule mod_expires.c>
  ExpiresActive on

# Your document html
  ExpiresByType text/html "access plus 0 seconds"

# Media: images, video, audio
  ExpiresByType audio/ogg "access plus 1 month"
  ExpiresByType image/gif "access plus 1 month"
  ExpiresByType image/jpeg "access plus 1 month"
  ExpiresByType image/png "access plus 1 month"
  ExpiresByType video/mp4 "access plus 1 month"
  ExpiresByType video/ogg "access plus 1 month"
  ExpiresByType video/webm "access plus 1 month"

# CSS and JavaScript
  ExpiresByType application/javascript "access plus 1 year"
  ExpiresByType text/css "access plus 1 year"
</IfModule>

另外：
LoadModule headers_module modules/mod_headers.so
<IfModule mod_headers.c>
    Header append Cache-Control "public, immutable"
</IfModule>
HTTP已经提供了一个新的缓存控制项目：  Cache-Control extensions，在Cache-Control中提供了一个新的扩展属性：immutable，immutable表示响应内容将一直不会改变，它和max-age是对缓存生命周期控制的互补性属性，具体举例如下：
当一个支持immutable的客户端浏览器看到这个属性时，它应该知道：如果没有超过时间上的过期失效时间，那么服务器端该页面内容将不会改变，这样浏览器就不应该再发送有条件的重新验证请求，比如通过If-None-Match 或 If-Modified-Since等条件再向服务器端发出更新检查，也就是说，通常过去我们使用304回复客户端该页面内容没有变化，但是如果用户按浏览器的刷新或F5键，浏览器会再次向服务器端发出该页面内容请求，服务器端如果确认该页面没有变化，那么发回304给客户端，不再发送该页面的实体内容，虽然这样节省了来回流量，但是如果大型网站的很多用户为了得到及时信息，经常会刷新浏览器，这就造成了大量刷新请求，向服务器端求证该页面是否改变，这会影响网站的带宽，也增加服务器端验证压力，而新的选项immutable可以杜绝这种现象。
