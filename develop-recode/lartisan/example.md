# 例子

例子项目在`Project/Lartisan_example`文件夹中 .

**基本配置**

```
folders:
 - map: ~/Project/Lartisan_example
   to: /home/vagrant/Example
sites:
 - map: lartisan.example
   to: /home/vagrant/Example/public
databases:
 - lartisan_example
```

项目运行在Homestead中 , 启动进入 : 

```
> cd ~/Homestead && vagrant up
> vagrant ssh
$ cd ~/Example
$ composer create-project laravel/laravel Laravel --prefer-dist "5.5.*"
```



