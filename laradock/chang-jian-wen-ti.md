# 常见问题

**Larave安装后页面空白**

```
# 尝试
sudo chmod -R 777 storage bootstrap/cache
```

**看到了Nginx的欢迎页面 , 而不是Laravel**

```
# 使用
http://127.0.0.1
# 而不是
http://localhost
```

**提示端口被占用**

停掉占用端口服务 , 或换个端口

**服务中的时间与当前时间不匹配**

配置时区

重建服务

```
docker-compose up -d --build <services>
```

**MySQL拒绝连接**

错误原因可能是Laravel程序没有在容器本机也就是127.0.0.1上运行 , 解决步骤 : 

1.在Laravel中打印IP地址

```
dd(Request::ip())
```

2.修改Laravel的DB\_HOST配置的ip地址 , 或者修改DB\_HOST值为MySQL容器的名字

