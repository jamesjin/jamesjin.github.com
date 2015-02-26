可以在DAO层加事务吗？
===================
大部分Java项目都会采用dao、service、controller这样的分层结构。在这样的项目中常常会发现，很多DAO中声明过的方法都要在service中再声明一遍，而其实现却仅仅是一个简单的dao调用。这样的重复代码，难占到80%以上。重复代码坏处实在多，不了解的可以读下这篇 :ref:`dry` 。

.. code-block:: java

  @Transactional
  public void save(User u) {
      dao.save(u);
  }

那为什么要重复的声明相同的方法呢？这是因为很多项目都遵循着两个原则：
  * 数据库操作都要加事务；
  * 只在 service 层加事务。

这样，原本可以直接调用 dao 的 CRUD 类业务，就必须在 service 中再声明一遍。

那为什么要遵循这两个原则呢？首先

看以下这段代码：

.. code-block:: java

  logger.debug("add a new task {}", task.toString());   // bad, task.toString might be time costing
  if (logger.isDebugEnabled()) { // good
      logger.debug("add a new task " + task.toString());
  }
  logger.debug("add a new task " + task.getName()); // bad, there is a better way
  logger.info("add a new task, name: {}", task.getName());   // good
  logger.debug("find {} valid users.", num);                 // good

总的来说，对于采用分层结构的Java应用程序，可以遵循以下原则以减少冗余代码，提高效率。

  #. DAO和service类都不用接口；
  #. 每个Model类一个DAO类，而且DAO类加入事务；
  #. 如果一个业务需求涉及多个DAO，或一个DAO的多个方法，则在service中为此创建一个transactional方法。
  #. 需要的时候再加service类，而且也并不是一定要把service类命名成某个model类的service。比如，订单（Trade）和 子订单（SubTrade），没有必要单独声明一个 SubTradeService，而是把子订单的service代码加入TradeService为好。

.. author:: default
.. categories:: java
.. tags:: dao, transaction, spring, hibernate
.. comments::

