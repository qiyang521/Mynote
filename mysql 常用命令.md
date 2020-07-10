#### mysql 常用命令

 1. 修改用户名:update user set user='xxx' where uesr='xxx';(默认是root) flush privileges;

 2. 四种方法:

     	1. set password for [root@localhost](mailto:root@localhost) = password('shapolang');
     	2. mysqladmin -u用户名 -p旧密码 password 新密码;
     	3. update user set password=password("shapolang") where user="root";(需要use mysql);
     	4. 忘记root密码时,关闭mysql服务,输入mysqld --skip-grant-tables(启动mysql服务时跳过权限表认证),打开另外一个DOS,输入mysql回车,use mysql;改密码:update user set password=password('root') where user='root'; flush privileges;quit;

    

 3. 

    