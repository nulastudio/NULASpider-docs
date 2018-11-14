.. _callback:

********
回调函数
********

回调函数概览
    - `on_start`_
    - `on_exit`_
    - `on_error`_
    - `on_exception`_
    - `on_request`_
    - `on_status_code`_
    - `on_process`_
    - `on_scan_url`_
    - `on_list_url`_
    - `on_content_url`_
    - `on_fetch_field`_
    - `on_fetch_page`_
    - `on_export`_
    - `requestOverride`_
    - `findUrlsOverride`_

on_start
========

在爬虫程序启动时执行，可在此做一些初始化工作，比如设置全局默认的Header或者Cookie。回调接受一个参数，不接受任何返回：
    - $spider，当前的爬虫程序。

on_exit
=======

在爬虫程序终止时执行，可做一些资源释放工作。当爬虫程序发生错误或者抛出未捕获的异常时也会调用此回调。回调接受两个参数，不接受任何返回：
    - $spider，当前的爬虫程序。
    - $exit_code，退出状态码，正常退出时为0。

on_error
========

在爬虫程序发生错误时执行。回调接受六个参数，接受布尔类型的返回值：
    - $spider，当前的爬虫程序。
    - $errno，错误码。
    - $errstr，错误信息。
    - $errfile，报错文件。
    - $errline，报错行数。
    - $errcontext，报错附加信息数组，包含了当前报错位置作用域中的变量。

当回调返回 `true` 时，爬虫程序将尝试继续运行下去，当发生了 `Error` 级别的时候不应该强行继续运行，对于 `NOTICE` 以及 `WARNING` 级别的错误，一般不会导致程序崩溃，但可能会导致运行结果异常。

on_exception
============

在爬虫程序抛出未捕获的异常时执行。回调接受两个参数，接受布尔类型的返回值：
    - $spider，当前的爬虫程序。
    - $ex，异常。

当回调返回 `true` 时，爬虫程序将尝试继续运行下去。不应强行继续运行！不应强行继续运行！不应强行继续运行！

on_request
==========

在爬虫程序抓取到一个页面时执行。回调接受三个参数，接受布尔类型或者Response类型的返回值：
    - $spider，当前的爬虫程序。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

当回调返回 `false` 时，将放弃抓取当前的页面。当回调返回 `Response` 对象时，将使用返回的 `Response` 对象进行数据抓取。

on_status_code
==============

在爬虫程序抓取到一个页面后执行，在 `on_request` 回调之后执行。一般用于判断反爬状态。回调接受四个参数，接受布尔类型或者Response类型的返回值：
    - $spider，当前的爬虫程序。
    - $status_code，当前请求返回的状态码。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

当回调返回 `false` 时，将放弃抓取当前的页面。当回调返回 `Response` 对象时，将使用返回的 `Response` 对象进行数据抓取。

on_process
==========

在爬虫程序抓取到一个页面后执行，在 `on_status_code` 回调之后执行。一般用于处理特殊情况或者特殊的反爬技术，比如网页反爬虫了但同时又返回了 `200` 状态码，这时候我们就要从内容中分析出是否被反爬了。回调接受四个参数，接受布尔类型的返回值：
    - $spider，当前的爬虫程序。
    - $url，当前请求的url。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

当回调返回 `false` 时，将放弃抓取当前的页面。不返回或返回非 `false` 时继续抓取当前页面。

on_scan_url
===========

在爬虫程序匹配到入口url时执行。回调接受四个参数，接受布尔类型的返回值：
    - $spider，当前的爬虫程序。
    - $url，匹配到的入口url。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

当回调返回 `true` 时，将url添加到待爬队列中，否则放弃抓取此url。

on_list_url
===========

在爬虫程序匹配到列表页时执行。回调接受四个参数，接受布尔类型的返回值：
    - $spider，当前的爬虫程序。
    - $url，匹配到的列表页url。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

当回调返回 `true` 时，将url添加到待爬队列中，否则放弃抓取此url。

on_content_url
==============

在爬虫程序匹配到内容页时执行。回调接受四个参数，接受布尔类型的返回值：
    - $spider，当前的爬虫程序。
    - $url，匹配到的内容页url。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

当回调返回 `true` 时，将url添加到待爬队列中，否则放弃抓取此url。

on_fetch_field
==============

在爬虫程序抓取到一个字段时执行。回调接受三个参数，必须返回处理过后的字段值：
    - $spider，当前的爬虫程序。
    - $name，抓取到的字段名。
    - $field，抓取到的字段值。

.. hint:: 此回调会在两种情况下执行

    - 非嵌套数据时，每一个字段都会执行。
    - 为嵌套数据时，具体字段将不会执行，但每一层的嵌套都会执行一次。

举个例子：

.. code-block:: php

    <?php

    $config['fields'] = [
        // 非嵌套数据，执行
        'title'    => [
            'type'     => 'css',
            'selector' => '.post-title>a',
        ],
        // 嵌套数据并且非最底层，执行
        'meta'     => [
            'type'     => 'css',
            'selector' => '.post-meta',
            'fields'   => [
                // 嵌套数据但已具体到字段，不执行
                'author' => [
                    'type'     => 'css',
                    'selector' => 'li:nth-child(1)>a',
                ],
                // 嵌套数据但已具体到字段，不执行
                'time'   => [
                    'type'     => 'css',
                    'selector' => 'li:nth-child(2)>time',
                ],
                // 嵌套数据但已具体到字段，不执行
                'author' => [
                    'type'     => 'css',
                    'selector' => 'li:nth-child(3)>a',
                ],
            ],
        ],
        // 非嵌套数据，执行
        'content'  => [
            'type'     => 'css',
            'selector' => '.post-content>.md_content>textarea',
        ],
        // 非嵌套数据，执行
        'comments' => [
            'type'     => 'css',
            'selector' => '#comments>.comment-list>li',
            'repeated' => true,
        ],
    ];


.. hint:: 当字段设置了 `repeated` 属性时，$field是一个数组。

.. hint:: 当字段为嵌套数据时，$field仍保持着嵌套格式。

on_fetch_page
=============

在爬虫程序抓取完一个页面的字段时执行。一般在此进行数据后处理以及清洗。回调接受四个参数，接受一个返回值：
    - $spider，当前的爬虫程序。
    - $fields，当前页面抓取到的字段数组。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

当回调返回 `false` 时，将丢弃当前页面以及抓取到的字段，否则接受返回的字段数组。

on_export
=========

在爬虫程序抓取完一个页面的字段后入库执行。一般在此进行自定义的入库。回调接受五个参数，不接受返回值：
    - $spider，当前的爬虫程序。
    - $export，export配置数组。
    - $fields，当前页面抓取到的字段数组。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。

.. warning:: 当设置了 `on_export` 回调时，程序中的所有Exporter将失效，因为Exporter正是使用 `on_export` 回调实现的，也就意味着您需要自己根据具体的 `export` 配置来进行入库操作。


requestOverride
===============

特殊回调。使用自定义的请求方式覆盖系统默认的请求方式，在执行 `beforeRequest` 钩子后执行。回调接受两个参数，必须返回 `null` 或一个 `Response` 对象：
    - $spider，当前的爬虫程序。
    - $request，原Request请求对象。

findUrlsOverride
================

特殊回调。使用自定义的url抓取方式覆盖系统默认的url抓取方式。程序默认抓取所有a标签的href，如果此行为并不符合你当前的情况则可进行自定义抓取。回调接受四个参数，必须返回抓取到的url数组：
    - $spider，当前的爬虫程序。
    - $content，当前页面内容。
    - $request，当前的Request请求对象。
    - $response，当前的Response响应对象。
