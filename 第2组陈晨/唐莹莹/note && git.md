##note && git
#### 1.git
安装brew在mac上
mac http://brew.sh
```
brew install git 
```
#### 1.1 告诉git你是谁
```
git config --global user.name xxxxx
git config --global user.email xxxxx
git config --list
```
#### 1.2 初始化git
```
git init
```

#### 创建并进入目录 
```
mkdir gitTest && cd gitTest
```

#### ls显示所有文件
```
ls -al 显示隐藏的
```

#### 创建文件
```
touch index.txt
cat index.txt 查看内容
```

#### 查看git状态
```
git status 
```

#### 添加到暂存区
```
git add .
```

#### 提交到历史库
```
git commit -m 'write hello world'
```

#### 对比代码
工作区和暂存区
```
git diff
```
暂存区和历史区 
```
git diff --cached
```
工作区和历史区 
```
git diff master(分支名)
```

#### 查看日志
```
git log 过多可以使用上下键查看，不想看可以q退出
```

#### 查看所有日志
```
git reflog
```
#### 回滚
```
git reset --hard 版本号
```
#### vi编辑
```
vi index.txt
i 编辑模式
esc + :wq 保存并退出  强制退出q!
```


#### 分支管理
- 查看分支
```
git branch 
```
- 创建分支
```
git branch <branchName>
```
- 切换分支
```
git checkout <branchName>
```
- 删除分支（不能自己删除自己）
```

git branch -D <branchName>
```
- 创建并切换(相当于将当前内容克隆一份)
```
git checkout -b <branchName>
```
- 在master上合并dev分支
```
git merge dev
```
- 产生冲突(手动解决提交新的版本)

#### 创建仓库
- 创建空仓库，写一个仓库名
```
new repository
```

- 本地要建一个README.md .gitignore
```
echo '.idea' >> .gitignore
echo 'welcome' >> README.md
```

- 提交并关联仓库
```
git add .
git commit -m ''
git remote add origin 地址
git remote -v 查看所有关联
git remote rm origin 删除
```
- 推送到远程仓库上
```
git push origin master(可以推送其他分支) -u(upstream 下次提交不必再输入origin master)
```

#### 发布静态页
- 发布的静态页必须在gh-pages分支
```
git checkout -b gh-pages
git add .
git commit -m 
git push origin gh-pages
```

#### 讲师有一个仓库 
- 组长fork我的仓库
- 将信息 放到自己的文件夹下提交代码
- 推送到github上
- 发送pull request请求合并

#### 组长给组员开通权限
- 组员clone最新代码
- 放入自己的文件提交
- 拉取组长的仓库的最新代码
```
git pull 
```
- 再次推送

#### 组长推送给老师
- 先拉取自己组的最新代码
```
git pull 
```
- 拉取老师的最新代码
```
git remote add teacher https://github.com/zhufengzhufeng/201615node.git
```
- 推送到自己的仓库上
```
git push origin master
```
- pull request

###note
#### 单线程和多线程
- node主线程是单线程的，进程中包含线程，正常java 一个进程中包含多个线程，node中一个进程只能包含一个线程，允许开子进程。

#### 同步和异步
- 代码从上到下执行，先走同步在走异步，异步不会阻塞主线程
- 五个人一起吃饭

#### 阻塞和非阻塞
- 针对内核来说的，非阻塞是异步的前置条件

#### 回调（回头再调）
- 用回调来解决异步编程问题

#### 事件环

#### 异步的文件读写，callback,定时器，能用异步 不用同步

#### node全局对象
- 在任意地点可以直接访问的
- 在global上挂载的都是全局对象

#### js中模块 
- (seajs cmd,requirejs amd,node commonjs)
- cmd 就近依赖,amd 依赖前置
- 单例(不能保证完全解决冲突,调用时调用名字过程)
- 闭包，node中实现模块化 采用的是读写

#### commonjs(提高了可维护性，有利于分工协作，高内聚低耦合)
- 如何定义模块    
创建一个js文件，每个文件就是一个模块，多个模块可以组成一个包
- 如何导出一个模块  
exports/module.exports  
- 如何使用一个模块  
require



## npm node-package-manager
### 全局下载(在命令行下使用) -g
### 安装nrm源切换工具
```
npm i nrm -g
```
### 增加珠峰源
```
nrm ls
nrm add zf http://172.18.1.139
nrm use zf
```
### 安装全局
https://github.com/ksky521/nodePPT
```
npm install nodeppt -g
```
### http-server
```
npm install http-server -g
```
启动服务
```
http-server -p 3000 更改端口
```
### 卸载
```
npm uninstall http-server -g
```

### 本地安装(在我们代码里使用的)
#### 初始化依赖文件(package.json)
在指定目录下生成
```
npm init -y
```
#### 1.开发依赖 gulp

```
npm install gulp (--save-dev)或者(-D)
```

#### 2.项目依赖 jquery
查看版本
```
npm info jquery
npm install jquery@2.2.4 (--save)或者(-S)
```

#### 3.发布自己的包
- package.json 
    - name不能和已发布的包重名
    - main里对应的主文件件写一个
- 发布要切换到npm  
- 添加用户 有的话可以登录
```
npm addUser
```
- 发布
```
npm publish 
```
- 卸载包
```
npm unpublish  --force
```

## 模块的查找机制

> 为什么-g安装的可以在命令行下使用,npm可以在命令行下使用，所有的全局包 都安装在npm上，会在npm下创建一个脚本文件，可以映射到真实的文件上，所以通过全局安装的可以直接在命令行下使用

## bower(管理前端文件的) 
npm(管理node模块的) ->安装的文件放到node_modules下 不能指定安装目录
bower(管理前端文件的) ->制定目录下载 只管理前端文件(在git中下载)
```
npm install bower -g
```
### 1.1初始化 bower.json记录依赖
```
bower init
```
### 1.2下载文件
```
bower install bootstrap --save
```
### 1.3 指定目录
```
.bowerrc
{"directory":"src/public/lic"}
```


> 默认安装到bower_component下

### 发布hexo
```
npm install hexo-cli -g
```

### 生成blog
```
hexo init
```

### 启动服务
```
hexo server
```

### 文章放在_posts下
md格式文件

##### 仓库名
```
github名.github.io
```

#### 发布到git上需要下载一个插件 
```
npm install hexo-deployer-git --save
```

#### 配置_config.yml
```
deploy:
  type: git
  repo: https://username:password@github.com/zuyuan/zuyuan.github.io.git
  branch: master
  message: push
```

#### 如果重新提交
##### 生成
```
hexo g
```

#####发布
```
hexo deploy
```

##### 访问网址
```
zuyuan.github.io
```