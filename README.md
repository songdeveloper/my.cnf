# my.cnf
mac 下mysql 配置示例文件（用于mysql下没有my-default.cnf的用户下载）

Mac 环境，使用homebrew 安装mysql,版本为5.7.19. 
终端下，load data infile "文件路径" into table "表名称" 操作,Mac上csv导入mysql提示错误[Error Code] 1290 - The MySQL server is running with the --secure-file-priv option解决办法

1.mysql下查看secure_file_prive的值

mysql>SHOW VARIABLES LIKE "secure_file_priv";

secure_file_prive=null   -- 限制mysqld 不允许导入导出

secure_file_priv=/tmp/   -- 限制mysqld的导入导出只能发生在/tmp/目录下

secure_file_priv=' '     -- 不对mysqld 的导入 导出做限制

2， 我们只要修改secure_file_priv的值即可解决这个问题

添加修改mysql配置
mysqld --help --verbose | more (查看帮助, 按空格下翻)
你会看到开始的这一行(表示配置文件默认读取顺序)
Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/local/etc/my.cnf ~/.my.cnf
通常这些位置是没有配置文件的, 所以要自己建一个
ls $(brew --prefix mysql)/support-files/my-* (用这个可以找到样例.cnf)
cp /usr/local/opt/mysql/support-files/my-default.cnf /etc/my.cnf (拷贝到第一个默认读取目录)
在my.cnf文件中添加 secure_file_priv = ''

3，修改完毕后，重启mysql.    mysql.server restart

上面的操作命令是解决问题的正确流程，但是在发现5.7.18后开始，没有默认的配置示例文件。没办法，那我们只能自己创建一个，本文件已经创建好了，你可以直接下载，放在/etc路径下。

注意事项：load data infile "文件路径" into table "表名称" 操作的文件一定要使用utf8编码格式，防止文件编码格式导致失败



