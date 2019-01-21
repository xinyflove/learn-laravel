> 假设已经搭建好 Laravel 本地开发环境。

## 1. 创建一个新的 Laravel 项目

有两种方式可以创建一个新的 Laravel 项目，这两种创建方式都是从命令行执行的：

第一种是通过全局的 Laravel 安装器；

另一种是通过 Composer 的`create-project`命令 \[\[Composer的安装和使用\]\(javascript:alert%28'筹划中'%29;\)\]。

### 1.1 使用 Laravel 安装器安装

安装 Laravel 安装器很简单，在命令行执行以下命令即可（如果已经安装过，会自动进行更新）：

```bash
composer global require laravel/installer
```

安装完成后，后续就可以通过`laravel new [项目名称]`来创建新的 Laravel 项目了：

```
laravel new blog
```

该命令会在当前目录下创建一个新的名为`blog`的应用。

### 1.2 使用 Composer create-project 命令安装

除此之外，还可以通过 Composer 自带的`create-project`命令来安装新应用：

```bash
composer create-project laravel/laravel blog --prefer-dist
```

效果和上面使用安装器安装的一样，使用这个方式安装的一个好处是可以**安装旧版本**的 Laravel 项目，比如要安装 5.6 版本的项目`blog56`，可以这么做：

```bash
composer create-project laravel/laravel blog56 5.6.* --prefer-dist
```

> 注意：如果Composer安装失败，先更新Composer `composer self-update` 试试。

---

## 2. Laravel 应用的目录结构

### 安装完成后，我们来看一下新安装 Laravel 项目`blog`的目录结构：![](/assets/laravel_dir.png)2.1 目录

根目录默认包含一下一级子目录：

app：存放应用核心代码，如模型、控制器、命令、服务等

* bootstrap：存放 Laravel 框架每次启动时用到的文件
* config：用于存放项目所有配置文件
* database：存放数据库迁移和填充类文件
* public：Web 应用入口目录，用于存放入口文件`index.php`及前端资源文件（CSS、JS、图片等）
* resources：用于存放与非 PHP 资源文件，如视图模板、语言文件、待编译的 Vue 模板、Sass、JS 源文件
* routes：项目的所有路由文件都定义在这里
* storage：用于存放缓存、日志、上传文件、已经编译过的视图模板等
* tests：存放单元测试及功能测试代码
* vendor：通过 Composer 安装的依赖包都存放在这里，通常该目录会放到`.gitignore`文件里以排除到版本控制系统之外

> 注：更多关于目录结构的信息，可参考[官方文档](https://laravelacademy.org/post/9529.html)。

### 2.2 文件

* `.env.example`/`.env`：用于配置环境变量，`.env.example` 是一个示例模板，而`.env` 是真正的配置文件，由于包含敏感信息，通常也将其放到 `.gitignore` 文件中。
* `artisan`：允许你在项目根目录下通过 `php artisan` 执行 Artisan 命令
* `.gitignore` 和`.gitattributes`：Git 配置文件
* `composer.json` 和 `composer.lock`：Composer 配置文件
* `webpack.mix.js`：Laravel Mix Webpack 配置文件，用于编译和打包前端资源
* `package.json`：配置前端资源依赖和脚本（类似于 `composer.json` 之于 PHP）
* `phpunit.xml`：PHPUnit 配置文件
* `server.php`：用于通过 `php artisan serve` 启动 PHP 内置服务器进行一些简单的本地预览
* `yarn.lock`：类似于 `composer.lock` 之于 Composer，指定 NPM 包版本
* `.editorconfig`：用于在不同 IDE 或编辑器中维护代码风格的一致性

---

## 3. 配置

Laravel 应用的一些核心配置，比如数据库、队列、邮件等，都位于`config`目录下，通过配置文件名称就可以很直观地甄别出不同的服务配置。这些配置文件都会返回一个数组，数组中的每个值都可以通过配置键获取（配置键以配置文件名为前缀，以「.」号分隔数组层级），例如，如果你在`config/services.php`中定义了如下配置：

```php
// config/services.php 
return [
    'sparkpost' => [
        'secret' => env('SPARKPOST_SECRET'),
    ],
];
```

然后，你就可以通过`config('services.sparkpost.secret')`来访问配置值。

如上例所示，所有的因环境而异的变量配置值（尤其是敏感信息）都应该存放到根目录下的`.env`环境变量文件中：

```
SPARKPOST_SECRET = xyj_laravelacademy.org
```

然后在配置文件中通过`env()`辅助函数传入键名`SPARKPOST_SECRET`来获取，这样做有两个好处：一是将敏感信息存放到版本控制系统（如 Git、Svn）之外，提高了系统的安全性；此外还可以方便我们在不同环境中（每个环境有自己独立的`.env`文件）使用不同的配置值，提高了代码的复用性和灵活性。

> 注：更多配置信息请参考[官方文档](https://laravelacademy.org/post/9528.html#toc_2)。

---

## 4. 运行

安装好 Laravel 项目，了解了目录结构及其作用，以及如何对项目进行配置后，我们就可以运行这个应用了，启动方式因开发环境而异，我们以 phpStudy 为例，通过配置项目域名为`laravel_blog.test`，在浏览器中访问`http://laravel_blog.test`，即可看到应用首页：![](/assets/laravel_home.png)

---

## 5. 将项目代码提交到 Github 仓库

首先在 Github 上创建一个仓库

我创建的 Github 仓库为：[https://github.com/xinyflove/laravel-blog](https://github.com/xinyflove/laravel-blog)

以下是关联本地分支到 Github 项目主干并第一次提交代码的示例操作：

```bash
cd blog
git init
git remote add origin https://github.com/xinyflove/laravel-blog
git add .
git commit -m '新建一个Laravel项目'
git branch --set-upstream-to=origin/master master
git pull origin master --allow-unrelated-histories
git push
```

如果在执行命令 `git branch --set-upstream-to=origin/master master` 提示如下错误：

```
$ git branch --set-upstream-to=origin/master master
error: the requested upstream branch 'origin/master' does not exist
hint:
hint: If you are planning on basing your work on an upstream
hint: branch that already exists at the remote, you may need to
hint: run "git fetch" to retrieve it.
hint:
hint: If you are planning to push out a new local branch that
hint: will track its remote counterpart, you may want to use
hint: "git push -u" to set the upstream config as you push.
```

则进行如下命令：

```
git push --set-upstream origin master
```

之后会提示输入 Github 的帐号密码，如果没有错误提示，等待代码上传到 Github 仓库即可。

这样，就可以在 Github 上看到刚刚提交的代码了：

![](/assets/QQ截图20190107123946.png)

---

## 6. 测试

Laravel 开箱提供了基于 PHPUnit 进行单元测试和功能测试的功能，并且为我们做好了基础配置（`phpunit.xml`）和示例代码（位于`tests`目录下），由于本节并没有编写任何代码，所以可以通过以下命令运行示例测试：

```
./vendor/bin/phpunit
```

![](/assets/QQ截图20190107124219.png)

---



