.. _how-to-write-exporter:

**********
编写导出器
**********

下面教大家怎么编写一个写文件的导出器，以便大家了解导出器编写的基本过程。

首先我们创建一个 `WriteToFileExporter.php` 文件，放于程序目录的 `User/Exporters` 下，程序启动时会自动引用导出器文件。

导出器需要存在于 `User\\Exporters` 命名空间内，并且实现 `nulastudio\\Spider\\Contracts\\ExporterContract` 接口或继承于 `nulastudio\\Spider\\Contracts\\AbstructExporter` 抽象类。

.. code-block:: php

    <?php

    namespace User\Exporters;

    use nulastudio\Spider\Contracts\ExporterContract;

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

此外，我们还需要编写一个 `WriteToFileExporter` 插件来使用我们的导出器，不然导出器是无法单独使用的。

.. code-block:: php

    <?php

    namespace User\Plugins;

    use nulastudio\Spider\Contracts\PluginContract;
    use User\Exporters\WriteToFileExporter as WTFExporter;

    class WriteToFileExporter implements PluginContract
    {
        public static function install($application, ...$params)
        {
            // 注册到 file 类型的导出器
            $application->registerExporter('file', WTFExporter::class);
            // 此外，如果导出器需要额外的资源释放的话，可以注册 beforeExit 钩子，用于释放。
            // 需要自己额外的在导出器添加释放逻辑。
            $application->hooks['beforeExit'][] = function ($spider, $exit_code) {
                $exporter = $spider->getExporter();
                if ($exporter !== null && $exporter instanceof WTFExporter) {
                    // $exporter->dispose();
                }
            };
        }
    }


这样我们的导出器就已经编写完毕了，可以在程序中使用了。下面的程序中会将我们抓取到的数据写到对应的文件中。

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
