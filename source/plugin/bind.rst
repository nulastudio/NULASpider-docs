.. _bind:

*********
bind 方法
*********

+----------+-----------------------------------------------+----------+
| 返回类型 | 签名                                          | 说明     |
+==========+===============================================+==========+
| void     | public function bind(string $name, $provider) | 动态绑定 |
+----------+-----------------------------------------------+----------+

`bind` 方法是实现插件机制的核心方法。通过该方法，你可以将任意类型的东西动态绑定到程序中。

.. hint:: 为防止过多的对象绑定导致内存剧增，可以使用匿名函数包装起来进行懒创建，用到的时候按需创建。


.. code-block:: php

    <?php

    $spider->bind('Foo', 'Bar');

    // print out "Bar"
    var_dump($spider->Foo);


对于 `callable` 类型的绑定，可直接调用，且程序会自动将自身注入到第一个参数中。

.. code-block:: php

    <?php

    $spider->bind('Foo', function($application, ...$params) {
        var_dump($application);
        var_dump($params);
    });

    // print out Spider Object, "Hello", "World"
    var_dump($spider->Foo('Hello', 'World'));

但如果先取出来再调用，需要手动注入$application参数。

.. code-block:: php

    <?php

    $spider->bind('Foo', function($application, ...$params) {
        var_dump($application);
        var_dump($params);
    });

    $foo = $spider->Foo;
    // print out Spider Object, "Hello", "World"
    var_dump($foo($spider, 'Hello', 'World'));

懒创建栗子

.. code-block:: php

    <?php

    class Foo
    {
        function echo () {
            echo "Hello World!\n";
        }
    }

    $spider->bind('foo', function () {
        static $foo;
        if (!$foo) {
            $foo = new Foo;
        }
        return $foo;
    });

    // 只有在调用foo方法时，Foo对象才会被创建，由于使用了static，该对象只创建一次
    $spider->foo()->echo();

