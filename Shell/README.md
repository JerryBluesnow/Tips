# Shell Tips

- [getopt：命令行选项、参数处理](http://blog.csdn.net/tdmyl/article/details/24714297)
- [expect shell发送组合键](http://blog.csdn.net/iodoo/article/details/49175707)
- [组合键ASCII Table](http://blog.csdn.net/iodoo/article/details/49175749)

# 让 Shell 命令提示符显示 Git 分支名称
function git-branch-name {
  git symbolic-ref HEAD 2>/dev/null | cut -d"/" -f 3
}
function git-branch-prompt {
  local branch=`git-branch-name`
  if [ $branch ]; then printf " [%s]" $branch; fi
}
PS1="\u@\h \[\033[0;36m\]\W\[\033[0m\]\[\033[0;32m\]\$(git-branch-prompt)\[\033[0m\] \$ "