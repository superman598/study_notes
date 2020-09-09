# 笔记

## 前面的话
1. 编辑器              -> VSCode (https://code.visualstudio.com/)
2. 课堂文字互动        ->  QQ 群 (听懂 扣1 不懂 扣2)  
3. 视频和作业发送方式   -> QQ 群
4. 加我 QQ(779498590)  或 微信(xiaohigher)  备注格式 BJ200622-张岩

## 卸载密码管理工具
```
git credential-manager uninstall
```

## Git 介绍
Git 就是一个应用程序

## 安装的注意事项
1. 尽量到官网下载
2. 安装尽量放到非 C 盘, 路径『尽量不要使用中文』

## .swp
这个文件是 vim 生成的临时文件, 编辑完成之后, 会自动销毁.

## 复制粘贴快捷键
* ctrl + insert   复制
* shift + insert  粘贴

## 软件推荐
* utools  快速启动本地应用程序  alt + space   *****
* processon.com   在线图像绘制               *****
* clover  标签页形式的资源管理                *****
* typora  markdown 编辑器                    *****
* https://www.faceyourmanga.com/   在线头像制作

## 如果在命令行被困住了
* ctrl + c
* 输入  q

## 关于 git status 输出颜色
* 红色  表示此修改只存在于『工作区』
* 绿色  表示此修改存在于 『工作区和暂存区』

## Git 忽略文件小结
* 编辑器生成的配置文件(文件夹)
* 临时文件
* 多媒体文件  mp3  mp4
* 第三方模块 (node_modules)  npm 生成的安装第三方模块的文件夹

## 做冲突练习的时候
注意: 一定要在 master 分支上进行至少一次的提交.



## git remote 

是对远程仓库( URL )的管理.

```
git remote add origin https://github.com/xiaohigh2/second-resp.git
```

* git           git 应用程序命令
* remote        运行参数, 『遥远的』
* add           添加. 添加远程 URL 的别名
* URL           仓库 URL 地址

其他命令

* git remote -v  查看别名配置
* git remote remove 删除别名
* git rename old new  重命名

## git push

作用: 将本地仓库的『某个分支』推送到远程仓库的『某个分支』

```
git push -u origin master
```

* git           应用程序的命令
* push          推送. 『推』
* -u            分支关联.  master  <==>  master   好处是以后再向 master 分支进行推送的时候, 可以直接使用『git push』
* origin        远程仓库的『别名』
* master        本地的分支名

```
//将本地的 A 分支推送到远程仓库的 B 分支
git push origin A:B
```

## 对于克隆的仓库

默认有存在一个叫做『origin』的别名配置, url就是被克隆仓库的 URL, 而且两个分支是关联的

## 提交习惯

git push 提交前要使用 git pull 做更新

## 远程中心仓库

不一定是 GitHub 仓库地址. 

## Git 常见错误

```
fatal: not a git repository (or any of the parent directories): .git
```

命令行所在位置不是一个 git 仓库

解决方法: 检查命令所在的位置, 是否已经进行过初始化或者克隆

#### Git提交远程仓库中容易遇到的问题

![image-20200909192129582](C:%5CUsers%5CAdministrator%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200909192129582.png)

如果新建的远程仓库不是空的，例如你勾选了add a README.md 。 那么你通过命令

`git push -u origin master`，就是会报上图中出现的问题。这是因为你新建仓库中的那个README文件不在本地仓库中，所以要先进行合并`git pull --rebase origin master`,进行合并之后，本地的仓库和新建的仓库中的内容就保持一致了。这时再进行`git push -u origin master`就可以完成推送了。









## 注意点

1. 分支切换
2. 版本回退
3. 代码推送

在这三个操作之前, 一定要将当前分支代码进行提交. (git add -A   git commit )

## Git 常用命令小结

* git init      初始化
* git add -A    暂存区添加
* git commit    代码提交, 创建新历史
* git status    查看仓库的状态   红色 表示修改只在工作区, 绿色 表示修改在工作区和暂存区都存在
* git branch        查看分支
* git branch name   分支创建
* git checkout  name 切换分支
* git merge  name   分支合并
* git clone     克隆仓库
* git push      推送仓库
* git pull      更新
* git remote    远程仓库别名管理
* 忽略文件配置

## Git 仓库是不能嵌套的

## CDN

Content Delivery  network

## vs简便操作

1. 多选 alt + 鼠标左键 创建多个光标
2. 快捷键 ctrl + d 对相同内容进行选取
3. ctrl + shift + d 快速复制单行
4. alt + shift + f 格式化代码

## win10 自带剪切板

非常实用的工具  win + v.  QQ输入法自带剪切板.

## UI 框架复制技巧

看到某个效果, 然后使用审核工具审核元素, 然后选中元素 `ctrl + c` 即可

## holder.js 图片占位插件  (https://github.com/imsky/holder)

1. 引入 js 文件   

```
<script src="https://cdn.bootcdn.net/ajax/libs/holder/2.9.3/holder.min.js"></script>
```

2. 使用标签创建图片    

```
<img data-src="holder.js/200x200?bg=908&text=helou">  固定尺寸
<img data-src="holder.js/100px250?bg=#678" alt="">    固定比例
```

## 字体图标

* https://fontawesome.com/  
* iconfont

## vscode 代码片段功能

1. 点击左下角『齿轮』
2. 点击『用户代码片段』
3. 点击『新建代码片段』
4. 在弹出的输入框中『输入文件名』
5. 编辑文件内容

```js
"Print to console": {
    "scope": "javascript,typescript,html,css",     //生效的文件类型
    "prefix": "h5200622",                           //简写
    "body": [
        "<script src=\"https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js\"></script>"  //完整结果
    ],
    "description": "Log output to console"
}
```

6. 保存

