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


