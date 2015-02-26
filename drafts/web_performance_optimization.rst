前端性能优化 - CSS/JS 合并, 最小化, 压缩和缓存
===========================================

合并
----
css/js 文件减少请求数

最小化
------
去掉空行、注释、无用代码以减小文件

版本化
-------
在文件名中增加版本号

压缩
------

IE6 gunzip
An IE6 user who is current on patches should be able to properly decompress any response -- XPSP2 (identified by the SV1) shouldn't be required. In contrast, an IE6 user who is not current on patches has probably already been pwned by the bad guys.

缓存
-----

Cache-Control
++++++++++++++++++++

1. no_store
2. no_cache
1. private/public
2. max-age
3. s-maxage
4. must-revalidate

expires
+++++++++++++++++++++

conditional requests
+++++++++++++++++++++++++++++++++++

1. etag

If-None-Match, If-Match, If-Range header

2. last-modified

If-Modified-Since, If-Unmodified-Since

缓存失效
++++++++++++++

1. 过期
2. 缓存满了
3. javascript reload
4. meta refresh
5. refresh
6. shift refresh
7. 针对同一URL有post, put, delete 操作

http://podlipensky.com/2012/03/behind-refresh-button/
http://podlipensky.com/2012/04/when-cached-items-became-invalid/
http://blogs.msdn.com/b/ieinternals/archive/2010/07/08/technical-information-about-conditional-http-requests-and-the-refresh-button.aspx

陷阱
+++++++++

1. etag
多服务器情况下, IIS 和 apache 的多个服务器会生成不同的 etag, 导致 etag 失效

2. vary
若 Response 不是 gzip 格式, 但设置了 vary 头, 则
IE 6 无法缓存
IE 7 可以缓存, 但必须revalidate

若 Response 是 gzip 格式, 但设置了 vary:accept-encoding 头, 则可以缓存, 否则同上

3. Firefox缓存键值冲突

Avoid URL hash collision in Mozilla Firefox ensuring that your application generates URLs that differ on more than 8-character boundaries

5. use cache-control public to enable https caching in Firefox

6. Resources with a "?" in the URL are not cached by some proxy caching servers.

Google Pagespeed 的这条规则应该过时了, 现在这样的代理服务器应该很少了。


http://blogs.msdn.com/b/ieinternals/archive/2009/06/17/vary-header-prevents-caching-in-ie.aspx
http://www.bizcoder.com/the-insanity-of-the-vary-header
http://gtmetrix.com/leverage-browser-caching.html
http://blogs.msdn.com/b/ie/archive/2010/07/14/caching-improvements-in-internet-explorer-9.aspx



.. author:: default
.. categories:: none
.. tags:: none
.. comments::
