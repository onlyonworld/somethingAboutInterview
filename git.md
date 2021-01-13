# github关联本地仓库

1、本地先安装好`Git Bash`并打开；

2、利用指令输入自己的用户名和邮箱(用户名和邮箱是注册`GitHub`时输入的用户名和邮箱)

```js
$ git config --global user.name "yourname"
$ git config --global user.email "youremail"
```

3、设置`SSH key`

先检查本机中是否已经生成密钥了：

```js
$ cd ~/.ssh
```

如果没有报：`xxx/.ssh: No such file or directory`，则说明本机已经有生成了，使用`ls`就可以查看密钥的位置；

如果报了`No such file or directory`，则使用

```js
$ ssh-keygen -t rsa -C "yourname"
```

来生成一个密钥，生成过程中按3次回车键就好（默认路径，默认没有密码登录），生成成功后，去对应默认路径里用记事本打开`id_rsa.pub`，得到ssh key公钥。

4、为`github`账号配置SSH key

回到个人`github`账号的首页，点击右上角用户头像下的小三角，找到`settings`，在右侧菜单栏中找到`SSH and GPG keys`，选择`new SSH key`，输入`title`，这个title可以随便输入你喜欢的，下面`key`的内容就是本机`ssh key`公钥，直接将`id_rsa.pub`中的内容粘贴过来就可以，然后点击下面的`add SSH key`即可完成。

5、创建本地仓库

先新建一个文件夹，使用`git init`初始化文件夹，生成一个隐藏的文件夹`.git`。

使用指令将文件添加到代码仓科中

```js
$ git status         // 查看哪些文件还没有进入缓存区，红色的文件是未进入缓存区的，绿色的是进入缓存区，但没有提交到本地仓库的
$ git add .          // 添加当前文件夹下的所有文件
$ git add -A         // 添加当前文件夹下的所有文件
$ git add **.md      // 添加当前文件夹下的**.md这个文件
```

6、使用指令提交代码，同时给本次提交的代码作一个说明

```js
$ git commit -m "本次提交代码的说明"
```

7、在`github`账户中新建一个`repository`，复制仓库地址，通过指令将本地仓库与`github`上的仓库连接起来

```js
$ git remote add origin addressofrepository
```

如果github上已经存在，可以执行

```js
$ git remote rm origin
```

将github上的仓库先删掉，，然后再次执行

```js
$ git remote add origin addressofrepository
```

8、将本地代码提交到`github`上

```js
$ git push origin master
```

9、删除github中仓库的某个文件

```js
$ git rm -r --cached filename // filename是所要删除的文件名，包括其后缀名。cached是不会把本地的文件删掉
$ git commit -m "本次提交代码的说明"
$ git push -u origin master
```

10、删除github中的仓库

1）进入github主页，选择进入要删除的仓库

2）点击settings选项中的options选项

3）在options的最后面有一个Danger Zone，选择delete this repository