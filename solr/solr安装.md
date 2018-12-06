# solr安装

## 软件

[solr6.6.5 download](http://archive.apache.org/dist/lucene/solr/)
Tomcat8
Java8

## 安装步骤

*按照个人习惯，`/opt/setups`用来存放各种软件安装包，`/usr/local`用于存放解压后的软件包。*

- 解压solr文件：`tar -zxvf solr-6.6.5.tgz`
- 修改解压文件：`mv solr-6.6.5 solr`
- 复制solr下的文件至tomcat:
	- 将 solr 压缩包中`solr/server/solr-webapp/`文件夹下webapp文件夹移动至`tomcat/webapps`目录：`mv /opt/setups/solr/server/solr-webapp/webapp /usr/local/tomcat/webapps`
	- 将 solr 压缩包中`solr/server/lib/ext`中的 jar 全部复制到`tomcat/webapps/solr/WEB-INF/lib`目录中：`mv /opt/setups/solr/server/lib/ext/* /usr/local/tomcat/webapps/solr/WEB-INF/lib`
	- 将 solr 压缩包中`solr/server/lib/metrics*`开头的jar全部复制到 `tomcat\webapps\solr\WEB-INF\lib` 目录中：`mv /opt/setups/solr/server/lib/metrics* usr/local/tomcat/webapps/solr/WEB-INF/lib`
	- 将solr压缩包中`solr/dist/solr-dataimporthandler-*` 开头的jar全部复制到 `tomcat\ webapps\solr\WEB-INF\lib` 目录中：`mv /opt/setups/solr/dist/solr-dataimporthandler-* usr/local/tomcat/webapps/solr/WEB-INF/lib`
- 在`tomcat/webapps/solr/WEB-INF`下建立classes目录：`mkdir /usr/local/tomcat/webapps/solr/WEB-INF/classes`
- 将`solr/server/resources/log4j.properties`文件复制至classes目录：`mv /opt/setups/solr/server/resources/log4j.properties /usr/local/tomcat/webapps/solr/WEB-INF/classes`
- 创建solr的主目录：`mkdir /usr/local/solrhome`
- 复制`solr/server/solr/*` 所有文件到`solrhome`目录，用到创建solr的core时使用：`mv /opt/setups/solr/server/solr/* /usr/local/solrhome`

## 配置solr

- 编辑web.xml文件：`vim /usr/local/tomcat/webapps/solr/WEB-INF/web.xml`
- 配置solr下core路径：
```
<env-entry>
   <env-entry-name>solr/home</env-entry-name>
   //将路径指向我们创建的solrhome目录。
   <env-entry-value>/usr/local/solrhome</env-entry-value> 
   <env-entry-type>java.lang.String</env-entry-type>
</env-entry>
```
- 配置访问权限，注释权限访问：
```
<!--
  <security-constraint>
    <web-resource-collection>
      <web-resource-name>Disable TRACE</web-resource-name>
      <url-pattern>/</url-pattern>
      <http-method>TRACE</http-method>
    </web-resource-collection>
    <auth-constraint/>
  </security-constraint>
  <security-constraint>
    <web-resource-collection>
      <web-resource-name>Enable everything but TRACE</web-resource-name>
      <url-pattern>/</url-pattern>
      <http-method-omission>TRACE</http-method-omission>
    </web-resource-collection>
  </security-constraint>
-->
```

## 启动

- 启动tomcat：`./start`
- 访问solr界面：`http://localhost:3501/solr/index.html`

## 创建core

- 创建索引库
![](https://i.imgur.com/8o6tnhO.png)
如果按照上述方式直接创建，此刻会出现报错问题。
```
Error CREATEing SolrCore 'solr_core': Unable to create core [goods] Caused by: Can't find resource 'solrconfig.xml' in classpath or '/usr/local/tomcat-solr/solrhome/solr_core'
```
- 错误问题解决方案
	- 在solrhome目录下删除之前创建无效的索引库文件：`rm -rf /usr/local/solrhome/solr_core`
	- 创建新的索引库文件：`mkdir -p /usr/local/solrhome/solr_core`
	- 拷贝_default下的conf目录至`solr_core`：`cp -R /opt/setups/solr/server/solr/configsets/basic_configs/* /usr/local/solrhome/solr_core/`
	- 重启服务

## 中文分词器

- [ikanalyzer 中文分词器](https://blog.csdn.net/qq_28114645/article/details/77961998)
- [Solr的中英文分词实现](https://www.cnblogs.com/knitmesh/p/5439713.html)
- [内置中文分词器](https://blog.csdn.net/jiadajing267/article/details/78702158)

## 参考资料
- [https://www.linuxidc.com/Linux/2017-12/149898.htm](https://www.linuxidc.com/Linux/2017-12/149898.htm)

- [https://blog.csdn.net/l1028386804/article/details/70199983](https://blog.csdn.net/l1028386804/article/details/70199983)