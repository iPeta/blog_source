# 使用jenkins + git + 蒲公英 对 iOS 项目进行持续集成

## 持续集成解决什么问题？
>在项目进行到测试阶段的时候，每天需要给多台测试设备安装最新的 build 的版本让测试人员验证已经修改的 BUG，测试早上拿来一堆设备让你给他安装新版本，亦或者是让你打包一个 ipa 文件发给你们测试，在进行大规模测试时这样的行为每天要进行很多次。正是这些毫无价值的重复劳动浪费你大把的时间。构建持续集成则可以实现一键式发布最新 build 程序。

## 持续集成的基本流程
>使用 jenkins 自动拉取 git 仓库最新版本代码，并编译打包成 ipa 文件 使用脚本上传到蒲公英应用管理平台。

### 构建完善的持续集成系统
#### 一、安装 jenkins
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

		
			$ cd /usr/local/Cellar/jenkins/1.636/libexec/ 		
	* 运行 jenkins :
	 	
	 		$ java -jar jenkins.war --httpPort=8080
	
	* httpPort 用来指定访问的端口

	* 等待运行完毕用浏览器打开 [http://localhost:8080/](http://localhost:8080/) 就可以看到 jenkins 的界面了。

	
	 ![jenkins_home](http://7xo9cc.com1.z0.glb.clouddn.com/blog_jenkins_home)

	
#### 二、安装插件

* 安装所需的插件
 * GitLab Plugin
 * Gitlab Hook Plugin
 * Xcode integration
 * Environment Injector Plugin

 * Credentials Plugin
 * Keychains and Provisioning Profiles Management
* 安装插件步骤
  * 点击左侧的**系统管理（Manage Jenkins）**=> **管理插件（Manage Plugins）**

     ![进入插件](http://7xo9cc.com1.z0.glb.clouddn.com/blog_manager_plugus.png)



  * 选择**可选插件（Available）**=> 在左上角**搜索框**分别输入上面提到的插件

    
  * 选中所有需要安装的插件之后点击 **直接安装（Install without restart）**

       ![插件下载](http://7xo9cc.com1.z0.glb.clouddn.com/blog_download_plugus.png)


  * 等待插件全部安装完毕之后就可以配置你的第一个工程啦
   
   

#### 三、配置你的第一个job
1. 创建一个新的 jenkins job ,选择**构建一个自由风格的软件项目（Freestyle project）**。点击 OK 之后我们就进入到了一个配置界面。通过这个配置界面我们可以对我们的构建构建过程做出很多的自定义配置。
![创建job](http://7xo9cc.com1.z0.glb.clouddn.com/blog_create_home)


2. 你可以指定一个 jenkins 的自定义的工作空间,在配置路径中可以使用 jenkins 内置的环境变量方便使用，具体环境变量详情访问： [jenkins wiki](https://wiki.jenkins-ci.org/display/JENKINS/Building+a+software+project) [内置的环境变量]([jenkins wiki](https://wiki.jenkins-ci.org/display/JENKINS/Building+a+software+project))。可以使用 **$+环境变量的方式来使用** 如 `$JOB_NAME`

  ![路径设置](http://7xo9cc.com1.z0.glb.clouddn.com/blog_path_setting.png)
  
  
3. 首先你需要在**源码管理（Source Code Management）**中选择 Git，输入你项目 git 地址，点击 Add 添加 git 地址的账号和密码。（当然你也可以使用 ssh 方式连接，这里用 http 连接方式）。在 **Branches to build** 里面填写你需要构建的分支。这样 jenkins 每次构建之前的时候都会从这个 git 的地址上拉取最新的代码。


![git设置](http://7xo9cc.com1.z0.glb.clouddn.com/blog_git_setting.png)


4. 接下来就需要添加一系列的构建步骤包括用之前安装好的 Xcode 插件对工程进行编译打包出来一个 ipa 文件，使用[蒲公英](www.pgyer.com)提供的上传 ipa 的接口把 ipa 文件上传到你的蒲公英账户中，之后可以通过蒲公英生成的链接下载并安装程序。
5. 添加Xcode构建步骤：在下面 **构建（Build）** 的部分点击 **增加构建步骤（Add build step）**，选择 Xcode ,首先输入你工程中需要编译的 Target 

![Xcode设置](http://7xo9cc.com1.z0.glb.clouddn.com/blogBulid_Xcode_Setting.png)

