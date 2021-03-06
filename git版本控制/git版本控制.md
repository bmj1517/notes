## git安装

```python
linux系统
# sudo yum -y install git

window系统
安装包安装
```

## 配置邮箱和用户名

```python
git config --global user.email "you@example.com"
git config --global user.name "your name"
```

## git使用步骤

版本控制 ---> 让git管理文件夹

```python
"""
1.进入要管理的文件夹
2.初始化    git init
3.管理     git add
4.生成版本  git commit 
3,4 循环执行
"""
```

## git文件的三种状态

```python
红色:
    新增的文件/修改后的文件  git add 文件名
绿色:
    git已经管理起来的文件   git commit -m '描述'
生成版本:
    git status 
```

## git三大区域

```python
工作区, 暂存区, 版本库
```

## git回滚

```python
回滚至之前版本
"""
git log
git reset --hard 版本号  
"""

回滚至之后版本
"""
git reflog
git reset --hard 版本号
"""
```

## git分支

```python
git branch 分支名    创建新分支
git checkout dev    切换到新分支
git branch -d  分支名  删除分支
```

## git合并

```python
git checkout master    切换回master
git merge 分支名        合并分支
有冲突的话就手动解决冲突,然后在add和commit
```

## git工作流

```python
--master(正式版)--c1----c3    主线
	--dev(开发版)--c2---c3    分支
```



## git命令

```python
# 初始化
git init

# 检测当前文件夹下的文件状态
git status

# 管理文件或文件夹   提交到暂存区
git add 文件名或文件夹名
git add .      # 将文件夹下所有的文件都管理起来

# 生成版本  提交到版本库
git commit -m '描述'

# 查看版本记录
git log
git reflog

# 版本回滚
git reset --hard 版本号

# 暂存区 回撤至 未暂存状态
git reset HEAD 文件名

# 未暂存状态 回撤至 未修改状态
git checkout  -- 文件名

# 版本库  回撤至 暂存区状态
git reset --soft 版本号  ???

# 查看当前分支名
git branch

# 创建新分支
git branch 分支名
```

