## 1.注册账号

## 2.创建仓库

## 3.本地代码推送到云仓库

```python
git remote add origin 地址       # 连接github仓库,只有第一次时需要连接
git push -u origin master       # 推送到github, origin是别名,可以自己定义
```

## 4.拉取

```python
git clone 地址    # 克隆仓库,仅第一次时需要
git pull origin master    # 拉取master分支
```

## 代码review

```python
配置
# Settings -> Branches -> Add rule
```

# 给开源软件贡献代码

```python
1.fork源代码
	将别人的源代码拷贝到我自己的远程仓库
2.在自己仓库进行修改代码
3.给源代码的作者提交修复bug的申请(pull request)
```

# 配置

```python
1.项目配置文件: 项目.git/config
    git config --local user.name ''
    git config --local user.email ''

2.全局配置文件: ~/.gitconfig
    git config --global user.name ''
    git config --global user.email ''
    
3.系统配置文件: /etc/.gitconfig
    git config --system user.name ''
    git config --system user.email ''
    注: 需要有root权限
```

# 免密登录

```python
URL中体现
	修改地址: https://用户名:密码@github.com/......

                
SSH实现
	1.生成公钥和私钥
    	ssh-keygen
    2.拷贝公钥的内容,并设置到github中
    3.在git本地中配置ssh地址
```

# 任务管理

```python
1. issues  提问问题,文档以及任务管理
2. wiki   项目的文档
```



