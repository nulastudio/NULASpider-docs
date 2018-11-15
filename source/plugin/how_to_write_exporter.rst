.. _how_to_write_exporter:

************
编写Exporter
************

下面教大家怎么编写一个写文件的Exporter，以便大家了解Exporter编写的基本过程。

首先我们创建一个 `WriteToFileExporter.php` 文件，Exporter需要放于 `User\\Exporters` 命名空间中，并且实现 `nulastudio\\Spider\\Contracts\\ExporterContract` 接口。

.. code-block:: php

    <?php

    namespace User\Exporters;

    use nulastudio\Spider\Contracts\PluginContract;

    class WriteToFileExporter implements ExporterContract
    {
        private $file;

        public function __construct(array $config = []) {
            $this->file = $config['file'];
        }

        public function export($data)
        {
            file_put_contents($this->file, implode(', ', $data) . "\n", FILE_APPEND | LOCK_EX);
        }
    }

此外，我们还需要编写一个 `WriteToFileExporter` 插件来使用我们的Exporter，不然Exporter是无法单独使用的。

.. code-block:: php

    <?php

    namespace User\Plugins;

    use nulastudio\Spider\Contracts\PluginContract;
    use User\Exporters\WriteToFileExporter as WTFExporter;

    class WriteToFileExporter implements PluginContract
    {
        public static function install($application, ...$params)
        {
            // 注册到 file 类型的Exporter
            $application->registerExporter('file', WTFExporter::class);
            // 此外，如果Exporter需要额外的资源释放的话，可以注册beforeExit钩子，用于释放。
            // 需要自己额外的在Exporter添加dispose逻辑。
            $application->hooks['beforeExit'] += [function ($spider, $exit_code) {
                $exporter = $spider->getExporter();
                if ($exporter !== null && $exporter instanceof WTFExporter) {
                    $exporter->dispose();
                }
            }];
        }
    }


这样我们的Exporter就已经编写完毕了，可以在程序中使用了。下面的程序中会将我们抓取到的数据写到对应的文件中。

.. code-block:: php

    <?php

    use nulastudio\Spider\Spider;

    // 配置
    $config = [
        'export'=>[
            'type' => 'file',
            'file' => 'path/to/file',
        ],
    ];

    $spider = new Spider($config);

    $spider->use(User\Plugins\WriteToFileExporter::class);

    $spider->run();


