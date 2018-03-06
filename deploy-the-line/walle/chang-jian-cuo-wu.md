# 常见错误

#### 检测、上线出错

检测、上线出错，想知道到底发生了什么事情？我能告诉你的就是，有些错误walle捕捉不到，默认操作日志在`/tmp/walle/`下，具体可在`config/local.php`里`log.dir`配置路径，`tail -f /tmp/walle/walle-YYYYmmdd.log`着日志，部署看日志。

**一切检测、上线的错误均可被发现和解决。**

#### 上线至全量更新服务器时出错：`mv: 无法以非目录来覆盖目录 -fT /a/b/c /d/e/f`

原因分析：更新目标机群是以软链方式来更新webroot，如果提前在目标机群创建了webroot目录，软链覆盖将会失败。

解决办法：直接删除目标机群webroot目录:`rm -rf /d/e/f`，确定其父目录有读写的权限即可，由瓦力系统生成webroot软链接。

#### 更多帮助查看

http://www.walle-web.io/docs/troubleshooting.html

