# spring集成solrj或则spring-data-solr

## 集成spring-data-solr

- jar文件

```
<!-- solr -->
<dependency>
    <groupId>org.apache.solr</groupId>
    <artifactId>solr-solrj</artifactId>
    <version>${solrj.version}</version>
</dependency>
<dependency>
    <groupId>org.apache.solr</groupId>
    <artifactId>solr-core</artifactId>
    <version>${solrCore.version}</version>
</dependency>
<!-- spring-data-solr -->
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-solr</artifactId>
    <version>${spring-data-solr.version}</version>
</dependency>
```
- 与spring集成使用

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
	">
    <!-- 引入配置文件  -->
    <context:property-placeholder location="classpath:solr.properties" />
    <!--json返回对象-->
    <bean id="jsonMapResponseParser" class="org.apache.solr.client.solrj.response.DelegationTokenResponse.JsonMapResponseParser"/>
    <!-- 初始化slor对象客户端-->
    <bean id="httpSolrClient" class="org.apache.solr.client.solrj.impl.HttpSolrClient">
        <constructor-arg name="builder" value="${solr.baseUrl}"/>
    </bean>
    <!--solrTemplate-->
    <bean id="solrTemplate" class="org.springframework.data.solr.core.SolrTemplate">
        <constructor-arg name="solrClient" ref="httpSolrClient"/>
    </bean>
</beans>
```

- 具体说明

## 集成solrj

- jar文件

```
<!-- solr -->
<dependency>
    <groupId>org.apache.solr</groupId>
    <artifactId>solr-solrj</artifactId>
    <version>${solrj.version}</version>
</dependency>
<dependency>
    <groupId>org.apache.solr</groupId>
    <artifactId>solr-core</artifactId>
    <version>${solrCore.version}</version>
</dependency>
```

- 与spring集成使用

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:solr="http://www.springframework.org/schema/data/solr"
       xsi:schemaLocation="http://www.springframework.org/schema/data/solr
        http://www.springframework.org/schema/data/solr/spring-solr-1.0.xsd
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

	<!-- 引入配置文件  -->
    <context:property-placeholder location="classpath:solr.properties" />

    <bean id="httpSolrClient" class="org.apache.solr.client.solrj.impl.HttpSolrClient">
        <constructor-arg name="builder" value="${solr.baseUrl}"/>
        <!-- 设置响应解析器 -->
        <property name="parser">
            <bean class="org.apache.solr.client.solrj.impl.XMLResponseParser"/>
        </property>
        <!-- 建立连接的最长时间 -->
        <property name="connectionTimeout" value="3000"/>
    </bean>
</beans>
```

