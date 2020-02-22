取消mod_deflate的注释：
LoadModule deflate_module modules/mod_deflate.so


设置压缩一：默认压缩，对图片例外
<IfModule mod_deflate.c>
# 告诉 apache 对传输到浏览器的内容进行压缩
SetOutputFilter DEFLATE
# 压缩等级 9
DeflateCompressionLevel 9
#设置不对后缀gif，jpg，jpeg，png的图片文件进行压缩
SetEnvIfNoCase Request_URI .(?:gif|jpe?g|png)$ no-gzip dont-vary
</IfModule>

设置压缩方案二：压缩对应的文件，这里压缩js和css
<IfModule mod_deflate.c>
    <FilesMatch "\.(js|css)$">
        SetOutputFilter DEFLATE
    </FilesMatch>
</IfModule>

方案3：很全
<IfModule mod_deflate.c>
    SetOutputFilter DEFLATE    #必须的，就像一个开关一样，告诉apache对传输到浏览器的内容进行压缩

    SetEnvIfNoCase Request_URI .(?:gif|jpe?g|png)$ no-gzip dont-vary #设置不对后缀gif，jpg，jpeg，png的图片文件进行压缩
    SetEnvIfNoCase Request_URI .(?:exe|t?gz|zip|bz2|sit|rar)$ no-gzip dont-vary #同上，就是设置不对exe，tgz，gz。。。的文件进行压缩
    SetEnvIfNoCase Request_URI .(?:pdf|mov|avi|mp3|mp4|rm)$ no-gzip dont-vary

    AddOutputFilterByType DEFLATE text/* #设置对文件是文本的内容进行压缩，例如text/html  text/css  text/plain等
    AddOutputFilterByType DEFLATE application/ms* application/vnd* application/postscript application/javascript application/x-javascript #这段代码你只需要了解application/javascript application/x-javascript这段就可以了，这段的意思是对javascript文件进行压缩
    AddOutputFilterByType DEFLATE application/x-httpd-php application/x-httpd-fastphp #这段是告诉apache对php类型的文件进行压缩

    BrowserMatch ^Mozilla/4 gzip-only-text/html # Netscape 4.x 有一些问题，所以只压缩文件类型是text/html的
    BrowserMatch ^Mozilla/4.0[678] no-gzip # Netscape 4.06-4.08 有更多的问题，所以不开启压缩
    BrowserMatch \bMSIE !no-gzip !gzip-only-text/html # IE浏览器会伪装成 Netscape ，但是事实上它没有问题
</IfModule>

