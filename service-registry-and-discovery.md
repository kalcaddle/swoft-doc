# 服务注册

服务启动后，自动注入到第三方集群工具里面，目前只支持RPC服务注册。目前支持 Consul , 其它可以自行实现接口。

config/beans/base.php 配置

```php
return [

    // ...

    'application' => [
        // ...
        'useProvider' => false, // 是否开启服务注册，且开启RPC内部服务才有效
    ],

    // ...
];
```
config/beans/service.php 配置
```php
 'consulProvider'       => [
        'class' => \swoft\service\ConsulProvider::class,
        'address' => '127.0.0.1:80', // 配置consul服务地址
    ],
    "userPool"           => [
        "class"           => \swoft\pool\ServicePool::class,
        "uri"             => '127.0.0.1:8099,127.0.0.1:8099',
        "maxIdel"         => 6,
        "maxActive"       => 10,
        "timeout"         => '${config.service.user.timeout}',
        "balancer"        => '${randomBalancer}',
        "serviceName"     => 'user',
        "useProvider"     => false,
        'serviceprovider' => '${consulProvider}' // 引用服务注册与发现器
    ],
```



