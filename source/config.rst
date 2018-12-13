.. _config:

******
config
******

thread
======

指定多线程并发请求以及处理的线程数。默认为5。个人建议值：CPU核心数 × 2 ± 1。对于使用者来说，程序已在各个回调调用中加入锁机制，确保在单位时间内有且只有一个线程进入，使用者不必在回调函数内考虑多线程的安全问题。

UI
==

是否使用控制台UI，默认为使用。在特殊时候可不用，比如在调试阶段使用调试导出器。

logger
======

用于程序中写入日志的 `Logger <https://github.com/php-fig/log/blob/master/Psr/Log/AbstractLogger.php>`_ ，接受任意符合 `PSR-3 <https://www.php-fig.org/psr/psr-3/>`_ 标准的 `Logger 对象 <https://github.com/php-fig/log/blob/master/Psr/Log/AbstractLogger.php>`_ 。默认为 :doc:`NullLogger <api/nulastudio/Log/NullLogger>` ，既不做任何日志记录。

scan_urls
=========

指定爬虫入口的URL匹配规则，可指定多条规则。详细配置规则： :doc:`URL匹配模式 <url_pattern>`。

list_url_pattern
================

指定列表页的URL匹配规则，可指定多条规则。详细配置规则： :doc:`URL匹配模式 <url_pattern>`。

content_url_pattern
===================

指定内容页的URL匹配规则，可指定多条规则。详细配置规则： :doc:`URL匹配模式 <url_pattern>`。

fields
======

指定内容页中要抓取的数据。详细配置： :doc:`fields <fields>`。

export
======

进行数据导出。数组中 ``type`` 为必需字段，用于匹配适合的Exporter，其余的字段根据具体的Exporter来配置。

.. _config_example:

配置示例
========

.. code-block:: php

    <?php

    use nulastudio\Log\FileLogger;

    return [
        'thread'              => 5,
        'logger'              => new FileLogger(DIR_LOG . '/' . date('Y-m-d') . '.log'),
        'scan_urls'           => [
            'https://www.liesauer.net/blog/',
        ],
        'list_url_pattern'    => [
            '#^https://www.liesauer.net/blog/page/\d+/$#',
        ],
        'content_url_pattern' => [
            '#^https://www.liesauer.net/blog/post/.*?.html$#',
        ],
        'fields'              => [
            'title'   => [
                'type'     => 'css',
                'selector' => '.post-title>a',
            ],
            'meta'    => [
                'type'     => 'css',
                'selector' => '.post-meta',
                'fields'   => [
                    'author' => [
                        'type'     => 'css',
                        'selector' => 'li:nth-child(1)>a',
                    ],
                    'time'   => [
                        'type'     => 'css',
                        'selector' => 'li:nth-child(2)>time',
                    ],
                    'author' => [
                        'type'     => 'css',
                        'selector' => 'li:nth-child(3)>a',
                    ],
                ],
            ],
            'content' => [
                'type'     => 'css',
                'selector' => '.post-content>.md_content>textarea',
            ],
        ],
        'export'              => [
            'type'     => 'print',
        ],
    ];

