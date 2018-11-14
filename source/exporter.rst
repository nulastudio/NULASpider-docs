********
Exporter
********

.. toctree::

   ExcelExporter <exporter/excelexporter>
   PrintOutExporter <exporter/printoutexporter>
   PDOExporter <exporter/pdoexporter>

Exporter的使用
==============

首先Exporter也是通过插件机制加载到爬虫程序中的，所以使用Exporter前需要先加载Exporter插件（在启动爬虫之前加载即可）。

.. code-block:: php

    // 用到哪个加载哪个
    $spider->use(User\Plugins\ExcelExporter::class);
    $spider->use(User\Plugins\PrintOutExporter::class);


这样子在config里配置export参数才能使用对应的Exporter。

.. code-block:: php

    $config['export'] = [
        'type'  => 'excel',
        'file'  => 'path/to/excel/document',
        'sheet' => 'SpiderData',
    ];


Exporter的原理
==============

Exporter插件在爬虫启动加载时会向爬虫程序注册一个Exporter Type，然后会占用 `on_export` 回调函数，当爬虫抓取完页面数据时会通过type参数找到适合的Exporter并使用export配置进行懒加载，最后调用Exporter的export方法进行数据的导出入库。

.. hint:: `on_export` 回调以及Exporter只能二选一，不能同时使用，因为两者都需要占用 `on_export` 回调函数

.. hint:: 当多个Exporter注册同一个Exporter Type时，后注册的Exporter会覆盖前者。

