---
title: "我想在gitlab用CI/CD部署" # Title of the blog post.
date: 2021-05-13T17:23:19+08:00 # Date of post creation.
description: "Article description." # Description used for search engine.
featured: true # Sets if post is a featured post, making it appear on the sidebar. A featured post won't be listed on the sidebar if it's the current page
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - git
tags:
  - gitlab
  - git
  - CI/CD
---

### 持续集成
持续整合（英语：Continuous integration，缩写CI），又译为持续集成，是一种软件工程流程，是将所有软件工程师对于软件的工作副本持续集成到共享主线（mainline）的一种举措。该名称最早由[1]葛来迪·布区（Grady Booch）在他的布区方法[2]中提出，在测试驱动开发（TDD）的作法中，通常还会搭配自动单元测试。持续集成的提出主要是为解决软件进行系统集成时面临的各项问题，极限编程称这些问题为集成地狱（integration hell）。

### 持续交付
持续交付（英语：Continuous delivery，缩写为 CD），是一种软件工程手法，让软件产品的产出过程在一个短周期内完成，以保证软件可以稳定、持续的保持在随时可以释出的状况。它的目标在于让软件的建置、测试与释出变得更快以及更频繁。这种方式可以减少软件开发的成本与时间，减少风险。

### 持续部署
持续部署（英语：Continuous deployment，缩写为CD），是一种软件工程方法，意指在软件开发流程中，以自动化方式，频繁而且持续性的，将软件部署到生产环境（production environment）中，使软件产品能够快速的发展[1][2][3]。

持续部署可以整合到持续整合与持续交付（Continuous delivery）的流程之中。
### CI/CD
在软件工程中，CI/CD或CICD通常指的是持续集成和持续交付或持续部署的组合实践。CI/CD通过在应用程序的构建、测试和部署中实施自动化，在开发和运营团队之间架起了桥梁。

现代DevOps实践涉及软件应用程序在整个开发生命周期内的持续开发、持续测试、持续集成、持续部署和持续监控。CI/CD实践或CI/CD管道（CI/CD pipeline）构成了现代DevOps业务的主干。

### 缘由
公司运维系统每次发布系统都需要经过一轮打包——编译——发至跳板机（期间需要输入动态码）——然后登陆跳板机（需要输入动态码））——从跳板机将压缩包发至web主机——再登陆web主机再解压缩新包并重启。
一轮下来，费时费力不说，命令行忘记，手打输入错误等情况，还是会经常出现的。可能大家会觉得这么简单的事还说难，但是，事情有时候不是说只做这一件，事情也不是天天都做，还有就是，同事的加入或离开，都需要重新弄这一套流程。跳板机需要配置申请，各方面需要重新指导，其实很费时间。

本次的接入，也是由控制台那块流程已经接入至cicd，所以也就是有前车了，不怕，有问题问大佬，这么说都写了一套了。

### 过程
目前只做一个管道（pipeline）操作，里面主要实现的工作就是上面我们手动要做的那些事。我们把整个空间就是个docker空间
所以第一件事就是，先安装依赖再进行编译。没啥问题！

.gitlab-ci.yml
```
image: node:13.11.0

stages:
  - build

# 构建
deploy:
  stage: build
  script:
    - echo '---------- build ------------'
    - npm install
    - npm run build
  tags:
    - 此处填入自己的tags

```

编译完之后就是打包发至服务器了，此处直接去跳板机处生成自己的ssh private key并保存只仓库中的CI/CD的变量`SSH_PRIVATE_KEY_133`当中，
并将web主机加入known_hosts里面。
![01](/images/2021/WechatIMG4.png)

```
before_script:
    #- apt-get -y install openssh-client
    - 'which ssh-agent || ( apt-get update -y && apt-get install openssh-client -y )'
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY_133" | tr -d '\r' | ssh-add - > /dev/null
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - echo "$SSH_KNOWN_HOSTS" > ~/.ssh/known_hosts
    - '[[ -f /.dockerenv ]] && echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config'
    - chmod 644 ~/.ssh/known_hosts
  
```
将key加入至此后，尝试发至web主机。
```

    - TZ='Asia/Shanghai'; export TZ; export VERSION=`date +%Y%m%d%H%M%S`
    - export Y_VERSION=`date +%Y`
    - echo $VERSION
    - tar zcvf dist-$VERSION.tar.gz dist
    - scp dist-$VERSION.tar.gz $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH-pre/tars/$Y_VERSION
```

如图可见，是发送成功了
![02](/images/2021/WX20210521-152830@2x.png)

那现在就简单了，ssh登陆。
```
- ssh $DEPLOY_USER@$DEPLOY_HOST "cd '$DEPLOY_PATH'-pre && tar -zxvf tars/'$Y_VERSION'/dist-'$VERSION'.tar.gz && pm2 restart 14"
```

### 后续
目前这个已经可运行并且开始使用。效果大概是5分钟左右可以发布完所有流程，本来是希望通过引入cache机制去跳过npm install的步骤，但目前公司提供的机器无法提供cache缓存。而且加入cache配置后，管道运行时间竟然超过12分钟。。。
![03](/images/2021/1.png)
![04](/images/2021/2.png)

### 下个版本
这个只是一个管道，没有全部分离出来。以后可以进行分解加强。
1）加eslint校验
2）css校验
3）编译独立
4）发布流程独立并且正式环境需手动发布


目前主要源码：
```
# This file is a template, and might need editing before it works on your project.
# Official framework image. Look for the different tagged releases at:
# https://hub.docker.com/r/library/node/tags/
image: node:13.11.0

before_script:
    #- apt-get -y install openssh-client
    - 'which ssh-agent || ( apt-get update -y && apt-get install openssh-client -y )'
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY_133" | tr -d '\r' | ssh-add - > /dev/null
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - echo "$SSH_KNOWN_HOSTS" > ~/.ssh/known_hosts
    - '[[ -f /.dockerenv ]] && echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config'
    - chmod 644 ~/.ssh/known_hosts
  
stages:
  - build

# 构建
pre-auto-deploy:
  stage: build
  only:
    refs:
      - pre-release
      - feature/test_CI
  script:
    - cd $CI_PROJECT_DIR && pwd
    - >
      if [ ! -d "./node_modules" ] || [ ! "$(ls -A './node_modules')" ]; then
        echo '---------- install ------------'
        npm install
          if [ $? -ne 0 ]; then
            echo "npm_install error"
            exit 1
          fi
      fi
      
    - echo '---------- build ------------'
    - npm run build
    - TZ='Asia/Shanghai'; export TZ; export VERSION=`date +%Y%m%d%H%M%S`
    - export Y_VERSION=`date +%Y`
    - echo $VERSION
    - tar zcvf dist-$VERSION.tar.gz dist
    - scp dist-$VERSION.tar.gz $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH-pre/tars/$Y_VERSION
    - ssh $DEPLOY_USER@$DEPLOY_HOST "cd '$DEPLOY_PATH'-pre && tar -zxvf tars/'$Y_VERSION'/dist-'$VERSION'.tar.gz && pm2 restart 14"
  #cache:
  #  key: ${CI_COMMIT_REF_SLUG}
  #  paths:
  #    - node_modules/
  tags:
    - 一个tags
    
prod-auto-deploy:
  stage: build
  only:
    refs:
      - develop
      - feature/test_CI_D
  script:
    - cd $CI_PROJECT_DIR && pwd
    - >
      if [ ! -d "./node_modules" ] || [ ! "$(ls -A './node_modules')" ]; then
        echo '---------- install ------------'
        npm install
          if [ $? -ne 0 ]; then
            echo "npm_install error"
            exit 1
          fi
      fi
      
    - echo '---------- build ------------'
    - npm run build
    - TZ='Asia/Shanghai'; export TZ; export VERSION=`date +%Y%m%d%H%M%S`
    - export Y_VERSION=`date +%Y`
    - echo $VERSION
    - tar zcvf dist-$VERSION.tar.gz dist
    - scp dist-$VERSION.tar.gz $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH/tars/$Y_VERSION
    - ssh $DEPLOY_USER@$DEPLOY_HOST "cd '$DEPLOY_PATH' && tar -zxvf tars/'$Y_VERSION'/dist-'$VERSION'.tar.gz && pm2 restart 8"
  #cache:
  #  key:
  #    files:
  #      - package-lock.json
  #      - package.json
  #    prefix: $CI_PROJECT_NAME-$CI_JOB_NAME
  #  paths:
  #    - node_modules/
  tags:
    - 一个tags
```

CI的编写可以再gitlab页面上进行CI lint检查
![04](/images/2021/3.png)

## REF
https://docs.gitlab.com/ee/ci/  
https://twblog.hongjianching.com/2020/05/09/GitLab-CI-CD-use-cache-and-artifacts/  
https://www.jianshu.com/p/b835a4d5bb4e  
https://docs.gitlab.com/ee/ci/caching/  
https://docs.gitlab.com/ee/ci/variables/README.html