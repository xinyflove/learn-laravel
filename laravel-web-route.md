Web路由入口在 `routes/web.php` ，用于处理终端用户通过 Web 浏览器直接访问的请求。

## 定义一个简单的路由

定义路由最简单的方式就是在`routes/web.php`中定义一个路径以及一个映射到该路径的[闭包函数](http://php.net/manual/zh/class.closure.php)：

```
// routes/web.php 
Route::get('/', function () { 
    return 'Hello, World!'; 
});
```

这样，当我们访问应用首页`laravel_blog.test`时，就可以看到页面显示`Hello, World!`这一行字符串。这就是一个最简单的 Laravel 路由定义，但是涵盖了一个 Web 框架的基本功能：处理请求，返回响应。

