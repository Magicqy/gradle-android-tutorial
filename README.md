#Gradle简易介绍

Gradle是Android官方使用的新一代构建工作，与Ant相比更加灵活易用并具有相当强的可扩展性。
Unity在安卓平台的默认构建方式是将插件代码及资源置于plugins目录中从Unity中直接导出apk，无法对构建过程进行自定义。

官方网站:	http://gradle.org

#准备工作
	安装AndroidSDK，下载合适版本的BuildTools，设置好相应的环境变量ANDROID_HOME
	安装JavaSDK，设置好相应的环境变量JAVA_HOME
	安装AndroidStudio

#基础
- ##基本概念
	
	简单来讲，可以将Gradle看作一个构建框架，通过各种Plugin来提供构建不同类型产品的能力。
	默认情况下Gradle被用于构建Java工程。
	Google为Gradle提供了Android的构建插件，自从AndroidStudio中的所有的构建工作都通过Gradle Android Plguin来实现。

- ##获取Gradle Android Plguin
	
	从AndroidStudio安装目录中拷贝相应的文件即可，它们位于:
		{android_sdk_path}\tools\templates\gradle\wrapper
	该目录下共包含四个文件：
		gradle\wrapper\gradle-wrapper.jar
		gradle\wrapper\gradle-wrapper.properties
		gradlew
		gradlew.bat
	拷贝后注意修改gradle-wrapper.properties这个文件中所使用的gradle的文件版本，有可能比较旧。
	本例中使用2.2.1版本，将文件中最后一行的Url替换为如下即可：
		distributionUrl=https\://services.gradle.org/distributions/gradle-2.2.1-bin.zip
	执行gradle命令时，它会自动寻找相关的依赖项并下载安装。也可以提前下载好gradle zip包，将Url修改为本地文件路径以节省等待时间。

- ##第一次尝试
	
	将上述的四个文件拷贝至一个目录中来开始第一个测试工程，例如本例中的first-proj。
	在命令行中调用gradlew.bat init(如果在MacOS上需要使用gradlew init)来进行初始化操作。
	会发现生成了两个文件build.gralde和settings.gradle，以及一个目录.gradle
	build.gradle是构建过程的核心，它里面记载了构建工程的配置，其内容如下：
		/*
		// Apply the java plugin to add support for Java
		apply plugin: 'java'
		
		// In this section you declare where to find the dependencies of your project
		repositories {
			// Use 'maven central' for resolving your dependencies.
			// You can declare any Maven/Ivy/file repository here.
			mavenCentral()
		}
		
		// In this section you declare the dependencies for your production and test code
		dependencies {
			// The production code uses the SLF4J logging API at compile time
			compile 'org.slf4j:slf4j-api:1.7.5'
		
			// Declare the dependency for your favourite test framework you want to use in your tests.
			// TestNG is also supported by the Gradle Test task. Just change the
			// testCompile dependency to testCompile 'org.testng:testng:6.8.1' and add
			// 'test.useTestNG()' to your build script.
			testCompile "junit:junit:4.11"
		}
		*/
	这是一个用于java的构建配置，在使用前时请将首行和末行的注释取消。其核心部分只有一行，其它的配置暂时都不是必须的。
		apply plugin: 'java'		
	接下来我们编译一段小程序，创建一个文件位于：first-proj\src\main\java\main.java
		package com.test.gradle;

		public class main
		{
			public static void main(String args[])
			{
				System.out.println("Hello Gradle!");
			}
		}
	在命令行中执行gradlew.bat build，结果如下，编译成功。
		:compileJava
		:processResources UP-TO-DATE
		:classes
		:jar
		:assemble
		:compileTestJava UP-TO-DATE
		:processTestResources UP-TO-DATE
		:testClasses UP-TO-DATE
		:test UP-TO-DATE
		:check UP-TO-DATE
		:build
		
		BUILD SUCCESSFUL
	在build目录下可以找到生成的结果，class/jar等。执行以下命令来观察输出结果：
		java -classpath build\libs\first-proj.jar com.test.gradle.main
		Hello Gradle!
	java plugin是gradle提供的众多插件的一种，可以修改另一种插件'application'进行尝试
		apply plugin: 'application'
		mainClassName = 'com.test.gradle.main'
	在命令行执行gradlew.bat tasks以观察任务列表。执行gradlew.bat run直接运行出程序，观察输出结果：
		:compileJava UP-TO-DATE
		:processResources UP-TO-DATE
		:classes UP-TO-DATE
		:run
		Hello Gradle!
	
- ##Gradle概念介绍

	Gradle的核心概念是project和task，一个project以一个目录为根，搭配一个或多个build.gradle配置。
	task则是一系列操作，完成构建过程。上面使用过的init/build/run等都是Gradle默认提供task。
	只需要在build.gradle中编写配置，在命令行中调用一个task即可完成构建过程。
	
	例如一个简单的任务定义为：
		task helloWorld << {
   			println "Hello World!"
		}
	执行结果：
		gradlew.bat helloWorld
		Hello World!
		
	每种插件都为某种类型的构建任务提供了大量预置的任务，通过简单的配置就可以方便的完成构建配置。
	
	Gradle Plugin For Android相关页面：
	- http://developer.android.com/tools/building/plugin-for-gradle.html
	- http://tools.android.com/tech-docs/new-build-system/user-guide
	
- properties文件

##进阶
- settings文件
- 多工程
- 工程依赖
- Build Variants
- 自定义参数

##与Unity相结合
- Unity导出工程并使用gradle构建
- 加入第三方SDK
- 多渠道
- 辅助任务