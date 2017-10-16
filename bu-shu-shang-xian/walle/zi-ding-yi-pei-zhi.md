# 自定义配置

config/params.php 配置

```
// *******操作日志目录*******
'log.dir' => '/tmp/walle/',

// *******Ansible Hosts 主机列表目录*******
'ansible_hosts.dir' => realpath(__DIR__ . '/../runtime') . '/ansible_hosts/',

// *******指定公司邮箱后缀*******
'mail-suffix' => [
    'walle-web.io', # 支持多个
],
```



