# 使用jenkins + git + 蒲公英 对 iOS 项目进行持续集成

## 持续集成解决什么问题？
>在项目进行到测试阶段的时候，每天需要给多台测试设备安装最新的 build 的版本让测试人员验证已经修改的 BUG，测试早上拿来一堆设备让你给他安装新版本，亦或者是让你打包一个 ipa 文件发给你们测试，在进行大规模测试时这样的行为每天要进行很多次。正是这些毫无价值的重复劳动浪费你大把的时间。构建持续集成则可以实现一键式发布最新 build 程序。

## 持续集成的基本流程
>使用 jenkins 自动拉取 git 仓库最新版本代码，并编译打包成 ipa 文件 使用脚本上传到蒲公英应用管理平台。

## 构建完善的持续集成系统
### 一、安装 jenkins
* 安装 jdk : 
	* jenkins 依赖 jdk ，所以需要安装先安装 jdk ，可以从 jdk 官网 [http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 下载 dmg 进行安装。
* 下载 jenkins : 

	* 安装 homebrew ：[homebrew官网](http://brew.sh/)
			
			$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	* 用 homebrew 安装 jenkins

			$ brew install jenkins //等待安装完成
	
	你也可以从 [jenkins](http://jenkins-ci.org/) 官网上下载最新的 jenkins.war 包
	
* 启动 jenkins : 
	* 进入 jinkns 目录 homebrew 的安装路径，如果你是从官网上下载 war ，则 cd 进你 war 所在的目录
 		
`$ cd /usr/local/Cellar/jenkins/1.623/libexec/ `		
			
	* 运行 jenkins :
	 	
```
 $ java -jar jenkins.war --httpPort=8080
```
	
	
	* httpPort 用来指定访问的端口

	* 等待运行完毕用浏览器打开 [http://localhost:8080/](http://localhost:8080/) 就可以看到 jenkins 的界面了。
	
		


