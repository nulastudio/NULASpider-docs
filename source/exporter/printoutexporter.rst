.. _printout-exporter:

**********
调试导出器
**********

.. _config:

配置参数
========

+--------+--------+-------+
| 参数名 | 类型   | 说明  |
+========+========+=======+
| type   | string | print |
+--------+--------+-------+

.. _usage:

使用说明
========

将数据使用 `print_r` 打印到控制台中，一般在调试阶段使用。

.. hint:: 由于控制台会实时刷新UI界面，所以使用调试导出器时需要将 `UI` 配置设为 `false`，不然是无法看到输出的。

