---
title: Coding Guidelines
date: 2017-11-05 21:30:52
tags: Programming
---

![](http://cc.cocimg.com/api/uploads//20171026/1509003373729113.gif "读别人写的代码的时候：这是什么…………我X")

<!-- more -->
  
![](http://www.10tiao.com/img.do?url=http%3A//mmbiz.qpic.cn/mmbiz_gif/rh7Nru3hbNOrWQPgZCnOyIwwgQ3LmNluibusvicXViaAkXjck9anMWC6P5IhGGtfFhHfgEB9uLvqQxfAWCVChuUxg/0%3Fwx_fmt%3Dgif "偶然间看到之前写的代码")

上面应该是开发者经常遇到的内心戏😂

### 痛点
项目创建和团队组建一般会有一些公共的约定以及一些规范定下来，而代码大多数是通过主流的Git来做版本控制的，但是这种规范只能依靠个人的意识，或者通过代码Review来解决，而做代码Review的时候，你也不好意思总是写上一堆诸如“这里要加个空格”、“那里要加上换行”的评论吧？同时随着团队人员的扩增或项目功能迭代代码量越来越大，如果不管，久而久之，会因为每个人的习惯不同，代码呈现出多种风格，导致项目的维护扩展越来越困难，项目代码看起来也不像一个成熟团队做出来的产品，这也导致开发者经常吐槽项目代码烂。   

commit message编写随意，提交内容和主题差异大（甚至commit massage是空的，或者永远都是固定的类似 test的字符😂），导致回查困难。 

代码规范想必有部分同学会把它和个人习惯混为一谈，其实代码规范和个人习惯压根不是一个层面上的东西。代码规范针对的是团队，而个人习惯仅仅针对你自己。代码规范的目的是为了提高团队协作的效率以及代码的可读可维护性，这是需要团队共同推进实施的，难的是如何更好的落地实施和持续做下去。

### 针对上面痛点要解决的问题
1. 代码的规范化及规范的定义
2. 规范的应用与落地实施 （要简单易用，最好能自动化）
3. commit message的规范化及自动校验

### 解决方案
* 代码规范 	>> 基于clang-format的 [spacecommander](https://github.com/bluesky0109/spacecommander)  
* commit message 	>> [Commitizen](https://github.com/commitizen/cz-cli) + [commitlint](https://github.com/marionebl/commitlint) + [husky](https://github.com/typicode/husky)

### 代码规范
#### 关于代码规范的选择原则   
1. 每种语言 都会有一些 行业标准或最佳实践之类的，优先选择 大家都认可的规范标准
2. 不要带有个人习惯倾向 来设定编码规范，当遇到个人习惯和行业规范相冲突时应该反思下自己的编码习惯是否不太合适，调整自己改掉自己的不良习惯来去适应标准规范，而不是让标准来迁就自己的个人习惯

这里针对Objctive-C语言 推荐一份个人翻译的基于raywenderlich.com的[iOS-Style-Guide](/images/iOS-Style-Guide.pdf)。   
针对iOS的Objective-C项目选择基于 clang-format的 spacecommand来做代码规范校验及自动化格式代码, 主要优点：   

* 使用方法极其简单  
* 功能丰富，不仅能结合githooks进行校验，还能按照指定的规范自动格式化代码  
* 可默认自定义忽略不需要格式化的文件  

#### spacecommand的安装和使用
##### 安装
1. clone spacecommander的代码  
`git clone https://github.com/bluesky0109/spacecommander`  
 到任意一个目录中，记录下这个目录路径 (**`pwd`查看路径**）。  
2. 为了后期方便使用,我们可以定义环境变量,在 ~/.zshrc中,添加一行  
`export SPACECOMMANDER="spacecommanderPath"`
3. 配置别名，方便后面使用。 ~/.zshrc中添加如下代码
配置别名：  

```
alias fmtSetup="$SPACECOMMANDER/setup-repo.sh"

alias fmtFile="$SPACECOMMANDER/format-objc-file.sh"

alias fmtRepo="$SPACECOMMANDER/format-objc-files-in-repo.sh"
```

##### 使用  
1. 打开终端，切换到你的某个项目的根目录.例如,示例工程 /Users/sky/myProject
` cd /Users/sky/myProject `  
然后 执行 `$SPACECOMMANDER/setup-repo.sh`  
如果配置了别名 直接 执行 `fmtSetup` 进行初始化配置

> 备注: 如果你的项目包含了sub module,sub module并不会被添加hooks脚本,如果需要添加,需要进入module,再运行脚本命令  

从上面脚本的运行中的回显,可以看到,该命令完成了两件事情:    

	1. 添加了隐藏文件 .clang-format文件的替身（.clang-format定义了各种规范配置）   
	2. 添加了git hooks ,pre-commit,没有后缀名的那个,凡是带.sample的文件都是示例文件,那是git给你的示例demo,教你写hooks的

2. 执行 fmtRepo 根据.clang-format定义的各种规范配置对整个 Repo工程代码进行格式化
3. 执行 fmtFile filePath 根据.clang-format定义的各种规范配置 对指定文件进行格式化

### commit message的规范和校验
代码按照规范编写完成后，就需要提交及推送到远程，这里有个重要的点就是 commit message的编写。

#### commit message的格式规范
社区有多种 Commit message 的[写法规范](https://github.com/ajoslin/conventional-changelog/blob/master/conventions)。
这里介绍基于[Angular 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0)，这是目前使用最广的写法，比较合理和系统化，并且有配套的工具。

每次提交，commit message都要包含三个部分：Header,Body和Footer。  

```
<type>(<scop>): <subject>  
//  空一行   
<body>  
//  空一行  
<footer>
```

其中，Header是必需的，Body和Footer可以省略。
不管是哪一部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

type用于说明commit的类别，只允许使用下面的标识。

```  
feat：新功能  
fix：修复bug  
docs：只改变文档  
styles：只是改变代码风格（不影响代码的原有运行和逻辑）  
refactor：重构（既不是新增功能，也不是修改bug的变动）  
perf：提高性能的代码修改  
test：增加缺失的测试或修正现有的测试  
build：影响构建系统或外部依赖的改变  
ci：CI配置和脚本的改动  
chore：构建过程或辅助工具的变动  
revert：撤销之前的提交  
```

#### commit message的格式化工具及自动校验
[Commitizen](https://github.com/commitizen/cz-cli)是一个格式化commit message的工具  
1. 需要 npm环境，npm安装自行根据系统选择安装方式  
2. npm install -g commitizen  
3. 使用Anjular的commit 规范,在项目的根目录下执行命令：`commitizen init cz-conventional-changelog`  
4.以后所有提交的时候，用git cz 代替 git commit命令，就会出现提及类型的选择  

> 备注：可能遇到的问题：因为commitizen工具是基于Node.js的，而我们的工程目录下是没有package.json的
，执行上面的命令可能就会报错，或者git cz时不能正常弹出 类型选择
解决方法： 在项目中 执行 npm init —yes来添加一个package.json然后再执行上面的命令。  

5.commit message校验。上面的配置只有在 使用git cz才能保证commit message的规范，
接下来安装 [commitlint](https://github.com/marionebl/commitlint)，并通过[husky](https://github.com/typicode/husky)配置githooks每次提交执行脚本 自动校验 commit message是否符合规范：
> Install commitlint cli and angular config
`npm install --save-dev @commitlint/{config-angular,cli}`  
> Configure commitlint to use angular config
`echo "module.exports = {extends: ['@commitlint/config-angular']}" > commitlint.config.js`

然后在 package.json中配置如下脚本  

```
"scripts": {  
    "commitmsg": "commitlint -e $GIT_PARAMS"  
}
```
以后每次提交  git commit 时就会后自动跑脚本校验 编写的 commit message是否符合规范，如果不符合会失败。

前面代码规范 和 commit message 逐步做的都比较好后 慢慢就会有这种感觉
![](http://www.10tiao.com/img.do?url=http%3A//mmbiz.qpic.cn/mmbiz_gif/1hReHaqafad7tbP4xPynYftcqyibzkXvS3F3aM9La7OEpr9Z4QLz51ib0WAOtTF8Lb0ShZQ3VJy1E92OibDxIpJ1Q/0%3Fwx_fmt%3Dgif "一切尽在掌控中")

### 接下来何去何从
just do it, 实操才能真正掌握, 工具和操作方法都给了，至于用不用、怎么用还取决于你。

### 小彩蛋
下篇预告：
给你一个高性能、高可定制化的 扫码组件（极速启动、什么二维码、条形码、各种常规码都不在话下，什么微信扫码界面、支付宝扫码界面、自动感应环境亮度均可定制，还可以按照自己的需求DIY，你的扫码组件你做主）  

提前体验 请查看[YFCodeScan](https://github.com/bluesky0109/YFCodeScan)（<mark>功能均已完成，文档和示例demo待补充</mark>），敬请关注本博客.

