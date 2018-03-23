# nginx 配置



## Nginx 全局变量

- $arg_PARAMETER #这个变量包含GET请求中，如果有变量PARAMETER时的值。
- $args #这个变量等于请求行中(GET请求)的参数，例如foo=123&bar=blahblah;
- $binary_remote_addr #二进制的客户地址。
- $body_bytes_sent #响应时送出的body字节数数量。即使连接中断，这个数据也是精确的。
- $content_length #请求头中的Content-length字段。
- $content_type #请求头中的Content-Type字段。
- $cookie_COOKIE #cookie COOKIE变量的值
- $document_root #当前请求在root指令中指定的值。
- $document_uri #与$uri相同。
- $host #请求主机头字段，否则为服务器名称。
- $hostname #Set to the machine’s hostname as returned by gethostname
- $http_HEADER
- $is_args #如果有$args参数，这个变量等于”?”，否则等于”"，空值。
- $http_user_agent #客户端agent信息
- $http_cookie #客户端cookie信息
- $limit_rate #这个变量可以限制连接速率。
- $query_string #与$args相同。
- $request_body_file #客户端请求主体信息的临时文件名。
- $request_method #客户端请求的动作，通常为GET或POST。
- $remote_addr #客户端的IP地址。
- $remote_port #客户端的端口。
- $remote_user #已经经过Auth Basic Module验证的用户名。
- $request_completion #如果请求结束，设置为OK. 当请求未结束或如果该请求不是请求链串的最后一个时，为空(Empty)。
- $request_method #GET或POST
- $request_filename #当前请求的文件路径，由root或alias指令与URI请求生成。
- $request_uri #包含请求参数的原始URI，不包含主机名，如：”/foo/bar.php?arg=baz”。不能修改。
- $scheme #HTTP方法（如http，https）。
- $server_protocol #请求使用的协议，通常是HTTP/1.0或HTTP/1.1。
- $server_addr #服务器地址，在完成一次系统调用后可以确定这个值。
- $server_name #服务器名称。
- $server_port #请求到达服务器的端口号。
- $uri #不带请求参数的当前URI，$uri不包含主机名，如”/foo/bar.html”。该值有可能和$request_uri 不一致。
- $request_uri是浏览器发过来的值。该值是rewrite后的值。例如做了internal redirects后。