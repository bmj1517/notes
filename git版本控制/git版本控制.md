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





## git命令

```python
# 初始化
git init

# 检测当前文件夹下的文件状态
git status

# 管理文件或文件夹
git add 文件名或文件夹名
git add .      # 将文件夹下所有的文件都管理起来

# 生成版本
git commit -m '描述'

# 查看版本记录
git log
```

