现在已经有了很多，关于如何开发 Laravel 扩展包的文章。但是大多文章写的太过片面，不够完整，而且我在实际进行开发扩展包的时候，还是遇到了很多的问题，我把自己的开发经验，以及遇到的问题记录下来，分享给大家。

## 一、扩展包开发

### 1. 创建新项目，初始化扩展包配置

首先创建一个新的 Laravel 项目，创建新项目参照 [创建并运行一个新的 Laravel 项目](/create-and-run-a-new-laravel-project.md)。

接下来在此项目中，在根目录创建目录`packages/{your_name}/{your_package_name}`，我创建的目录为 `packages/peak-xin/package-test` 。

进入扩展包目录\(`packages/peak-xin/package-test`\)，初始化 composer 配置

```
composer init
```

![](/assets/QQ截图20190213172912.png)

执行之后，项目下生成一个`composer.json`文件：

```
{
    "name": "peak-xin/package-test",
    "description": "这是一个简单测试开发Laravel扩展包",
    "license": "MIT",
    "authors": [
        {
            "name": "Peak Xin",
            "email": "xinyflove@sina.com"
        }
    ],
    "require": {},
}
```

### 2. 创建扩展包基本目录、文件

一般情况下，我们会创建以下文件和目录:

```
peak-xin/package-test
├── src  #存放扩展包所有的逻辑代码
├── tests # 存放测试用例
├── README.md
├── composer.json
└── LICENSE
```

### 3. 修改扩展包 composer 配置

然后，修改此**扩展包**中的`composer.json`文件，设置一下`composer`的自动加载配置、以及扩展包的命名空间。

```
{
    ...,
    "autoload": {
        "psr-4": {
            "PeakXin\\PackageTest\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "PeakXin\\PackageTest\\Tests\\": "tests/"
        }
    },
    ...
}
```

### 4. 编写扩展包逻辑代码

接下来，我们来创建`PackagetestServiceProvider.php`、`TestAdmin.php`文件。

`PackagetestServiceProvider.php`文件

```
<?php

namespace PeakXin\PackageTest;

use Illuminate\Support\ServiceProvider;

class PackagetestServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap services.
     *
     * @return void
     */
    public function boot()
    {
        //
    }

    /**
     * Register services.
     *
     * @return void
     */
    public function register()
    {
        $this->app->singleton('test-admin', function () {
            return new TestAdmin;
        });
    }
}
```

`TestAdmin.php`文件

```
<?php

namespace PeakXin\PackageTest;

class TestAdmin
{
    public function printRunning()
    {
        echo 'running';
    }
}
```

此时，这个扩展包算是开发好了，接下来我们开始进行本地安装、测试。

## 二、扩展包本地测试

把`PackagetestServiceProvider`添加到项目的 config/app.php`providers`数组中

```
'providers' => [
    ...,
    PeakXin\PackageTest\PackagetestServiceProvider::class,
],
```

再修改项目下的 `composer.json` 文件

```
{
    "require": {
        ...,
        "peak-xin/package-test": "dev-master"
    },
    ...,
    "autoload": {
        ...,
        "psr-4": {
            ...,
            "PeakXin\\PackageTest\\": "packages/peak-xin/package-test/src/"
        }
    },
    ...
}
```

运行命令：

```
composer dump-autoload
```

最后，修改一下`routes/web.php`文件：

```
Route::get('/', function () {
    app('test-admin')->printRunning();
});
```

此时，我们打开浏览器访问此项目，显示`running`，恭喜你，成功了!

## 三、扩展包发布

扩展包开发、测试完成之后，这个时候就可以发布到`Packagist`。

### 1. 提交代码到 GitHub

首先，在 GitHub 上创建存放扩展包的版本库，记录下 GitHub 版本库的地址。

![](/assets/QQ截图20190213163723.png)

然后把扩展包的代码提交到 GitHub 上。

### 2. 把扩展包发布到 Packagist

然后，访问 [Packagist 官网](https://packagist.org/packages/submit)，登录后，点击右上角Submit按钮，进入发布向导，此时，将 GitHub 版本库的地址填写至`Repository URL`输入框中:![](/assets/20190213162514.png)然后点击Check按钮:

![](/assets/20190213162640.png)

然后点击 Submit 提交按钮，一切顺利，可以看到发布成功。![](/assets/20190213163030.png)

### 3. 设置版本信息

版本默认是`dev-master`，Composer 包的版本号会从 Git 的 tag 中同步过来。

```
git tag 1.0.0
git push --tag
```

扩展包刚发布，此时安装，可能会报找不到安装包的错误，需要稍等一下服务器同步，一般不过超过 3-5 分钟，如果一切正常，会看到版本提示，安装成功！

