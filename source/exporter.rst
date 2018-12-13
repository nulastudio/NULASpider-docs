.. _how-to-use-exporter:

**********
使用导出器
**********

.. _how-to-import-and-use-exporter:

注入和使用导出器
================

.. code-block:: php

    $config = [
        'export'              => [
            // 程序根据type匹配出合适的导出器
            'type'  => 'excel',
            // 其余参数根据每个导出器的不同具体配置
            'file'  => DIR_DATA . '/blog.xlsx',
            'sheet' => 'sheet name',
        ],
    ];

    $spider = new Spider($config);

    // 可以同时注入多个导出器，并不会造成冲突
    // 但不建议全都写上，因为导出器可能会注册钩子拖慢系统运行（尽管注册的钩子只是空运行）
    $spider->use(User\Plugins\ExcelExporter::class);
    $spider->use(User\Plugins\PrintOutExporter::class);
    $spider->use(User\Plugins\PDOExporter::class);

    $spider->start();


.. _exporter-vs-on-export:

导出器 VS on_export
===================

导出器实际上也是通过 `on_export` 回调函数完成数据导出入库的，这也就意味着导出器和 `on_export` 回调函数你只能二选一，不能同时使用，在导出器能满足的情况下请尽量少用 `on_export` 回调函数。

.. hint:: 当多个导出器使用了同一个 `type` 时，最后注入的导出器将会被使用。

.. _handle-unsupported-data:

处理复合数据
============

默认的，程序会将数组类型的数据使用 `json_encode` 进行序列化导出，此行为可能会被具体的导出器所改变。
