# Git 命令

#### 初始化

```git init```

.gitignore  //按具体项目和个人需要编写

```git add 文件路径```  //在commit之后会清空

```git status```  //查看状态

```git commit -m "description"```  //提交到本地.git文件夹下

```git branch -M main```  //设置主分支为main(github默认main为主分支)

```git remote remove origin```  //删除旧origin(如果origin已定义)

```git remote add origin https://github.com/Caspian-G/仓库名.git```  //给远程仓库起名



#### Push

###### 法一：

```git push -u origin main```  //push到origin仓库的main分支中(之后直接push即可)

###### 法二：

```git push https://github.com/Caspian-G/仓库名.git main```  //临时push



#### Pull

(本地已有项目 只需pull更新)

```git fetch origin```  //从远程仓库下载最新变化 只更新本地远程跟踪分支 不影响工作目录

```git checkout main```  //切换到本地main分支

```git pull origin main```  //"git fetch"和"git merge"的组合 git merge将更新合并到工作目录

######  如果提示冲突可强制覆盖

```git reset --hard origin/main```

###### 或

```git stash```  //本地变化暂存

```git push origin main```

```git stash pop```  //重新应用暂存变化



#### Clone

(本地无项目 全部clone)

```git clone origin 项目名```

















