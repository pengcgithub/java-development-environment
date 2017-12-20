# svn 安装和配置

## svn安装

## svn提交注释限制

- 进入svn代码仓库

`cd /usr/local/svn`

- hooks文件夹中的pre-commit.tmpl文件重命名为pre-commit

`mv ./hooks/pre-commit.tmpl ./hooks/pre-commit`

- 修改pre-commit文件

<pre>
REPOS="$1"  
TXN="$2"  
  
# Make sure that the log message contains some text.  
SVNLOOK=/usr/bin/svnlook  
  
LOGMSG=`$SVNLOOK log -t $TXN $REPOS | wc -m`       //定义个变量，注意这里不是单引号  
  
#$SVNLOOK log -t "$TXN" "$REPOS" | \               //把这一行和下面的一行注释掉  
# grep "[a-zA-Z0-9]" > /dev/null || exit 1  
  
echo $LOGMSG > /home/administrator/www/aaa.txt     //为了测试变量用的，查看$LOGMSG有没有值,最后要注释掉  
if [ "$LOGMSG" -lt 48 ]                            //这里为什么是48呢，一个汉字对应16个字符  
then  
 echo "\n至少输入4个汉字" >&2                        //必须填四个汉字  
 exit 1  
fi  
  
# Exit on all errors.  
#set -e
  
# Check that the author of this commit has the rights to perform  
# the commit on the files and directories being modified.  
#"$REPOS"/hooks/commit-access-control.pl "$REPOS" $TXN \    //把这一行和下面的一行注释掉。  
#  "$REPOS"/hooks/commit-access-control.cfg  
  
# All checks passed, so allow the commit.  
exit 0 
</pre>

- 设置pre-commit文件为可读写文件

`chmod +x pre-commit`

## 参考资料

- [CentOS 7中firewall防火墙详解和配置以及切换为iptables防火墙](http://blog.csdn.net/xlgen157387/article/details/52672988)

- [CentOS下使用yum安装配置和使用svn](https://my.oschina.net/junn/blog/164041)

- [svn添加提交注释限制](http://blog.csdn.net/cubesky/article/details/38754283)