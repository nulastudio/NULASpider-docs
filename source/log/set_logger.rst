.. _set_logger:

***********
设置 Logger
***********

config配置中设置
================

直接设置config配置的logger参数。

.. code-block:: php

    <?php

    $config = [
        'logger' => new XXXLogger(),
    ];

动态更改设置
============

创建爬虫程序后任意时刻调用 `$spider->setLogger(new XXXLogger)` 即可更改Logger。不建议随便更改Logger。

.. warning:: 仅支持符合 `PSR-3 <https://www.php-fig.org/psr/psr-3/>`_ 标准的 `Logger <https://github.com/php-fig/log/blob/master/Psr/Log/AbstractLogger.php>`_ 。

