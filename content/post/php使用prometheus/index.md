---
title: "php使用prometheus"
description: 
date: 2022-12-13
draft: false
tags: ["prometheus", "php"]
categories: ["PHP"]
---
## 使用场景
-   长期趋势分析：通过对监控样本数据的持续收集和统计，对监控指标进行长期趋势分析。例如，通过对磁盘空间增长率的判断，我们可以提前预测在未来什么时间节点上需要对资源进行扩容。
-   对照分析：两个版本的系统运行资源使用情况的差异如何？在不同容量情况下系统的并发和负载变化如何？通过监控能够方便的对系统进行跟踪和比较。
-   告警：当系统出现或者即将出现故障时，监控系统需要迅速反应并通知管理员，从而能够对问题进行快速的处理或者提前预防问题的发生，避免出现对业务的影响。
-   故障分析与定位：当问题发生后，需要对问题进行调查和处理。通过对不同监控监控以及历史数据的分析，能够找到并解决根源问题。
-   数据可视化：通过可视化仪表盘能够直接获取系统的运行状态、资源使用情况、以及服务运行状态等直观的信息。

## 如何使用
### 引入软件包
`composer require promphp/prometheus_client_php`

### 封装公共类
```php
use Prometheus\CollectorRegistry;
use Prometheus\Storage\Redis;
use Prometheus\RenderTextFormat;

class Prometheus
{
    public static $registry;
    public static $renderer;
    public static $counter;

    public function __construct()
    {
        // 连接redis
        $adapter = new Redis();
        self::$registry = new CollectorRegistry($adapter);
        self::$renderer = new RenderTextFormat($adapter);
    }

    // 打点方法
    public function registerCounter($prometheus)
    {
        if (!empty($prometheus['action'])) {
            $namespace = $prometheus['namespace'];
            $name = $prometheus['name'];
            self::$counter = self::$registry->registerCounter($namespace, $name, 'it increases', ['type']);
            if (is_array($prometheus['action'])) {
                foreach ($prometheus['action'] as $v) {
                    // 将统计结果增加1
                    self::$counter->incBy(1, [$v]);  
                }
            } else {
                // 将统计结果增加1
                self::$counter->incBy(1, [$prometheus['action']]);  
            }
        }
    }

    // 输出结果给prometheus服务
    public function RenderTextFormat(){
        $result = self::$renderer->render(self::$registry->getMetricFamilySamples());
        header('Content-type: ' . RenderTextFormat::MIME_TYPE);
        return $result;
    }
}
```

### 示例
下面代码表示给登录成功次数进行打点记录
```php
$prometheus = new Prometheus();
$pthParam = [
    'action' => 'loginSuccess', // 要打点的数据
    'namespace' => 'service', // 命名空间
    'name' => 'loginApi', // 名称
];
$prometheus->registerCounter($pthParam);
```

## 扩展阅读
1. [Prometheus简介 · Prometheus中文技术文档](https://www.prometheus.wang/quickstart/why-monitor.html) 
2. [第1章 天降奇兵 - prometheus-book (gitbook.io)](https://yunlzheng.gitbook.io/prometheus-book/parti-prometheus-ji-chu/quickstart)
