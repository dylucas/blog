---
title: "git子模块"
description: 
date: 2022-12-15
image: "https://www.bing.com/th?id=OHR.PoinsettiaDay_ZH-CN5115071992_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"
draft: false
tags: ["git"]
categories: ["codeTool"]
---
## 背景
公司把服务从阿里云迁移自有机房中，需要对多个php服务进行容器化，在阿里云可对负载均衡的服务进行单台配置定时任务，容器由于是统一的镜像，无法对单台配置定时任务。

当时有两种解决方案：1. 对每个定时任务进行改造加锁；2. 合并php服务的所有代码单独开一台容器，后期维护两套代码。

迁移时间的紧迫感和后续维护的困难，这两种方案都让人难以接受，遂通过搜索引擎搜寻解决方案，发现了git子模块的功能。

## Git子模块介绍
有种情况我们经常会遇到：某个工作中的项目需要包含并使用另一个项目。 也许是第三方库，或者你独立开发的，用于多个父项目的库。 现在问题来了：你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。

我们举一个例子。 假设你正在开发一个网站然后创建了 Atom 订阅。 你决定使用一个库，而不是写自己的 Atom 生成代码。 你可能不得不通过 CPAN 安装或 Ruby gem 来包含共享库中的代码，或者将源代码直接拷贝到自己的项目中。 如果将这个库包含进来，那么无论用何种方式都很难定制它，部署则更加困难，因为你必须确保每一个客户端都包含该库。 如果将代码复制到自己的项目中，那么你做的任何自定义修改都会使合并上游的改动变得困难。

 **Git** **通过子模块来解决这个问题。 子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。 它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。**

  

## 实践
### 创建子模块服务
由于不同环境开启的定时任务不同，不同环境使用的分支无法进行合并

#### 创建生产master分支
```PHP
git clone http://gitlab.com/php-cron.git
cd php-cron/
# 项目过大
git config --global http.postBuffer 524288000 
git submodule add http://gitlab.com/test1.git
git submodule add http://gitlab.com/test2.git

# 配置对应各git子目录的分支
git config -f .gitmodules submodule.test1.branch master;git config -f .gitmodules submodule.test2.branch master;
git add -A
git commit -m "init"
git push
```

#### 创建测试dev分支
```PHP
git checkout -b dev
# 配置对应各git子目录的分支
git config -f .gitmodules submodule.test1.branch dev;git config -f .gitmodules submodule.test2.branch dev;
git add -A
git commit -m "dev"
git push origin dev
# 查看效果
git submodule update --remote
```

### 克隆子模块项目
```PHP
git clone --recurse-submodules http://gitlab.com/php-cron.git
```

**更新代码**：`git submodule update --remote`

### 流水线发布命令
git子模块与正常的git拉取代码有差异，需要修改Jenkins拉取代码命令。

```PHP
git clone http://gitlab.com/php-cron.git -b dev;cd php-cron;git submodule update --init;git submodule update --remote
```

`git submodule init` 用来初始化本地配置文件  
`git submodule update` 则从该项目中抓取所有数据并检出父项目中列出的合适的提交  
`git submodule update --remote` Git 将会进入子模块然后抓取并更新。

## 结论

使用git子模块后，后续只需维护原服务代码即可，发布`php-cron`流水线时会自动拉取各子项目的最新代码。  

## 参考链接
[Git - 子模块 (git-scm.com)](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)