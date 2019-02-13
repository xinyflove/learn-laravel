现在已经有了很多，关于如何开发 Laravel 扩展包的文章。但是大多文章写的太过片面，不够完整，而且我在实际进行开发扩展包的时候，还是遇到了很多的问题，我把自己的开发经验，以及遇到的问题记录下来，分享给大家。

## 扩展包开发

### 1. 创建新项目，初始化扩展包配置

首先创建一个新的 Laravel 项目，创建新项目参照 [创建并运行一个新的 Laravel 项目](/create-and-run-a-new-laravel-project.md)。

接下来在此项目中，在根目录创建目录`packages/{your_name}/{your_package_name}`，我创建的目录为 `packages/peak-xin/package-test` 。

进入扩展包目录，初始化 composer 配置

![](/assets/QQ截图20190213172912.png)



## 扩展包发布

扩展包开发、测试完成之后，这个时候就可以发布到`Packagist`。

### 1. 提交代码到 GitHub

首先，在 GitHub 上创建存放扩展包的版本库，记录下 GitHub 版本库的地址。

![](/assets/QQ截图20190213163723.png)

然后把扩展包的代码提交到 GitHub 上。

### 2. 把扩展包发布到 Packagist

然后，访问 [Packagist 官网](https://packagist.org/packages/submit)，登录后，点击右上角Submit按钮，进入发布向导，此时，将 GitHub 版本库的地址填写至`Repository URL`输入框中:![](/assets/20190213162514.png)然后点击Check按钮:

![](/assets/20190213162640.png)

然后点击 Submit 提交按钮，一切顺利，可以看到发布成功。![](/assets/20190213163030.png)

