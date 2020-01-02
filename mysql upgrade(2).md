第一步：停止原来的MySQL服务，打开任务管理器，找到mysqld的进程名，停止掉。

第二步：备份原来数据库的文件，在C:\ProgramData\MySQL 相应的版本目录下面，有data目录，将此目录复制到其他地方备份。

第三步：运行MySQL的卸载程序，可以使用360或者QQ电脑管理的软件管理，或者是控制面板程序里面执行卸载。

第四步：进入mysql官网 https://dev.mysql.com/downloads/installer/ 选择下载：mysql-installer-community-5.7.20.0.msi


第五步：选择custom安装server：



第六步：根据自己系统选择：



第七步：取消“Development Components”的勾选（因为我们只需要安装mysql server）



第八步：按提示点击 Execute 安装



第九步：安装mysql server



第十步：Config Type选择“Development Machine”



第十一步：设置root密码后，这里我取消了开机启动



第十二步：点击“Execute”，等待完成就可以了



第十三步：点击finish，后面的update Catalog可以忽略。使用 HeidiSQL 测试连接即可。

第十四步：：将我们第一步中备份的data目录复制到C:\ProgramData\MySQL下面，找到5.7的目录，覆盖里面的data目录。如果提示覆盖失败，则在任务管理器里面，先将mysqld的进程关掉。

第十五步：现在启动MySQL5.7，会发现启动失败。在开始菜单里面找到MySQL Install - Community，打开始是如下的界面，执行一下Reconfigure即可。



点赞
