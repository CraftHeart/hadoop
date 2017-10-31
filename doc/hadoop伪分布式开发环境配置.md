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
![1](https://github.com/CraftHeart/hadoop/blob/master/pic/java%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F.png)  
添加完之后，在命令行执行source ~/.bashrc
3. 测试环境是否安装正确：  
![2](https://github.com/CraftHeart/hadoop/blob/master/pic/java%E7%8E%AF%E5%A2%83%E6%B5%8B%E8%AF%95.png)  

## Linux上安装Hadoop伪分布式开发环境
1. 下载并解压hadoop安装包：[hadoop2.7.4](http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.7.4/hadoop-2.7.4.tar.gz)  
2. 配置hadoop环境变量  
在~/.bashrc文件中添加：  
export HADOOP_HOME=~/hadoop/hadoop-x.y.z  
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin  
添加完之后保存，在命令行执行source ~/.bashrc  
测试hadoop是否安装成功：
hadoop version  
![3](https://github.com/CraftHeart/hadoop/blob/master/pic/hadoop%E7%8E%AF%E5%A2%83%E6%B5%8B%E8%AF%95.png)
3. 配置分布式开发环境  
Hadoop的每一部分都是使用XML文件配置的，通用属性是在core-site.xml文件中，这些属性用来使HDFS、MapReduce、YARN能进入
到对应的配置配置文件中读取配置：hdfs-site.xml，mapred-site.xml，以及yarn-site.xml。
这些文件全部都位于etc/hadoop文件夹(hadoop安装包的子文件夹)中。  
hadoop可以运行在三种模式下：
- 单机模式：默认情况下，Hadoop被配置成以非分布式模式运行的一个独立Java进程。这对调试非常有帮助。  
- 伪分布式：Hadoop可以在单节点上以所谓的伪分布式模式运行，此时每一个Hadoop守护进程都作为一个独立的Java进程运行。  
- 完全分布式：hadoop进程运行在一个完全的集群上。  
这里先说如何搭建伪分布式的开发环境：  
  1. cd $HADOOP_HOME/etc/hadoop
  2. vim hadoop-env.sh  
  ![4](https://github.com/CraftHeart/hadoop/blob/master/pic/%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8Fjava%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE.png)
  3. 依次修改以下文件及其内容：  
    ![5](https://github.com/CraftHeart/hadoop/blob/master/pic/hadoop%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%9B%B4%E6%94%B9.png)  
  4. 修改好配置文件后，对HDFS集群进行格式化，HDFS集群是用来存储数据的.  
     在命令行执行：hdfs namenode -format,如果执行成功会看到：  
    ![6](https://github.com/CraftHeart/hadoop/blob/67c72443312bdf71b97a3d3ac571c60d3984abe7/pic/hadoop-format.png)
  5. 启动hdfs集群：  
    ![7](https://github.com/CraftHeart/hadoop/blob/67c72443312bdf71b97a3d3ac571c60d3984abe7/pic/start-dfs.png)  
    启动YARN集群：  
    ![8](https://github.com/CraftHeart/hadoop/blob/67c72443312bdf71b97a3d3ac571c60d3984abe7/pic/start-yarn.png)  
    启动作业历史服务器：  
    ![9](https://github.com/CraftHeart/hadoop/blob/67c72443312bdf71b97a3d3ac571c60d3984abe7/pic/jobservice.png)  
    查看以上进程是否正常运行，在命令行执行jps，正常情况会看到：  
    ![10](https://github.com/CraftHeart/hadoop/blob/67c72443312bdf71b97a3d3ac571c60d3984abe7/pic/jps.png)   
  6. 访问HDFS和YARN集群相对应的WEB监控页面：  
    在浏览器输入localhost:50070，可以访问hdfs集群web：    
    ![11](https://github.com/CraftHeart/hadoop/blob/67c72443312bdf71b97a3d3ac571c60d3984abe7/pic/8088.png)  
    在浏览器输入localhost:8088，可以访问Yarn集群web：  
    ![12](https://github.com/CraftHeart/hadoop/blob/67c72443312bdf71b97a3d3ac571c60d3984abe7/pic/50070.png)    
    需要说明的是，也可以通过远程访问者两个web，将localhost改为这台机器对应的ip即可。
    若无法远程访问8088端口，在yarn-site.xml文件下添加：  
     ```
        <property>
              <name>yarn.resourcemanager.webapp.address</name>
              <value>0.0.0.0:8088</value>
        </property>
     ```

  
至此，hadoop伪分布式开发环境安装成功！
    

