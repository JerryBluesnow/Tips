# Tips for git usage

## global and local configuration for account
+ 全局配置
```
  git config --global user.name “github’s Name”
  git config --global user.email “github@xx.com”
  git config --list
```

+ 单独项目配置
```
  git config user.name "JerryBluesnow"
  git config user.email "bbluesnow@126.com"
  git config --list
```

## Git 修改远端仓库地址
```
  方法有三种：
  1.修改命令
  git remote set-url origin [url]

  例如：git remote set-url origin gitlab@gitlab.chumob.com:php/hasoffer.git

  2.先删后加

  git remote rm origin
  git remote add origin [url]
  3.直接修改config文件
```

## 配置
### 显示所有的配置
```
git config --list
```

### 添加全局配置
```
 git config --global user.name yxibng
 git config --global user.email yxibng@gmail.com
```

### 为本地的工程里面添加单独的配置,可以使用不同的用户名和密码

```
 git config --local user.name xxx
 git config --local user.email xxx@xxx.com

```
## 比较两次commit修改的文件列表
```
git diff --name-only <commit-id-1> <commit-id-2>
```
```
exp:
mengalong@along-mac:~/code/git/bter$ git diff --name-only 7fa56f9c83429bc564e6d123498b14aae5c390b1 45eadc1bb962ff4d49c7c5dbf298ddb41664dd28
ChangeLog
bter/fork_pdb.py 
```

## 获取历史所有的修改记录
```
git log
```

## 单行显示历史所有的commit
```
git log --pretty=oneline
```
```
exp:
mengalong@along-mac:~/code/git/bter$ git log --pretty=oneline
ea8a0f7eb5af01c6daf718029ede465b0911dda5 (HEAD -> master) modify the publish policy
7fa56f9c83429bc564e6d123498b14aae5c390b1 (origin/master) add fork_pdb for debug the project
45eadc1bb962ff4d49c7c5dbf298ddb41664dd28 modify the insert function in impl_mysql
637a829011a3d3b7bfdf1ce24299f631a8e7741a update Changelog info
5c78ccbab445e4ba0575133d8ee68e69df72a786 (tag: 0.0.2) update README.md for database
0eab7e5214d5302d8ee6fe01b3e28837fb57c781 update the Changelog
390b1555af4373a2abe0ee243230b21cdd7ff655 add dist and bter.egg.* into .gitignore
f1ffcd3ef07ec8cfe00a03f286ef3d6c8486bc31 add mysql-sync for table create
```
## 查看指定文件所有的历史修改
```
git log <file-path>
```
## 单行显示指定文件历史所有commit
```
git log --pretty=oneline <file-path>
```

```
exp:
mengalong@along-mac:~/code/git/bter/bter$ git log --pretty=oneline utils.py
8160a4919e75420d32234ec136fe77e5f5281888 implement period task
6a0e823b446a2e88adeec37bae11b45928e80f85 add conf parser and license header
bc5e932862f25688c09f0c90df74d505efb5bcc3 init the bter demo
```

## 更换远端repo

`git remote set-url origin git@github.com:User/UserRepo.git`

## submodule 

```
git submodule init

git submodule update
```

[Git中submodule的使用](https://zhuanlan.zhihu.com/p/87053283)

[使用Git Submodule管理子模块](https://segmentfault.com/a/1190000003076028)

## 使用Git pull文件时，出现"error: RPC failed; curl 18 transfer closed with outstanding read data remaining"
```
error: RPC failed; curl 18 transfer closed with outstanding read data remaining
fatal: The remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed
出现以上错误有以下原因

1.缓存区溢出curl的postBuffer的默认值太小，需要增加缓存
使用git命令增大缓存（单位是b，524288000B也就500M左右）

git config --global http.postBuffer 524288000
使用git config --list查看是否生效

此时重新克隆即可

2.网络下载速度缓慢
修改下载速度

git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999
3.以上两种方式依旧无法clone下，尝试以浅层clone，然后更新远程库到本地
git clone --depth=1 http://xxx.git
git fetch --unshallow
```
## git查看当前commit修改的文件列表
```
git diff --name-only HEAD~ HEAD

git diff --name-only <commit compare1> <compare2>

gdiff 63e3b647d55fcc643e793e650c893be8601719b1 548cdaf01dbc2f08d1ca0b697a24afe512b75a2f --stat

git log 查看commit的历史
git show <commit-hash-id>查看某次commit的修改内容
git log -p <filename>查看某个文件的修改历史
git log -p -2查看最近2次的更新内容
```
