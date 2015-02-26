Code Review 的关注点
===========================

代码 Review 时, 通常需要检查以下几个方面:

* 检查输入, 比如前提条件得到满足了吗? 可能出现 NPE 问题吗?
* 代码容易理解吗? 命名很重要
* 异常得到处理了吗? 拒绝空的 catch 代码块, 或者 e.printStackTrace();
* 尽早暴露错误;
* 避免 magic constants;
* 保持之前的代码风格。类似的功能，要采用类似的解决办法;
* 复杂算法逻辑必须有单元测试;
* 没有出现编辑器警告

针对代码所处的环境，比如使用的框架、库等，需要做一些针对性的常见错误检查:

* java 常见错误
* spring 常见错误
* hibernate 常见错误
* spring mvc 常见错误
* javascript 常见错误
* jquery 常见错误
* angular 常见错误





.. author:: default
.. categories:: none
.. tags:: none
.. comments::
