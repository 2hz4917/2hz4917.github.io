---
title: Hexo使用方法
date: 2019-10-26 22:17:11
tags:
- Hexo
- 技巧
---

# 最近重新开始写起Blog，用的还是之前的框架，快速生成博客--Hexo，距离上次部署到Github已经过去很久了，所以为了防止自己下一次部署的时候，忘记操作步骤，在这记录一下使用方法。

写在正文前，由于Hexo版本更新问题，可能会有部分方法不适用所有情况。

## 正文开始

### 为了更加方便的管理Hexo的源文件和目标文件，我们这里采用了创建两个分支的方法。一个分支用来存放Hexo生成的网站原始的文件，另一个分支用来存放生成的静态网页。

-- 具体操作步骤
1. 创建一个 [yourname.github.io](https://github.com/2hz4917/2hz4917.github.io)
2. 再创建一个hexo分支，并将其设置为默认分支（master分支默认已存在）
3. git clone https://github.com/2hz4917/2hz4917.github.io.git 克隆仓库
4. git checkout hexo 切换到hexo分支，依次执行cnpm i hexo、hexo init、cnpm i 和 cnpm i hexo-deployer-git
5. 修改_config.yml中的deploy字段，内容如下
```javascript
- type: git
    repo: https://github.com/2hz4917/2hz4917.github.io.git
    branch: master
- type: git
    repo: https://github.com/2hz4917/2hz4917.github.io.git
    branch: hexo
    extend_dirs: /
    ignore_hidden: false
    ignore_pattern:
    public:
```
6. git push origin hexo 执行命令提交文件到远程
7. hexo g -d 执行命令生成静态网站（会自动部署到master分支）
8. 补充：hexo g -d 可以分解为 hexo g，hexo d。hexo d就是把public文件夹下的文件替换掉master的文件
### 以上这些操作就基本完成了hexo博客的部署

## 这里是[Hexo主题](https://hexo.io/themes/) 的使用，找了一个ACE的主题，[链接在此](https://github.com/kinggozhang/hexo-theme-ace) ，按照步骤来就行。

## 最后讲一下Hexo的各种命令，权当简单版的cheatSheet。

### 创建新的一篇新的博客文章
```
    hexo new "postName"
```

### 清除本地站点文件夹下的缓存文件（db.json）和已有的静态文件（public）
```
    hexo clean
```

### 生成静态文件
```
    hexo generate(同 hexo g)
```

### 本地预览
```
    hexo start(同 hexo s)
```
会在本地开启[服务](http://localhost:4000)