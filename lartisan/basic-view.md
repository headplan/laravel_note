# Basic View

**重写layouts/app.blade.php**

> 参考bulma的文档 - [http://bulma.io/documentation/overview/start/](http://bulma.io/documentation/overview/start/)

```css
# Navbar
# 目前使用旧版的nav,重写了部分样式,添加了个jquery事件
# 主要还是重写layout中的nav布局及样式
<nav class="nav has-shadow">
    <div class="container">
        <div class="nav-left">
            <a href="{{ route('home') }}" class="nav-item">
                <img src="{{ asset('images/lartisan-logo.png') }}" alt="Lartisan">
            </a>
            <a class="nav-item is-tab is-hidden-mobile is-active m-l-10">专栏</a>
            <a class="nav-item is-tab is-hidden-mobile">视频</a>
            <a class="nav-item is-tab is-hidden-mobile">问答</a>
        </div>
        <span class="nav-toggle">
            <span></span>
            <span></span>
            <span></span>
        </span>
        <div class="nav-right" style="overflow: visible;">
            @if (Auth::guest())
                <a href="" class="nav-item is-tab">登录</a>
                <a href="" class="nav-item is-tab">注册</a>
                @else
                <button class="dropdown is-aligned-right nav-item is-tab">
                    彼得赫德王子 <span class="icon"><i class="fa fa-caret-down"></i></span>
                    <ul class="dropdown-menu">
                        <li>
                            <a href="#">
                                <span class="icon"><i class="fa fa-fw m-r-5 fa-user-circle-o"></i></span>
                                <span style="vertical-align: middle">个人信息</span>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <span class="icon"><i class="fa fa-fw m-r-5 fa-bell"></i></span>
                                <span style="vertical-align: middle">消息</span>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <span class="icon"><i class="fa fa-fw m-r-5 fa-cog"></i></span>
                                <span style="vertical-align: middle">设置</span>
                            </a>
                        </li>
                        <li class="separator"></li>
                        <li>
                            <a href="#">
                                <span class="icon"><i class="fa fa-fw m-r-5 fa-sign-out"></i></span>
                                <span style="vertical-align: middle">退出</span>
                            </a>
                        </li>
                    </ul>
                </button>
            @endif
        </div>
    </div>
</nav>
# 重构样式时再更新5.0.1版本的navbar样式
```

**使用bulma重写登录和注册**

还有其中的重置密码的模板 .

修改resources/assets/sass/\_variables.scss文件中的变量 , 可以使用默认的也可以自定义 , 或者添加Bulma的变量 .

修改背景色

```
$body-background-color: #f5f5f5;
```

添加了助手在helpers.scss

其他就是重写布局了 , login.blade.php完成 . 布局中用到了一个vue组件 , 是buefy的一个组件 , value传入true是选中 , 记住登录状态那里之后需要修改一下 . 然后是重写注册register , email , reset页面 , 暂时不添加任何新东西 , 只是替换模板 .

重置密码的邮件暂时使用了 , mailtrap.io测试 . 

