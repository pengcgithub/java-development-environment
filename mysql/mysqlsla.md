# mysqlsla慢查询分析

## 简介

- mysqldumpslow
	- mysqldumpslow slow.log

- mysqlsla
	- ./mysqlsla slow.log

- percona-toolkit
	- 依赖环境
	- percona-toolkit
	- perl-IO-Socket-SSL
	- perl-IO-Socket-SSL
	- perl-Net-LibIDN
	- per-Net-SSLeay

mysqldumpslow为mysql自带的慢查询分析工具，分析效果一般。mysqlsla 和 percona-toolkit 都是属于第三方开源的分析工具，此处只分析mysqlsla的安装和使用。

## 安装步骤

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

- 下载： `wget http://hackmysql.com/scripts/mysqlsla-2.03.tar.gz`

- 解压： `tar -zxvf mysqlsla-2.03.tar.gz`

- 移到解压包：`mv /opt/setups/mysqlsla-2.03 /usr/local/`

- 进入mysqlsla-2.03： `cd /usr/local/mysqlsla-2.03`

- 执行perl脚本检查包依赖关： `perl Makefile.PL`

-  编译、安装： `make && make install`

<pre>
# make && make install
cp lib/mysqlsla.pm blib/lib/mysqlsla.pm
cp bin/mysqlsla blib/script/mysqlsla
/usr/bin/perl -MExtUtils::MY -e 'MY->fixin(shift)' -- blib/script/mysqlsla
Manifying blib/man3/mysqlsla.3pm
Installing /usr/local/share/perl5/mysqlsla.pm
Installing /usr/local/share/man/man3/mysqlsla.3pm
Installing /usr/local/bin/mysqlsla
Appending installation info to /usr/lib64/perl5/perllocal.pod
</pre>

- 执行日志分析： `mysqlsla -lt slow ./slow.log`

## 错误解决

> 执行`perl Makefile.PL`报`Can't locate ExtUtils/MakeMaker.pm in @INC。。。`错误
	
- 解决方法： `yum install perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker`
<pre>
Can't locate ExtUtils/MakeMaker.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at Makefile.PL line 2.
BEGIN failed--compilation aborted at Makefile.PL line 2.
</pre>

> `mysqlsla -lt slow ./slow.log`分析报错

-  解决方法： yum install perl-Time-HiRes

- `yum install perl-Time-HiRes`执行成功只有再次分析依旧报错
	- wget http://search.cpan.org/CPAN/authors/id/T/TI/TIMB/DBI-1.634.tar.gz
	- tar -zxvf DBI-1.634.tar.gz
	- perl Makefile.PL
	- make && make install

<pre>
Can't locate DBI.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at /usr/local/bin/mysqlsla line 2098.
BEGIN failed--compilation aborted at /usr/local/bin/mysqlsla line 2098
</pre>


## mysqlsla分析

<pre>
Count         : 2  (100.00%)
Time          : 6.000414 s total, 3.000207 s avg, 3.000201 s to 3.000213 s max  (100.00%)
Lock Time (s) : 0 total, 0 avg, 0 to 0 max  (0.00%)
Rows sent     : 1 avg, 1 to 1 max  (100.00%)
Rows examined : 0 avg, 0 to 0 max  (0.00%)
Database      : 
Users         : 
        root@localhost  : 100.00% (2) of query, 100.00% (2) of all users

Query abstract:
SET timestamp=N; SELECT sleep(N) FROM DUAL;

Query sample:
SET timestamp=1513152652;
select sleep(3) from dual;
</pre>

- 1、总的查询次数（queries）  去重后的SQL数量（unique）
- 2、输出报表的内容排序：Sorted by ‘t_sum‘   最重大的慢sql统计信息, 包括 平均执行时间, 等待锁时间, 结果行的总数, 扫描的行总数
- 3、Count: sql的执行次数及占总的slow log数量的百分比
- 4、Time: 执行时间, 包括总时间, 平均时间, 最小, 最大时间, 时间占到总慢sql时间的百分比
- 5、95% of Time: 去除最快和最慢的sql, 覆盖率占95%的sql的执行时间
- 6、Lock Time: 等待锁的时间
- 7、95% of Lock: 95%的慢sql等待锁时间.  
- 8、Rows sent: 结果行统计数量, 包括平均, 最小, 最大数量
- 9、Rows examined: 扫描的行数量 
- 10、Database: 属于哪个数据库
- 11、Users: 哪个用户,IP, 占到所有用户执行的sql百分比
- 12、Query abstract: 抽象后的sql语句
- 13、Query sample: sql语句

## 参考资料

- [http://www.ppzedu.com/archives/954.html](http://www.ppzedu.com/archives/954.html)
