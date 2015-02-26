Spring MVC 精要
====================
Spring MVC 的重点是 Controller, 而 Controller 的重点则是 "请求映射", "参数解析" 和 “视图选择”. "请求映射" 解决的是如何将一个 HTTP 请求映射到 Java 方法的问题; "参数解析" 可以将 HTTP 请求自动转换成 Command 并进行验证; “视图选择” 根据 Command 的执行结果, 选择对应的视图并提供视图渲染需要的数据。

.. _request_mapping:
请求映射
-----------


@RequestMapping
^^^^^^^^^^^^^^^^^



.. _argument_resolver:

Controller方法的参数解析
^^^^^^^^^^^^^^^^^^^^^^^^^^^
#. 用Model 不用 ModelAndView
#. 

.. _media_negotiation:

.. _controller_advice:

.. _model_attribute:

RequestToViewNameTranslator
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

@ModelAttribute
^^^^^^^^^^^^^^^^^


.. _init_binder:

@InitBinder
^^^^^^^^^^^^^
给你一个机会初始化 ``WebDataBinder`` 以用于构建 ``@RequestMapping`` 方法的参数.

* 哪些 ``@InitBinder``方法会被调用:

  * ``@ControllerAdvice`` 中的 ``@InitBinder`` 方法会先被调用;
  * 然后是 Controller 的中 ``@InitBinder`` 方法;
  * 最后是 父类中的 ``@InitBinder`` 方法.

* 何时调用: ``@ModelAttribute`` 方法之后, ``@RequestMapping`` 方法之前
* 调用次数: 被标为InitBinder的方法, 会为每次请求的每个 ``@RequestParam`` 参数，或 ``Command`` 复杂类型参数，调用一次; 如果 Command 的 ModelAttribute 的名字与请求中的参数名一样，则会被调用2次。

.. _exception_resolvers:

.. _exception_handler:

@ExceptionHandler
^^^^^^^^^^^^^^^^^^

.. _model:

Model
-------

.. _command:

Command
^^^^^^^^^^
除 Web Service 以外，不用简单类型作为 Controller 方法的参数，尽量用复杂类型的参数，即 Command 。这是因为，

#. 复杂类型的参数会自动加入Models以便在模板中使用，不需要手动添加；
#. 复杂类型参数方便增加通用的验证，jsr303；
#. 复杂类型参数可以更方便的使用默认值。

.. code-block:: java

    @RequestMapping("trade-created")
    public void findTradesByCreated(Date from, Date to) { // Bad
    }

    @RequestMapping("trade-created2")
    public void findTradesByCreated2(TradeSearch tradeSearch) {// Good
    }

    @RequestMapping("trade-created3")
    public void findTradesByCreated3(@ModelAttribute("ts")TradeSearch tradeSearch) { 
        // Good
    }

.. _path_varaible:

@PathVariable
^^^^^^^^^^^^^^^^^^^^

.. _type_conversion:

类型转换
^^^^^^^^^^


.. _validator:

Validator
^^^^^^^^^^

.. _annotations:

Annotations
^^^^^^^^^^^^^^
* @RequestParam

.. _view:

View
------



.. _conclusion:

总结
-------
学习和掌握spring mvc，可以从spring mvc提供的注解入手。了解了一下注解的用法，就会用spring mvc了。

* @EnableWebMVC
* @Controller 
* @RequestMapping
* 入参类注解

  * @PathVariable
  * @RequestParam
  * @SessionParam
  * @CookieParam
  * @RequestBody
  * @MatrixVariable

* 返回值类注解
  
  * @ResponseBody
  * @ModelAttribute

* 框架类注解

  * @ExceptionHandler
  * @ControllerAdvice

.. author:: default
.. categories:: none
.. tags:: none
.. comments::
