#改变固态链接后所有页面出现404问题
[![](https://github.com/wyhaiapple/avada/blob/master/1.png)

网上修改htaccess的方法都不管用，其实现在版本的wordpress已经自动生成了htaccess文件来处理伪静态。
研究了解决方案为修改apache下的http.conf文件：
1、将所有的AllowOverridenone修改为AllowOverrideAll；
2、找到#LoadModule rewrite_module modules/mod_rewrite.so，去掉注释
重启服务，ok！
