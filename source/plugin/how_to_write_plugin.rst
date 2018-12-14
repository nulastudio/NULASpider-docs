.. _how-to-write-plugin:

********
编写插件
********

下面教大家怎么编写一个 `Foo` 插件，以便大家了解插件编写的基本过程。

首先我们创建一个 `Foo.php` 文件，放于程序目录的 `User/Plugins` 下，程序启动时会自动引用插件文件。

插件需要存在于 `User\\Plugins` 命名空间内，并且实现 `nulastudio\\Spider\\Contracts\\PluginContract` 接口。

然后我们需要使用 `bind` 方法将我们的功能绑定到程序中。

.. code-block:: php

    <?php

    namespace User\Plugins;

    use nulastudio\Spider\Contracts\PluginContract;

    class Foo implements PluginContract
    {
        public static function install($application, ...$params)
        {
            $application->bind('foo', function($application, ...$params) {
                echo "this is my first plugin.\n";
                var_dump($params);
            });
        }
    }

这样我们的插件就已经编写完毕了，可以在程序中使用了。下面的程序中当爬虫启动时就会输出我们插件的内容。

.. code-block:: php

    <?php

    use nulastudio\Spider\Spider;

    // 配置
    $config = [];

    $spider = new Spider($config);

    $spider->on_start = function ($spider) {
        $spider->foo('Hello', 'World');
    };

    $spider->use(User\Plugins\Foo::class);

    $spider->run();
