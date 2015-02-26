Freemarker
==========

.. _why_freemarker:

为什么要用Freemarker
---------------------

.. _freemarker_best_practices:

Freemarker常用技巧
-----------------------

.. _setting:

页面的 freemarker 设置
========================

#. Locale

#. Time/Date/DateTime  如果某个页面需要特殊的时间格式，可以用 <#setting date_format="yyyyMMdd" > time_format, datetime_format


时间、日期格式
========================
freemarker的日期/时间输出可以用:
${dateRange.from?date}
${dateRange.from?time}
${dateRange.from?datetime}

如果怕值不存在，可以用: 
${(dateRange.from?date)!}
${(dateRange.from?time)!}
${(dateRange.from?datetime)!}

* 日期: startDay?date
* 日期+时间: beginTime?datetime
* 时间: begin

.. author:: default
.. categories:: none
.. tags:: none
.. comments::
