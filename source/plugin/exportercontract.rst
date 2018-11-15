.. _exportercontract:

****************************************************
nulastudio\\Spider\\Contracts\\ExporterContract 接口
****************************************************

.. _methods:

方法
====

+----------+---------------------------------+-----------------------------------------------+
| 返回类型 | 签名                            | 说明                                          |
+==========+=================================+===============================================+
| void     | __construct(array $config = []) | $config参数会接受到config配置中export参数数组 |
+----------+---------------------------------+-----------------------------------------------+
| void     | public function export($data)   | $data参数为抓取到的整个页面的字段数组         |
+----------+---------------------------------+-----------------------------------------------+

.. hint:: 所有的Exporter都必须实现这个接口！

源代码解析

.. code-block:: php

    <?php

    namespace nulastudio\Spider\Contracts;

    interface ExporterContract
    {
        public function __construct(array $config = []);
        public function export($data);
    }
