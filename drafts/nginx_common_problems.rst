nginx_common_problems
=====================

.. _working_directory:

工作路径
-------------
nginx 进程的工作路径在默认情况下为 /etc/nginx; 配置文件里也可以通过 working_directory 这个配置项来设置. 

proxy_set_header
----------------

上一级定义的proxy_set_header只有在下一级没有定义此标签时才会被继承

Syntax:  proxy_set_header field value;
Default:  
proxy_set_header Host $proxy_host;
proxy_set_header Connection close;
Context:  http, server, location
Allows redefining or appending fields to the request header passed to the proxied server. The value can contain text, variables, and their combinations. These directives are inherited from the previous level if and only if there are no proxy_set_header directives defined on the current level

client_max_body_size
----------------------
默认只有 1m，而我们一般需要默认为 10m

gzip_types
----------------------
不需要添加text/html，这个是默认的，只需要添加 text/plain application/json等

access_log
----------------------
如果要关闭，设置 access_log 为 off, 开启不能设置为 on 而是注释掉 off 那一行

与后台服务器的 keep_alive 设置
------------------------------

proxy_http_version      1.1;
proxy_set_header        Connection "";

cache_control 最多为1年
-----------------------------


.. author:: default
.. categories:: none
.. tags:: none
.. comments::
