.. _plugin-contract:

**************************************************
nulastudio\\Spider\\Contracts\\PluginContract 接口
**************************************************

.. _methods:

方法列表
========

+----------+----------------------------------------------------------+------+
| 返回类型 | 签名                                                     | 说明 |
+==========+==========================================================+======+
| void     | public static function install($application, ...$params) |      |
+----------+----------------------------------------------------------+------+

.. _more-about-install-params:

install方法参数说明
===================

    - `$application` ，注入的程序，既当前的Spider对象。
    - `$params` 可变参数数组，当使用 `$spider->use()` 时传进来的参数。

举个栗子

.. code-block:: php

    $spider->use(User\Plugins\Foo::class, 'Hello', 'World');


此时，$params参数为

.. code-block:: text

    [
        'Hello',
        'World',
    ]

.. hint:: 所有的插件都必须实现这个接口！
