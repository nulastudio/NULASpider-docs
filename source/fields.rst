.. _fields:

******
fields
******

fields目前支持以下形式的选择器抓取数据。
    - 简单xpath
    - 数组
    - 简单回调

简单xpath
=========

选取第一条与xpath匹配的数据。

.. code-block:: php

    <?php

    $config['fields'] = [
        'title' => "//*[contains(@class,'title')]/text()",
    ];

数组
====

数组形式支持最完整的选择器以及参数控制。

source参数
----------

可选。当设置了source参数，$content将被source参数的结果覆盖，source参数支持以下类型：
    - Request对象
    - Response对象
    - callback

当设置为Request对象时，尝试对Request发起请求，并使用返回的Response的内容作为$content。额外的可设置is_ajax参数以及auto_referer参数。

当设置为Response对象时，尝试使用Response的内容作为$content。

当设置为有效的callback时，尝试使用回调返回的值作为$content。参数接受4个参数：
    - $spider，Spider对象。
    - $content，原content内容。
    - $request，当前的Request对象。
    - $response，当前的Response对象。

is_ajax参数
-----------

可选。仅当source参数设为callback时有效， :code:`is_ajax` 为 :code:`true` 时，尝试对Request对象设置一个 :code:`X-Requested-With: XMLHttpRequest` 请求头。

auto_referer参数
----------------

可选。仅当source参数设为callback时有效， :code:`auto_referer` 为 :code:`true` 时，尝试将当前的 :code:`$request` 的url设置为Request对象的 :code:`Referer` 。

type参数
--------

必需。支持 :code:`xpath`  :code:`css`  :code:`regex`  :code:`callback`  :code:`raw` ，默认为 :code:`xpath` 。当设为 :code:`raw` 时，直接将当前内容作为数据返回，若设置了source，则使用source的结果。

.. warning:: 并不支持jquery选择器，jquery选择器与css选择器是有差别的。

selector参数
------------

可选。具体的选择器。仅当type为 :code:`raw` 时可省略。当设为 :code:`callback` 时，回调接受4个参数：
    - $content，从该内容中选取数据。
    - $spider，Spider对象。
    - $request，当前的Request对象。
    - $response，当前的Response对象。

callback参数
------------

可选。用于对选取的数据进行进一步处理，回调接受4个参数并返回处理后的数据：
    - $field，抓取到的数据。
    - $spider，Spider对象。
    - $request，当前的Request对象。
    - $response，当前的Response对象。

fields参数
----------

可选。从当前的字段中递归选取数据。fields的结构完全一致。

repeated参数
------------

可选。表示当前的字段是重复的。抓取到的字段会转为数组。

简单回调
========

选取由回调返回的数据。回调接受4个参数：
    - $content，从该内容中选取数据。
    - $spider，Spider对象。
    - $request，当前的Request对象。
    - $response，当前的Response对象。

.. code-block:: php

    <?php

    $config['fields'] = [
        'meta' => function($content, $spider, $request, $response) {
            $json = json_decode($content, true);
            return [
                'author' => $json['data']['author'],
                'category' => $json['data']['category'],
                'publish_time' => $json['data']['publish_time'],
                'comments_count' => $json['data']['comments_count'],
            ];
        },
    ];
