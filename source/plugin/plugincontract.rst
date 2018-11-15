.. _plugincontract:

**************************************************
nulastudio\\Spider\\Contracts\\PluginContract 接口
**************************************************

.. _methods:

方法
====

+----------+----------------------------------------------------------+------+
| 返回类型 | 签名                                                     | 说明 |
+==========+==========================================================+======+
| void     | public static function install($application, ...$params) | 无   |
+----------+----------------------------------------------------------+------+

install方法参数说明
===================

    - $application，注入的程序，既当前的Spider对象。
    - $params可变参数数组，当使用$spider->use()时传进来的参数。

举个栗子

.. code-block:: php

    <?php

    $spider->use(User\Plugins\Foo::class, 'Hello', 'World');

此时，$params参数为

.. code-block:: text

    [
        'Hello',
        'World',
    ]

.. hint:: 所有的插件都必须实现这个接口！

源代码解析

.. code-block:: php

    <?php

    namespace nulastudio\Spider\Contracts;

    interface PluginContract
    {
        public static function install($application, ...$params);
    }

