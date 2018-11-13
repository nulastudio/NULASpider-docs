.. _url_pattern:

***********
URL匹配模式
***********

URL目前支持以下匹配模式，通常地，完全匹配模式多用于匹配入口URL，正则匹配模式和回调匹配模式多用于匹配列表URL以及内容URL。
    - 完全匹配模式
    - 正则匹配模式
    - 回调匹配模式

当指定多条匹配项时，只需满足其中一条即可

.. _string_pattern:

完全匹配模式
============
指定一条匹配式，若待匹配的URL与匹配式完全一致则成功匹配。

.. code-block:: php

    <?php

    $config['scan_urls'] => [
        'http://www.example.com/entrance',
        'http://www.example.com/entrance2',
    ];

.. _regex_pattern:

正则匹配模式
============
指定一条正则匹配式，若待匹配的URL符合正则匹配式则成功匹配。

.. code-block:: php

    <?php

    $config['list_url_pattern'] => [
        '#^http://www.example.com/page/\d+$#',
        '#^http://www.example.com/post/.*?.html$#',
    ];

.. _callback_pattern:

回调匹配模式
==============
指定一条回调匹配式，若回调函数返回 :code:`true` 则成功匹配，回调函数接受一个字符串参数，其值为待匹配URL。

.. code-block:: php

    <?php

    $config['content_url_pattern'] => [
        function($url) {
            return strpos($url, 'something') !== false;
        },
    ];
