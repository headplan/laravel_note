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

**版本控制**

```
$ git init
$ git add -A
$ git status
$ git commit -m "Initial commit"
```

**配置hosts**

```
192.168.66.66 lartisan.example
```

**.env 文件**

```
APP_NAME=Lartisan example
APP_URL=http://lartisan.example

DB_DATABASE=lartisan_example

MAIL_USERNAME=...
MAIL_PASSWORD=...
```

**.editorconfig文件**

添加.editorconfig文件

```
$ git add -A
$ git commit -m "Add .editorconfig"
```



