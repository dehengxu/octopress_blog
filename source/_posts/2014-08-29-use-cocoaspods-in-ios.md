---
layout: post
title: "Use cocoaspods in iOS"
date: 2014-08-29 10:43:04 +0800
comments: true
categories: 
---
##Requirements
* rvm
* ruby
* gem

这三者缺一不可，因为Cocoapods就是一群想把gem包管理器引入到Cocoa开发中来的家伙开发出来的，感谢他们的辛勤努力！

##Installing
	
    gem install cocoapods

##Using

#### 安装Cocoapods:<pre lang=bash> sudo gem install cocoapods </pre> 

*   用 xcode 建立一个 xcode MyProj

#### 初始化Podfile

<pre lang="bash">cd MyProj

pod init

#编辑文件 'Podfile'

vi Podfile

</pre>

<pre lang="ruby">platform :iOS, '7.0'

xcodeproj 'MyProj/MyProj.xcodeproj'

pod 'JSONKit', '>0.1'

</pre>

*   在Cocoapods 中以workspace来管理所有的相关项目，并且会在workspace 根目录建立一个 Pods 目录，存放依赖的第三方子项目，并生成一个 libPods 静态库项目。<pre lang=bash> pod install#安装依赖项目，并配置pod 项目库 #最近 pod 更新 pod 数据库的速度会比较慢，导致 install 时间很长，也可以通过参数来绕开更新数据库 pod install --no-repo-update #设置一个别名来简化该操作，编辑 ~/.bashrc，在最末行增加 alias podinstall="pod install --no-repo-update" #最后执行 source ~/.bashrc 使其生效。以后可以直接执行命令 podinstall 来快速安装依赖库项目了。 </pre> 

*   Cocoapods 会自动通过git 下载对应的第三方项目，并组织好项目结构，完成项目的配置。可以说是一步到位，自动化完成项目配置和依赖配置。

*   Cocoapods 也可以针对 test 进行单独测试，看以下示例:（因为只有单元测试中使用mock第三方库，所以可以指定 test target 链接 ocmock ，正式项目就不需要链接编译该库了）<pre lang=ruby> platform :iOS, '7.0' #只针对 test target 添加 OCMock 库 target "MyProjTests" do pod 'OCMoc' end #以下针对整个 workspace 都起作用 pod 'JSONKit', '>0.1' </pre> 

*   其中 xcodeproj 'MyProj/MyProj.xcodeproj' 字段是必须的,因为 CocoaPods需要指定一个项目来设置Pods 的宿主项目，并且这个项目至少有一个 target ，因为 CocoaPods 也需要设置 target 的各种配置属性。

#### 在Cocoapods中使用本地代码库来创建项目

*   每次都需要从网站下载代码恐怕不是每个人都喜好的方式，cocoapods也支持直接从本地添加第三方库，这就需要用到 path 参数了。<pre lang=ruby> platform :iOS, '7.0' #只针对 test target 添加 OCMock 库 target "MyProjTests" do pod 'OCMoc' end #:path 开始表示，该第三方库，需要从 "~/Projects/3rd/JSONKit" 路径或取代码 pod 'JSONKit', '>0.1', :path => "~/Projects/3rd/JSONKit" </pre> 

> 注意：从本地仓库取代码需要有一个前提就是在项目的根目录中需要有 JSONKit.podspec 声明文件，来告诉 cocoapods 类库的配置情况和依赖的 framework 及第三方库。

###Different between Podfile and podspec

Podfile 和 podspec 两个文件可以看做是 CS 的关系， Podfile 是C，podspec 是S.

* Podfile 种描述需要用到的依赖库。
* podspec 描述向外提供依赖的项目配置信息。
  > 可配置的信息，相当丰富，从 target 系统版本，到CPU 架构支持，再到宏的配置，一应俱全。

###Use Svn in Podfile
###Use your local repostory in Podfile