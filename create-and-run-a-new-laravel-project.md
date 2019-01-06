## 将项目代码提交到 Github 仓库

首先在 Github 上创建一个仓库

我创建的 Github 仓库为：https://github.com/xinyflove/laravel-blog

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

