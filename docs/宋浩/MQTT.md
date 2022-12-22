## MQTT

#### 1.M&Q

​	M = 消息

​	Q = 队列

#### 2.MQTT

###### Active MQ

​	Active MQ = 一个消息队列应用服务器（推送服务器）

![](https://pic2.zhimg.com/v2-dd5579d4e545784c852d08903416a02d_r.jpg)

```
Producer：消息生产者，负责产生和发送消息到 Broker；	

Broker：消息处理中心。负责消息存储、确认、重试等，一般其中会包含多个 queue；

Consumer：消息消费者，负责从 Broker 中获取消息，并进行相应处理；
```

```python
ActiveMQ能做什么
	1.实现两个不同应用之间的通讯
    2.实现同一个应用，不同模块之间的消息通讯

ActiveMQ特点
	1.支持多语言（Java,C,C++,C#,Ruby,Perl,Python,PHP）多协议客户端（OpenWire, Stomp REST, WS Notification, XMPP, AMQP）
    2.对Spring的支持，ActiveMQ可以很容易整合到Spring的系统里面去。
    3.支持高可用、高性能的集群模式。
```

##### 	Zeno MQ

```python
ZMQ简介：ZMQ看起来像是一个嵌入式网络连接库，但实际上是一个并发框架。框架提供的套接字可以满足在多种协议之间传输原子信息，如线程间、进程间、TCP、广播等。可以使用ZMQ构建多对多的连接方式，如扇出、发布-订阅、任务分发、请求-应答等。ZMQ的高速使得它能胜任分布式应用。它的异步I/O机制让你能够构建多核应用程序，完成异步消息处理任务。ZMQ有着多语言支持，并能在几乎所有的操作系统上运行。
```

​	三种基本模型

​	Request-Reply（请求-回复）请求应答模式

​	客户端发起请求，并等待服务端回应请求。客户端发送一个简单的 “Hello”，服务端则回应一个 “World”。可以有 N 个客户端，一个服务端，因此是 1-N 连接。

​	![](https://upload-images.jianshu.io/upload_images/1752522-bdc5fcaa6a2fa8c5.png?imageMogr2/auto-orient/strip|imageView2/2/w/234/format/webp)

​	Publish-Subscribe 发布-订阅模式

​	一个天气预报的例子来介绍该模式。服务端不断地更新各个城市的天气，客户端可以订阅自己感兴趣（通过一个过滤器）的城市的天气信息。

​	![](https://upload-images.jianshu.io/upload_images/1752522-3faf1defd6c84b82.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

​	Parallel Pipeline 分布式处理	

- 任务分发器会生成大量可以并行计算的任务；

- 有一组worker会处理这些任务；

- 结果收集器会在末端接收所有worker的处理结果，进行汇总。

  ![](https://upload-images.jianshu.io/upload_images/1752522-b407a578492a4ce5.png?imageMogr2/auto-orient/strip|imageView2/2/w/415/format/webp)

###### MQTT

```python
MQTT（Message Queuing Telemetry Transport，消息队列遥测传输协议），是一种基于发布/订阅（publish/subscribe）模式的“轻量级”通讯协议，该协议构建于TCP/IP协议上，由IBM在1999年发布。

MQTT最大优点在于，用极少的代码和有限的带宽，为连接远程设备提供实时可靠的消息服务。

作为一种低开销、低带宽占用的即时通讯协议，使其在物联网、小型设备、移动应用等方面有较广泛的应用。
```

MQTT协议特点

```python
MQTT是一个基于客户端-服务器的消息发布/订阅传输协议。
MQTT协议是轻量、简单、开放和易于实现的，这些特点使它适用范围非常广泛。在很多情况下，包括受限的环境中，如：机器与机器（M2M）通信和物联网（IoT）。
MQTT 与 HTTP 一样，MQTT 运行在传输控制协议/互联网协议 (TCP/IP) 堆栈之上。
MQTT使用的发布/订阅消息模式，它提供了一对多的消息分发机制，从而实现与应用程序的解耦。这是一种消息传递模式，消息不是直接从发送器发送到接收器（即点对点），而是由MQTT server（或称为 MQTT Broker）分发的。
MQTT 服务器是发布-订阅架构的核心。
```

QoS（Quality of Service levels）

```python
QoS 0 这一级别会发生消息丢失或重复，消息发布依赖于底层TCP/IP网络。即：<=1
QoS 1 承诺消息将至少传送一次给订阅者。
QoS 2 使用 QoS 2，我们保证消息仅传送到目的地一次。为此，带有唯一消息 ID 的消息会存储两次，首先来自发送者，然后是接收者。QoS 级别 2 在网络中具有最高的开销，因为在发送方和接收方之间需要两个流。
```

MQTT 数据包结构

```python
固定头（Fixed header），存在于所有MQTT数据包中，表示数据包类型及数据包的分组类标识；
可变头（Variable header），存在于部分MQTT数据包中，数据包类型决定了可变头是否存在及其具体内容；
消息体（Payload），存在于部分MQTT数据包中，表示客户端收到的具体内容；
```

![](https://pic3.zhimg.com/80/v2-861c089bea9570876bb13a031e6c3902_1440w.jpg)

协议版本3定义了14种 MQTT 报文，用于建立/断开连接、发布消息、订阅消息和维护连接。固定报头的第一字节的4-7位的值指定了报文类型，其取值如下表。

| **报文类型** | **值** | **描述**                 |
| ------------ | ------ | ------------------------ |
| CONNECT      | 1      | 客户端向代理发起连接请求 |
| CONNACK      | 2      | 连接确认                 |
| PUBLISH      | 3      | 发布消息                 |
| PUBACK       | 4      | 发布确认                 |
| PUBREC       | 5      | 发布收到（QoS2）         |
| PUBREL       | 6      | 发布释放（QoS2）         |
| PUBCOMP      | 7      | 发布完成（QoS2）         |
| SUBSCRIBE    | 8      | 客户端向代理发起订阅请求 |
| SUBACK       | 9      | 订阅确认                 |
| UNSUBSCRIBE  | 10     | 取消订阅                 |
| UNSUBACK     | 11     | 取消订阅确认             |
| PINGREQ      | 12     | PING请求                 |
| PINGRESP     | 13     | PING响应                 |
| DISCONNECT   | 14     | 断开连接                 |

#### 3.MQTT模型

![](https://tse4-mm.cn.bing.net/th/id/OIP-C.7MwXy5N4rx4mAxZ2KZrwJQHaEM?pid=ImgDet&rs=1)

#### 4.python MQTT使用

目前使用的是emqx 加 paho.mqtt

emqx官网：https://www.emqx.io/zh/downloads#broker

用emqx在windows上建立服务器，先下载适合的安装包，解压后进入bin目录，在cmd中使用命令 emqx.cmd start启动服务器，关闭服务器的命令是emqx.cmd stop。

启动服务器后，浏览器打开`http://127.0.0.1:18083`，使用默认管理账号`admin/public`即可登录查看，在设置里还能改成中文界面，很方便。

使用python 连接mqtt服务器，使用的模块是paho-mqtt,代码如下

```python

# 为了能在外部脚本中调用Django ORM模型，必须配置脚本环境变量，将脚本注册到Django的环境变量中
import os, sys,json
import django
import requests
# 第一个参数固定，第二个参数是工程名称.settings
os.environ.setdefault('DJANGO_SETTING_MODULE', 'djangoProject.settings')
django.setup()

# 引入mqtt包
import paho.mqtt.client as mqtt
# 使用独立线程运行
from threading import Thread
from .app01 import models
import time
import json
from .app01.serializers import AiBingSerializer

# 建立mqtt连接
def on_connect(client, userdata, flag, rc):
    print("Connect with the result code " + str(rc))
    client.subscribe('test', qos=0)

# 接收、处理mqtt消息
def on_message(client, userdata, msg):
    out = str(msg.payload.decode('utf-8'))
    print(out)
    out = json.loads(out)
    # 收到消息后执行任务
    if msg.topic:
        try:
            aibing = models.AiBing.objects.get(Code=out,IsDelete=False)
            print(aibing.IsUerd)
            if not aibing.IsUerd:
                res = 1
                res2 = json.dumps(res)
                client.publish('test', payload=res2, qos=1)
                models.AiBing.objects.filter(Code=out, IsDelete=False).update(IsUerd=True)
                return
            else:
                res = 0
                res2 = json.dumps(res)
                client.publish('test', payload=res2, qos=0)
                return
        except:
            print('不存在')


# mqtt客户端启动函数
def mqttfunction():
    global client
    # 使用loop_start 可以避免阻塞Django进程，使用loop_forever()可能会阻塞系统进程
    # client.loop_start()
    # client.loop_forever() 有掉线重连功能
    client.loop_forever(retry_first_connection=True)

client = mqtt.Client(client_id="test", clean_session=False)

# 启动函数
def mqtt_run():
    client.on_connect = on_connect
    client.on_message = on_message
    # 绑定 MQTT 服务器地址
    broker = '127.0.0.1'
    # MQTT服务器的端口号
    client.connect(broker, 1883, 600)
    # 启动
    mqttthread = Thread(target=mqttfunction)
    mqttthread.start()

# 启动 MQTT
# mqtt_run()

if __name__ == "__main__":
    mqtt_run()
```

#### 6 源码解析

```python
paho-mqtt 包含这些文件
clinet.py
matchar.py
packetty.py
properties.py
publish.py
reasoncodes.py
subscribe.py
subscribeoptions.py
先看client文件
包含五个obj五个fanc
obj：Client，MQTTMessage,MQTTMessageInfo,WebsocketConnectionError,WebscoketWrapper
fanc:_socketpair_compat(),base62(),connack_string(connack_code),error_string,topic_matches_sub()

Client中常用的方法：
client.loop:默认无限阻塞循环，不推荐直接使用。成功时返回MQTT_ERR_SUCCESS。出错返回值>0,会引发值错误。 具有 QoS>0 的消息。

Client.loop_forever:无限阻塞循环，已在程序中循环运行客户端，默认会重新连接。retry_first_connection:如果第一次连接尝试失败时重试。

Client.loop_start:线程客户端，调用一次启动一个新线程

Client.loop_stop:与上对应，调用一次停止start启动的线程客户端

Client.on_message:客户端收到有关主题每条消息都会调用，用于特定主题过滤器

Client.publish:（参数：self, topic, payload=None, qos=0, retain=False, properties=None）
topic：发布消息的主题
payload：默认为None 实际发布的消息，不能为int和float类型，应为str类型
qos:服务级别
retain：设置为True，则设置为最后一个保存的主题消息
properties：mqtt5.0的属性
```







