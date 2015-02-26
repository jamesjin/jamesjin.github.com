spring-hibernate-timeout
========================

由于 MySQL Java 驱动的问题， 给查询设置 timeout 会存在内存泄露（scheduled 的任务取消但没有从队列中移除）问题

.. author:: default
.. categories:: none
.. tags:: none
.. comments::
