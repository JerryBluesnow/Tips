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

## git remote set-url origin git@github.com:User/UserRepo.git



## git submodule init
## git submodule update

https://zhuanlan.zhihu.com/p/87053283

https://segmentfault.com/a/1190000003076028