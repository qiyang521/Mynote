#### mysql 安装

  1. mysql 下载地址:	https://dev.mysql.com/downloads/mysql/

  2. 解压到指定磁盘目录,在解压好的mysql 下创建一个名为data的空文件夹

  3. 自行创建一个my.ini 文件,配置信息如下:

     ```python
     [mysqld]
     basedir ="D:\Mysql\mysql-8.0.12-winx64"   # 设置mysql的安装目录
     datadir ="D:\Mysql\mysql-8.0.12-winx64\data"   # 设置mysql数据库的数据的存放目录，必须是data，或者是//xxx/data
     sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
     #服务端的编码方式
     character-set-server=utf8mb4
     [client]
     #客户端编码方式，最好和服务端保存一致
     loose-default-character-set=utf8mb4
     [WinMySQLadmin]  
     Server = "D:\Mysql\mysql-8.0.12-winx64\bin\mysqld.exe"
     ```

		4.	配置MySQL运行环境:  选中 我的电脑 → 鼠标右键 → 属性(R) → 高级系统设置 → 环境变量(N) 进行环境配置(xxx\bin)

		5.	以管理员身份打开“命令行窗口”，输入命令mysqld --initialize-insecure --user=mysql

		6.	输入命令mysqld -install

     ​	注意:当看到Service successfully installed时，表示Mysql服务添加成功

		7.	输入命令net start mysql，启动Mysql服务

		8.	登录mysql后，修改密码（默认密码为空）

     		1.	以管理员身份打开“命令行窗口”，输入mysql -uroot -p并按下回车键
     		2.	在弹出Enter password: 时，继续按下回车键，即可登录mysql
     		3.	输入命令use mysql;
     		4.	输入命令ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码'; （注意末尾要有分号）
     		5.	输入命令flush privileges; 
     		6.	输入命令exit;

		9.	使用Navicat测试连接Mysql



#### mysql卸载

 1. 关闭mysql服务:命令: net stop mysql

 2. 删除Mysql的注册表

     1. Win+R打开运行界面，在输入框中输入 regedit 进入系统注册表窗口

     2. 找到的MySQL目录中的 EventMessageFile 和 TypesSupported 两个文件进行删除

        ```python
        HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Application/MySQL
        HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Application/MySQL
        HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Application/MySQL
        ```

 3. 移除Mysql服务:cmd--->mysqld -remove-->Service successfully removed

 4. 删除Mysql文件:将Mysql安装目录下的文件全部删除即可,卸载完毕.

    

    ```python
    
    ```

    



​	

​	



























​	



