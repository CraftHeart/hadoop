# Hadoop伪分布式开发环境配置
> **Created by cuber at 2017-10-31** 
## Linux上安装java开发环境
1. 下载并解压JDK: [Oracle JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，
注意，上一步可以在任何一个文件夹下解压，解压之后即是安装好了java，剩下的就是配置环境变量了。
2. 配置环境变量：
需要配置以下三个环境变量：
-  PATH环境变量。作用是指定命令搜索路径，在shell下面执行命令时，它会到PATH变量所指定的路径中查找看是否能找到相应的命令程序。
我们需要把 jdk安装目录下的bin目录增加到现有的PATH变量中，bin目录中包含经常要用到的可执行文件如javac/java/javadoc等待，
设置好PATH变量后，就可以在任何目录下执行javac/java等工具了。 
- CLASSPATH环境变量。作用是指定类搜索路径，要使用已经编写好的类，前提当然是能够找到它们了，JVM就是通过CLASSPATH来寻找类的。
我们需要把jdk安装目录下的lib子目录中的dt.jar和tools.jar设置到CLASSPATH中，当然，当前目录“.”也必须加入到该变量中。
- JAVA_HOME环境变量。它指向jdk的安装目录，Eclipse/NetBeans/Tomcat等软件就是通过搜索JAVA_HOME变量来找到并使用安装好的jdk。
在用户的.bashrc文件中添加以下几行：  
![1](D:\repo\java\hadoop\pic\java环境变量.png)
