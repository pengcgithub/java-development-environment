# docker.tomcat

docker exec -it ae8e426a0fb1 /bin/bash

docker run -d -p 8080:8080 -v /data/work/tomcat/wapei-admin.war:/home/tomcat/tomcat8/webapps/wapei-admin.war --name wapei-admin tomcattest:v1.0