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
