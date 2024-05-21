Spring Boot & Spring Cloud

Boot problems

*Project-Zillow*

**module cant start**

Content:

in value \"\$fzillow.banner .nginx.prefix\'\" .... .
AutowiredAmnotationBeanostProcesso\"SAutoui redFieldEl ement.,
resolveFieldvalue

Solution:

in line \@Value(\"\${zillow.banner.nginx.prefix}\"), this line is from
the only available yml file in the module. If u want is functionable,
make sure the only available yml file has this line configured.

correction

the yml file should be stored in cloud(GitHub), but hasn't or cant
connect to it (maybe VPM factor), so nothing to read!!!

**module cant start**

Content:

ERROR \-\-- \[main \] org.springframework.boot.SpringApplication
:Application run failed

org.springframework.beans.factory.BeanCreationException: Error creating
bean with name \'bannerController\': Lookup method resolution failed;
nested exception is java.lang.IllegalStateException: Failed to
introspect Class \[com.example.controller

Solution:

make sure the config center and gateway are running before starting
service

clean install maven

refresh maven

**sub module cant start:**

Content:

Error: Could not find or load main class com.example.XXX

Solution:

clean install parant module

Request sending

Method Not Allowed

content

{

\"timestamp\": \"2023-11-11T10:00:53.410+00:00\",

\"status\": 405,

\"error\": \"Method Not Allowed\",

\"path\": \"/comment/addComment\"

}

Solution:

check gateway

Gateway

the service is functional, but the gateway cant proxy it

Solution:

in the gateway yml file

Check whether the "uri:" are following with the correct service module
name.

Or predicates: - Path= stuff. **only 1 path can be config.** u may use
\* to proxy all interface

the service is functional, but the gateway access is a timeout

Solution:

restart service

**-**

Solution:

![](./image1.png){width="1.4926968503937008in"
height="1.8675754593175853in"}

kill the dash, its illegal for nothing

Unsorted kind

Weird fucking errors after some config changes

Solution:

**delete target file, refresh the cache, target file is the cache!!!**

clean install maven

refresh maven

*Project-Trip Advisor*

Uploaded images not showing

Check whether there is space in file path. The space is the god dame
cause!!!!!

Eureka:

*Project-Zillow*

unable to send heartbeat！

Content

\>ERROR \-\-- \[DiscoveryClient-HeartbeatExecutor-0\]
com.netflix.discovery. DiscoveryClient :DiscoveryClient \_

ZILLOW-BANNER/172.20.10.11:zillow-banner:9000 - was unable to send
heartbeat!

com.netflix.discovery.shared.transport.TransportException: Cannot
execute request on any known server

Explanation:

This is because currently the service is register on local Eureka. The
remote Eureka is not configured in config file yml

The service cant find the Eureka to register it self

Solution:

**before sloved**

server:\
port: 9004\
spring:\
application:\
name: zillow-deatail\
profiles:\
active: mongodb,bannerNginx ,redis\
cloud:\
config:\
uri: http://localhost:9010\
label: master\
name: Zillow\
profile: dev

**solved:**

server:\
port: 9004\
spring:\
application:\
name: zillow-deatail\
profiles:\
active: mongodb,bannerNginx ,redis\
cloud:\
config:\
uri: http://localhost:9010\
label: master\
name: Zillow\
profile: dev\
[eureka:\
client:\
service-url:\
defaultZone: <http://111.231.19.137:8761/eureka/>]{.mark}

/

Linux-Centos

VM Login

REMOTE HOST IDENTIFICATION HAS CHANGED

Content

**\>WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!**

Resolve steps

Solution

**S1:open file**

\~/.ssh/known_hosts

**S2: kill all records about the server IP trying to access**

*Project-Yelp Camp*

Docker container

Docker Redis stop right away after start

Content:

WRMIIG Your kernel has a bug that could lead to doto corrution during
bockground save. Please upgrode to the lotest stoble kemnel.

Explanation:

Redis cant match ARM server. U needs to do some special configs

Solution

这个问题就是arm版本的redis，有个配置需要调整（ignore-warnings
ARM64-COW-BUG），不然启动会报错导致启动不起来。

所以我是这么做的，以下几个步骤：

1、先去下载一个7.0的版本redis配置文件（因为docker里面的redis没有配置文件）（https://raw.githubusercontent.com/redis/redis/7.0/redis.conf）

2、将配置文件放到远程服务器的 /root/redis/redis.conf 路径下

3、调整配置文件内容，将84行（bind 127.0.0.1 -::1 ）代码注释， 将
2276行（ignore-warnings ARM64-COW-BUG）代码前面的注释去掉

4、使用镜像重新生成一个redis的容器（docker run -itd \--network=yelpcamp
-v /root/redis:/usr/local/etc/redis
prd_yelpcamp_svc_redis_python:1.0.0redis-server
/usr/local/etc/redis/redis.conf）

*Project-Zillow*

Remote server under attack (injected mining virus)

Redis

Spring Boot problems

*Project-Zillow*

module cant start: Failed to serialize

Content

ERROR \-\-- \[http-nio-9004-exec-6\]
o.a.c.c.C.\[.\[localhost\].\[/\].\[dispatcherServlet\]
:Servlet.service() for servlet \[dispatcherServlet\] in context with
path \[\] threw exception \[Request processing failed; nested exception
is org.springframework.data.redis.serializer.SerializationException:
Cannot serialize; nested exception is
org.springframework.core.serializer.support.SerializationFailedException:
Failed to serialize object using DefaultSerializer; nested exception is
java.lang.IllegalArgumentException: DefaultSerializer requires a
Serializable payload but received an object of type

Solution

make sure the corresponding

![](./image2.png){width="4.420176071741032in"
height="2.459382108486439in"}

**correction:**

"getDetail" here (\'detail(\'+#id+\')\') shell align with the request
mapping in controller

module cant start: Cannot deserialize

Content

ERROR \-\-- \[http-nio-9004-exec-1\]
o.a.c.c.C.\[.\[localhost\].\[/\].\[dispatcherServlet\]
:Servlet.service() for servlet \[dispatcherServlet\] in context with
path \[\] threw exception \[Request processing failed; nested exception
is org.springframework.data.redis.serializer.SerializationException:
Cannot deserialize; nested exception is
org.springframework.core.serializer.support.SerializationFailedException:
Failed to deserialize payload. Is the byte array a result of
corresponding serialization for DefaultDeserializer?; nested exception
is java.io.InvalidClassException: com.example.entity.Item; local class
incompatible: stream classdesc serialVersionUID = 5505599122515043000,
local class serialVersionUID = 5106356723969150989\] with root cause

Solution

After the the entity is changed, the data in Redis is a unmatch

U should delete everything in Redis then retry

➜ \~ docker exec -it dev_zilliow_svc_redis redis-cli

127.0.0.1:6379\> AUTH root

OK

127.0.0.1:6379\> FLUSHDB

OK

module cant start: NullPointerException

Content

ERROR 37507 \-\-- \[nio-9008-exec-3\]
o.a.c.c.C.\[.\[.\[/\].\[dispatcherServlet\] : Servlet.service() for
servlet \[dispatcherServlet\] in context with path \[\] threw exception
\[Request processing failed; nested exception is
java.lang.NullPointerException\] with root cause

Solution

make sure following are align

![](./image4.png){width="3.934712379702537in"
height="0.9910094050743657in"}

![](./image10.png){width="2.425715223097113in"
height="1.436748687664042in"}

MongoDB

Spring Boot problems

*Project-Zillow*

Found no data wanted

Solution

When MongoDB in your spring project trying to make a move, of course it
needs to know operate to which collection. To assign the collection, it
can be perform by "**Order.class**" in
"mongoTemplate.find(query,**Order.class**);" "Order" here should have
the exactly the same name with the collection u have in MongoDB.

Otherwise it's a not match, leads a to unexpected result. **MAKE SURE
THESE TWO ARE CORRESPONDING**

RabbitMQ

Spring Boot problems

*Project-Zillow*

The massage is not sent to RabbitMQ, when u checking the queue, u found
its empty

Solution

Make sure the service handling the RabbitMQ operation has its yml file
reading the following:

spring:\
rabbitmq:\
host: localhost\
username: admin\
password: admin

Vue

*Project-Zillow*

Npm start: Permission denied

Content:

➜ livegoods-vue sudo npm start

\> livegoods@0.1.0 start

\> concurrently \"npm run serve\" \" nodemon mock/index.js \"

\[1\] /bin/sh:
/Users/A-FILES/Coding/Temp/APPS/livegoods-vue/node_modules/.bin/nodemon:
Permission denied

\[1\] nodemon mock/index.js exited with code 126

\[0\]

\[0\] \> livegoods@0.1.0 serve

\[0\] \> vue-cli-service serve

\[0\]

\[0\] sh:
/Users/A-FILES/Coding/Temp/APPS/livegoods-vue/node_modules/.bin/vue-cli-service:
Permission denied

\[0\] npm run serve exited with code 126

Solution

sudo chmod -R u+rwx

errors after some commands execute

Solution

**S1: remove node_modules**

![](./image11.png){width="1.5262325021872265in"
height="0.2086220472440945in"}

**S2: reinstall npm**

npm install

**having trouble running project downloads from other sources**

Solution

rm -rf node_modules

rm package-lock.json

npm install

npm run serve

Elastic Search

*Project-Zillow*

Can\'t run es due to not finding java

Content:

![](./image12.png){width="7.042433289588802in"
height="0.3546117672790901in"}

Solution

**S1: ensure user \"es\" has the permission to the path
\"jkd/bin/java\"**
