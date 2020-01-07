1.概论：SonarQube是管理代码质量一个开放平台，可以快速的定位代码中潜在的或者明显的错误，下面将会介绍一下这个工具的安装、配置以及使用。
      这里以Mysql5.7、sonarqube7.0、sonar-scanner-cli-4.2.0.1873 windows为例。
      
2.说明： sonarqube8.0不支持mysql，如果需要用到mysql的朋友建议使用8.0以下版本，mysql版本只支持5.6以上。（如果版本不对请参照之前的mysql升级备份问题）

3.下载sonarqube7.0

  a、最新版官网地址：https://www.sonarqube.org/downloads/
  b、百度云盘：https://pan.baidu.com/s/1qCTXAHhFvuurpwibHIG8eQ 提取码：xkyr

4、下载sonar-scanner

  a、最新版官网地址：https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
 
5.安装
  1).安装sonarqube，在任意目录下解压sonarqube-7.0.zip
    a).进入conf目录的 sonar.properties 修改相应配置，这里我使用的是mysql5.7，
     在启动前需要新建 sonar 数据库，否则会启动失败，因为在启动的时候会默认去创建所需的表结构
      
    #此处的账号密码根据自己的数据库来设定，
    sonar.jdbc.username=root
    sonar.jdbc.password=root
    
    sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false
    
    #配置web端口
    sonar.web.port=9000
    
 ![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/01.png)
  
 ![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/02.png)

   b）、确保jdk的环境变量已经配置完成，否则需要到wrapper.conf文件中指定启动的配置

   c）、配置修改完成后，到bin目录下启动服务，此处我选择的是：sonarqube-7.0\bin\windows-x86-64\StartSonar.bat启动



第一次启动有点慢，会去插入表
   d）、出现以下画面说明启动成功

![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/03.png)

   e）、等数据库表都创建成功就可以正常访问了

![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/sonarMysql.jpg)

   f）、在浏览器输入 http://localhost:9000进行访问，  用户名密码都是admin。
第一次登录会看到 Tutorial，按照提示设置用于验证身份的token。生成的 token需要复制记下来(保存下来生成的是MD5 token)！不会再显示第二次！ 在 用户 > 我的账户 > 安全 中可以生成新token（令牌），或者回收已创建的 token。

如果想强化安全，不想在执行代码扫描或调用Web Service时使用真实SonarQube用户的密码，可以使用用户令牌来代替用户登录。这样可以通过避免把分析用户的密码在网络传输，从而提升安全性。
2)、安装scanner

  a）、将下载好的文件 sonar-scanner-cli-4.2.0.1873-windows.zip解压到任意目录。
  b）、配置windows系统环境变量

![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/05.png)

![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/06.png)

  c）、添加扫描的项目，window+r打开cmd，到需要扫描的项目下执行命令
指定项目目录


说明项目扫描完成，在页面管理端就可看到项目的分析报告了
  d）、如需重新扫描项目，重新执行上一步骤即可

  e）、
   1)当然，每次执行那么长一条命令让人困惑，这里可以在scanner的conf目录下修改配置文件 sonar-scanner.properties

    # your authentication token
    sonar.login=[之前生成的token]

    # must be unique in a given SonarQube instance
    sonar.projectKey=[项目key]
    # this is the name and version displayed in the SonarQube UI. Was mandatory prior to SonarQube 6.1.
    sonar.projectName=[项目名称]
    sonar.projectVersion=1.0

    # Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.
    # This property is optional if sonar.modules is set. 
    # Comma-separated paths to directories containing source files.
    # 设置要分析的路径
    sonar.sources=.

    # Encoding of the source code. Default is default system encoding
    sonar.sourceEncoding=UTF-8

    # Set the language of the source code to analyze
    # 不设置则默认分析多种语言
    sonar.language = js

  2).也可以 打开要进行代码分析的项目根目录，新建sonar-project.properties文件
    输入一下信息
      # must be unique in a given SonarQube instance
 
      sonar.projectKey=my:project
 
      # this is the name displayed in the SonarQube UI

      sonar.projectName=hnsi-calc-ybjs-service

      sonar.projectVersion=1.0

      # Path is relative to the sonar-project.properties file. Replace "\" by "/" on Windows.

      # Since SonarQube 4.2, this property is optional if sonar.modules is set.

      # If not set, SonarQube starts looking for source code from the directory containing

      # the sonar-project.properties file.

      sonar.sources=src

      sonar.java.binaries=target

      sonar.language=java

      # Encoding of the source code. Default is default system encoding

      sonar.sourceEncoding=UTF-8
其中：projectName是项目名字，sources是源文件所在的目录，binaries是class文件所在的目录

  f）设置成功后，启动sonarqube服务，并启动cmd 输入sonar-scanner,分析成功后会出现下图
  
![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/04.jpg)
  
  g)打开http://localhost:9000  我们会看到主页出现了分析项目的概要图  
  
![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/bug.jpg)

     
点击我们的项目，就会看到此项目分析结果，选择bugs，可以看到问题bug列表
      
![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/bug1.jpg)
     
![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/bug0.jpg)
      
选择一个bug，点击，能看到具体的代码。看到数字“+2”，说明引起该问题原因是“1”，会发生问题的是“2”，
找到数字对应的代码，并进行分析改进。
     
![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/bug2.jpg)
      
![](https://github.com/Huangqianqan/java-/blob/master/photo/sonarqube/bug3.jpg)
      
6、汉化

登录到系统中，在页面上找到Administration > Marketplace，在搜索框中输入chinese，会出现一个Chinese Pack，点击右侧的install按钮进行安装。
安装成功后，会提示重启 SonarQube 服务器。
只需要等提示框消失后页面就已经显示中文了。

    
  最后终结几点：
    1：在安装sonarqube时一定先查看自己的mysql是不是5.6以上版本。如果用mysql数据库下载的sonarqube版本不能为8.0版。mysql版本问题参见mysql版本升级
    2：在安装后启动sonarqube时也许会报错，自动关闭，出现这样问题多数是mysql数据库问题，
        首先：查看数据库版本是否正确。
        其次：进入sonarqube安装目录中logs打开web.log文本文档日志，查看错误地方，一般会有Can not connect to database不能连接数据库。
        （我电脑中的mysql版本比较低后面重新下载安装了mysql由于跟之前的密码不同，sonarqube-7.0\conf\sonar.properties中的配置没改，其中需要注意的是数据库用户名，密码是否能对应，
          数据库是否创建好。）
        最后：需要分析的项目根目录下创建的sonar-project.properties文件中 projectName项目名字，sources源文件所在的目录，binaries：class文件所在的目录。这些地方是否正确
        其实出现了启动不成功最应该的就是翻看logs中日志报错在那些地方。剩下的就是分析项目bug了。比较直观的显示出漏洞和bug。对于项目内容比较多，
        特别是多人开发的找起来也比较简单容易。对于代码优化也有一定作用。
   
