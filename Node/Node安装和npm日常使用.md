# Node 安装和 npm 日常使用

- 安装 Node.js
  - 安装包安装固定版本
  - 二进制文件安装（待填坑）
  - Node 版本管理简单介绍

- npm日常使用
  - 常用命令
  - 官网及镜像链接
  - 推荐使用nrm来管理和切换源
  - 全局安装和当前路径安装
  - npm 初始化以及 package.json

## 安装 Node.js

### Node.js 版本常识

早期的 Node 版本都是零字头，以中间版本号的奇偶来区分稳定版和非稳定版。例如：0.10.x，0.11.x。偶数表示稳定版本，奇数表示非稳定版本。

后来经历了 io.js 等一系列风波，Node 版本一跃从 4.x 开始，一直发展到如今。

目前看来， Node 的版本还是保持了偶数稳定，奇数非稳定的传统，只不过版本号从中间数字移到了首位大版本号。 Node 的 LTS(Long Time Support，长期稳定支持) 版本都出现在 4.x，6.x，8.x 等等。

### 安装包安装固定版本

Node.js 到今天生态圈已经非常繁荣了。像安装这样的问题，官方也早已经给出了非常方便的方式。

Windows 和 macOS 安装 Node 都可以直接在 [Nodejs官网](http://nodejs.org/zh-cn) 上下载安装包，无脑下一步安装。

Windows 下载对应 bit 的 `.msi` 安装包， macOS 下载 `.pkg` 即可。可以通过下载新版本的安装包进行更新 Node 版本。

安装成功之后在命令行执行`node -v`可以查看 Node 安装的版本。

### CentOS 环境源码编译安装 Node

Linux 系统中进行源码编译安装 Node 需要确保安装有 gcc 和 gcc-c++ 这两个 C/C++ 编译器，具体版本和其他一些要求可以查看官方的 [Build 文档](https://github.com/nodejs/node/blob/master/BUILDING.md) 查看。

```bash
# 查看 gcc gcc-c++ 安装情况
rpm -q gcc rpm -g gcc-c++
# 安装依赖
yum -y install gcc gcc-c++ kernel-devel
# 或者图省事可以直接安装 CentOS 的开发套件
yum -y update && yum -y groupinstall "Development Tools"
```

Node 源码的压缩包可以到 官方地址：https://nodejs.org/dist/v8.11.3/ 或者阿里云镜像地址：https://npm.taobao.org/mirrors/node/v10.5.0/ 下载。（后面的版本号可根据需要变换）

对应的文件名是 `node-v版本号.tar.gz`。

编译流程：

```bash
curl -o https://npm.taobao.org/mirrors/node/node-v8.11.3.tar.gz
# 或者 wget https://npm.taobao.org/mirrors/node/node-v8.11.3.tar.gz
tar -zxvf node-v8.11.3.tar.gz
cd node-v8.11.3.tar.gz
./configure --prefix=/usr/local/node/8.11.3
make
make test
make install

# 设置环境变量
vim /etc/profile
export NODE_HOME=/usr/local/node/8.11.3
export PATH=$NODE_HOME/bin:$PATH
source /etc/profile
```

整个编译安装过程视机器性能，我的单核主机用了差不多40分钟的样子。

### Node 版本管理简单介绍

可以通过 nvm 和 n 这两个工具来管理 Node 版本。

nvm 只能运行在 mac 下，windows 下可以使用 nvmw。

nvm 的安装和配置就稍微复杂一点，但是可以让各个版本的 Node 运行在独自的沙箱中，每一个版本的Node模块都相互独立（全局模块）。

n 则是 node 的一个模块，安装好 node 以后全局安装 n 模块就可以进行 node 版本的切换，不过 n 无法干涉全局模块。

附淘宝FED博文 [管理node版本，选择nvm还是n？](http://taobaofed.org/blog/2015/11/17/nvm-or-n/) 作为参考。

## npm 日常使用

npm 全称 Node Package Manager，是 Node 官方推荐的 Node 模块（包）管理工具。如今安装 Node 已经同时安装上了 npm， `npm -v` 可以查看安装的版本。

### 常用命令

```bash
# 全局安装更新npm到最新版本，mac终端提示权限问题可以前面加上sudo
npm i -g npm

# 查看npm配置
npm config list(ls) --json
npm config ls -l

# 查看当前镜像 默认 https://registry.npmjs.org/
npm config get registry

# 设置npm镜像为淘宝源（如果发包的话注意改回原来的）
npm config set registry http://registry.npm.taobao.org

# 全局安装包
npm install(i) <name> -g

# 当前路径安装包
npm i <name>

# 删除包
npm uninstall(un) <name>

# 全局删除包
npm un <name> -g

# 安装固定版本的包
npm i <name@version>

# 写入安装信息到 package.json 的 dependencies
npm i <name> --save(-S)

# 写入安装信息到 package.json 的 devdependencies
npm i <name> --save-dev(-D)
```

### 官网及镜像链接

[npm官网](https://www.npmjs.com)

[npm文档](https://docs.npmjs.com)

[npm淘宝镜像](https://npm.taobao.org/)

### 推荐使用 nrm 来管理和切换源

```bash
# 全局安装 nrm
npm i nrm -g

# 查看版本
nrm --version

# 列出所有当前源
nrm ls

# 使用某个源
nrm use cnpm
```

### 全局安装和当前路径安装

npm 安装的包都在 node_modules 目录内，当前路径安装会直接新建该目录，全局安装以 mac 为例，安装在 `/usr/local/lib/node_modules` 目录下。全局安装的路径也可以进行修改。

```bash
# 查看全局安装的模块路径（node_modules 所在的目录）
npm config get prefix

# 设置全局安装的模块路径（node_modules 所在的目录）
npm config set prefix xxx

# 查看全局 node_modules 路径（全局模块路径/node_modules）
npm root -g

# 查看当前目录安装的所有模块（通常只需要查看第一层深度的主要模块）
npm ls --depth=0

# 查看全局安装的所有模块（通常只需要查看第一层深度的主要模块）
npm ls --depth=0 -g
```

简单说，全局安装的模块通常都是一些涉及到命令行使用的工具，通过全局安装，可以直接在任何目录下使用该命令。

以 windows 为例，全局安装的路径会被添加到环境变量的 PATH 字段，访问该目录可以看到所安装模块的很多 `.cmd` 文件，就是 windows 独有的命令行执行脚本，可以**猜测**一下，当我们在命令行执行一个命令的时候，操作系统就会到环境变量的PATH里面去遍历查找那些可执行的命令行脚本（或者根据设置的环境变量事先就设置好了文件符号链接），找到了就执行命令，没有就会提示经常看到的一句 “xxx不是内部或外部命令，也不是可运行的程序或批处理文件”。

但是，全局安装的包并不能直接在项目中进行 `require('module')` 类似这样的引用，除非我们专门去为运行环境设置模块的访问路径，但这样显然是不利于多人合作也是不靠谱的。于是就有了 local 形式的本地安装。

本地安装的模块可以直接通过 require 函数传入模块标志进行引入，关于 Node 模块的查找机制以后再另行总结。

如果本地安装的模块涉及到命令行的命令，这些命令虽然不能直接在命令行使用，但是在当前目录下可以直接通过 `npm run <commandName>` 执行或者结合 npm scripts 来使用，这利用到了 node_modules/.bin 目录，这里面都是本地安装模块的命令行可执行脚本，该目录会在运行时临时加入系统的 PATH 变量，所以实际上并不是所有涉及命令行使用的模块都必须要进行全局安装。

### npm 初始化以及 package.json

```bash
# npm 初始化一个模块/项目
npm init

# npm 初始化一个模块/项目，并带上基本模板信息
npm init -y
```

初始化一个模块或者项目会需要填写诸如项目名称，作者，关键词等等，最后会生成一个 package.json 文件，用以提供模块/项目的相关信息，并且还有一些非常实用的字段。我通常在开启一个新项目/模块的时候，会用 `npm init -y` 生成基本的 package 文件模板，然后根据需要修改其中的内容。

下面介绍其中常用到的几个字段

- dependencies 和 devDependencies

  在刚接触 npm 的时候常常搞不懂 `npm install xxx --save-dev` 和 `npm install xxx --save` 还有 `npm install xxx` 有什么区别。其实单纯从安装的角度来说，并没有什么区别。其区别主要是在写入 package.json 文件的字段不同。
  
  低版本的 npm，不带任何参数执行 install，并不会将安装模块的信息写入到 package.json， 所以是不被采纳的做法。 npm 在 5.x 或者更早的某个版本会默认将不带参数安装的模块写入到 dependencies， 这和带上 `--save` 或者 `-S` 参数的效果一样，表明该项目的运行需要依靠这些包。比如用到的框架，工具类库，插件等在实际项目内部的模块就会写到这里。
  
  devDependencies 顾名思义就是本地的依赖，就是说当前程序要在本地运行和开发，需要依靠这些包。带上 `--save-dev` 或者 `-D` 参数就是在安装依赖时将其写入到这个字段。比如本地构建相关的包通常就会写入到这里。

  严格区分所安装模块的依赖和本地依赖关系，以便自己和他人在阅读 package.json 获取项目信息时能更有条理，一目了然。对使用者而言，如果需要阅读相关代码，也只需要关注 dependencies。

  对开发者而言，运行 `npm i` 会安装 package.json 文件中 dependencies 和 devDependencies 的所有依赖，以便项目能够在本地顺利运行。

  此外还有一个 peerDependencies 字段，但是我还没有在项目中用到，也未有见到过，这里就不提了。

- scripts

  这个字段就是 npm 为我们提供了一个非常好用的工具链手段， npm scripts。 我们知道可以通过在命令行执行 `node xxx`，让 Node 去运行当前目录下的 xxx.js 文件，但是如果命令带上一些复杂的参数，每次输入就很烦了，可以在这里把命令都添加进去，而在开发中直接运行 `npm run xxx` 就能够执行了，当然这只是 npm scripts 的基本用法之一。

  此外，npm 还提供几个常用的命令可以直接使用而无需加入 run 命令，常用到的就是 `npm start` 和 `npm test`。

  ```json
  "scripts": {
    "start": "node index.js",
    "dev": "node server.js",
    "build": "webpack --config.webpack.js",
    "test": "tap test/*.js"
  }
  ```

  以上scripts执行就是
  
  ```bash
  npm start
  npm run dev
  npm run build
  npm test
  ```

  scripts 字段还给我们提供了一个启示，就是入手一个模块，或者项目时，比如接手开发和维护，可以看看 package.json 文件的这个字段来了解项目的运行，构建和测试。

- main

  这个字段主要指明了模块加载的入口 js 文件，默认会是 index.js， 在使用 `require('module')` 引入模块时，就会去查找执行模块下的这个文件。

- bin

  这个字段用于指定命令的可执行文件，通常在开发命令行工具模块的时候用到。

关于 package.json 的两个参考文档：

[npm官方文档 - package.json](https://docs.npmjs.com/files/package.json)

[阮一峰JS标准参考教程alpha - package.json文件](http://javascript.ruanyifeng.com/nodejs/packagejson.html)
