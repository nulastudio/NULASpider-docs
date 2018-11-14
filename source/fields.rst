.. _fields:

******
fields
******

fields目前支持以下形式的选择器抓取数据。通过灵活配置可配置出强大的抓取规则。
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

当设置为有效的callback时，尝试使用回调返回的值作为$content。回调接受4个参数：
    - $content，原$content内容。
    - $spider，Spider对象。
    - $request，当前的Request对象。
    - $response，当前的Response对象。

is_ajax参数
-----------

可选。仅当source参数设为Request对象时有效， :code:`is_ajax` 为 :code:`true` 时，尝试对Request对象设置一个 :code:`X-Requested-With: XMLHttpRequest` 请求头。

auto_referer参数
----------------

可选。仅当source参数设为Request对象时有效， :code:`auto_referer` 为 :code:`true` 时，尝试对Request对象设置一个 :code:`Referer` 请求头。

type参数
--------

必需。支持以下类型：
    - :code:`xpath`
    - :code:`css`
    - :code:`regex`
    - :code:`callback`
    - :code:`raw`

默认为 :code:`xpath` 。当设为 :code:`raw` 时，直接将当前$content作为抓取数据返回，若设置了source参数，则使用source参数所返回的结果。

.. warning:: 并不支持jquery选择器，jquery选择器与css选择器是有差别的。

selector参数
------------

可选。具体用于抓取数据的选择器。仅当type参数为 :code:`raw` 时可省略。当设为 :code:`callback` 时，回调接受4个参数：
    - $content，从该内容中选取数据。
    - $spider，Spider对象。
    - $request，当前的Request对象。
    - $response，当前的Response对象。

callback参数
------------

可选。用于对选取后的数据进行后处理，回调接受4个参数并返回处理后的数据：
    - $field，抓取到的数据。
    - $spider，Spider对象。
    - $request，当前的Request对象。
    - $response，当前的Response对象。

.. warning:: 当设置了repeated参数时，$field的类型应该是抓取到的字段数组，其他情况下$field的类型视具体的source参数、type参数以及selector参数而定。


fields参数
----------

可选。从当前的字段中递归选取子级数据。fields的结构完全一致。

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
