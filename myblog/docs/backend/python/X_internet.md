# 网络

[[toc]]

在讨论并发时，主要讨论的是时间：单机解决方案(进程、线程和绿色线程)。还简单但是绝对了网络化的解决方案(Redis)。现在我们来单独介绍一下网络化，也就是跨空间的分布式计算。

## 模式

你可以使用一些基础的模式来搭建网络化应用。

最常见的`模式`是`请求-响应`，也被称为`客户端-服务器`模式，这个模式是同步的，用户会一直等待服务器的响应。你的web浏览器就是一个客户端，向web服务器发起一个HTTP请求并等待响应。

另一种常见的模式是`推送或者扇出`，你把数据发送到一个进程池中，空闲的工作进程会进行处理，一个典型的例子是有负载均衡的web服务器。

和推送相反的是`拉取或者扇入`，你从一个或者多个源接收数据，一个典型的例子是记录器，它会从多个进程接收文本信息并把它们写入一个日志文件中。

还有一个和收音机或者电视广播很像的模式`发布-订阅`模式，这个模式中，会有发送数据的发布者，在简单的发布-订阅系统中，所有的订阅者都会收到一份副本，更常见的情况是，订阅者只关心特定类型的数据(通常被称为话题)，发布者只会发送这些数据，因此，和推送模式不同，可能会有超过一个订阅者收到数据。如果一个话题没有订阅者，相关的数据会被忽略。

### 发布-订阅模式

发布-订阅模式并不是队列，而是广播。一个或多个进程发布信息，每个订阅进程声明自己感兴趣的消息类型，然后每个消息都会被复制一份发给感兴趣的订阅进程。因此，一个消息可能只被处理一次，也可能多于一次，还可能完全不被处理。每个发布者只负责广播，并不知道谁(如果有人的话)在监听。


#### Redis发布订阅模式

你可以使用Redis搭建一个发布订阅系统。发布者会发出包含话题和值的消息，订阅者会声明它们对什么话题感兴趣。

下面是发布者，redis_pub.py:

```py
import redis
import random

conn = redis.Redis()

cats = ['catA', 'catB', 'catC', 'catD']
hats = ['hatA', 'hatB', 'hatC', 'hatD']
for msg in range(10):
    cat = random.choice(cats)
    hat = random.choice(hats)
    print('Publish: %s wears a %s' % (cat, hat))
    conn.publish(cat, hat)
```

每个话题是猫🐱的一个品种，每个消息的值是帽子的一种类型。

下面是订阅者，redis_sub.py:

```py
import redis
conn = redis.Redis()
topics = ['catA', 'catC']
sub = conn.pubsub()
sub.subscribe(topics)
for msg in sub.listen():
    print(msg)
    if msg['type'] == 'message':
        cat = msg['channel']
        hat = msg['data']
        print('Subscribe: %s wears a %s' % (cat, hat))
    import time
    time.sleep(1)
```

订阅者只会展示猫的品种为`catA`或者`catC`的消息，`listen()`方法会返回一个字典，如果它的类型是`message`消息，那就是由发布者发出的消息，`channel`键是话题(猫的种类)，`data`键包含消息的值(帽子)。

注意一点，如果先启动发布者，这时没有订阅者，就像电台发布广播却没有听从一样。我们需要先启动订阅者：

```sh
/usr/local/bin/python3 redis_sub.py
{'type': 'subscribe', 'pattern': None, 'channel': b'catA', 'data': 1}
{'type': 'subscribe', 'pattern': None, 'channel': b'catC', 'data': 2}
```

此时，订阅者在监听发布者发布的消息，一直等待发布者发布消息，我们再启动发布者，它会发布10个消息：

```sh
/usr/local/bin/python3 redis_pub.py
Publish: message 0 : catB wears a hatA
Publish: message 1 : catC wears a hatA
Publish: message 2 : catA wears a hatD
Publish: message 3 : catA wears a hatC
Publish: message 4 : catB wears a hatB
Publish: message 5 : catD wears a hatC
Publish: message 6 : catD wears a hatD
Publish: message 7 : catA wears a hatA
Publish: message 8 : catD wears a hatD
Publish: message 9 : catB wears a hatD

Process finished with exit code 0
```

这时，可以在订阅者运行的控制台可以看到监听到了消息：

```sh
/usr/local/bin/python3 redis_sub.py
{'type': 'subscribe', 'pattern': None, 'channel': b'catA', 'data': 1}
{'type': 'subscribe', 'pattern': None, 'channel': b'catC', 'data': 2}
{'type': 'message', 'pattern': None, 'channel': b'catC', 'data': b'hatA'}
Subscribe: b'catC' wears a b'hatA'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatD'}
Subscribe: b'catA' wears a b'hatD'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatC'}
Subscribe: b'catA' wears a b'hatC'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatA'}
Subscribe: b'catA' wears a b'hatA'
```
此时可以看到，订阅者获取到的消息与发布者发布的消息的顺序是相同的，获取到发布者的以下消息：

```sh
Publish: message 1 : catC wears a hatA
Publish: message 2 : catA wears a hatD
Publish: message 3 : catA wears a hatC
Publish: message 7 : catA wears a hatA
```

我们并没有让订阅者退出，因此它会一直等待消息。如果重新启动一个发布者，那订阅者会继续抓取消息并输出。我们重新运行一下redis_pub.py,在redis_sub.py控制台可以看到订阅者仍然可以接收到新的消息:


```sh
/usr/local/bin/python3 redis_sub.py
{'type': 'subscribe', 'pattern': None, 'channel': b'catA', 'data': 1}
{'type': 'subscribe', 'pattern': None, 'channel': b'catC', 'data': 2}
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatC'}
Subscribe: b'catA' wears a b'hatC'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatB'}
Subscribe: b'catA' wears a b'hatB'
{'type': 'message', 'pattern': None, 'channel': b'catC', 'data': b'hatA'}
Subscribe: b'catC' wears a b'hatA'
{'type': 'message', 'pattern': None, 'channel': b'catC', 'data': b'hatB'}
Subscribe: b'catC' wears a b'hatB'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatD'}
Subscribe: b'catA' wears a b'hatD'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatC'}
Subscribe: b'catA' wears a b'hatC'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatC'}
Subscribe: b'catA' wears a b'hatC'
{'type': 'message', 'pattern': None, 'channel': b'catA', 'data': b'hatD'}
Subscribe: b'catA' wears a b'hatD'
{'type': 'message', 'pattern': None, 'channel': b'catC', 'data': b'hatA'}
Subscribe: b'catC' wears a b'hatA'
{'type': 'message', 'pattern': None, 'channel': b'catC', 'data': b'hatD'}
Subscribe: b'catC' wears a b'hatD'
{'type': 'message', 'pattern': None, 'channel': b'catC', 'data': b'hatA'}
Subscribe: b'catC' wears a b'hatA'
{'type': 'message', 'pattern': None, 'channel': b'catC', 'data': b'hatB'}
Subscribe: b'catC' wears a b'hatB'
```

### ZeroMQ发布订阅模式

ZeroMQ没有核心的服务器，因此每个发布者都会发送给所有订阅者。我们来使用ZeroMQ重写一下上面的猫-帽子示例。

安装ZeroMQ python包：

```sh
pip install pyzmq
```

参考：[https://zeromq.org/languages/python/](https://zeromq.org/languages/python/)

发布者为zmq_pub.py，内容如下：

```py
import zmq
import random
import time

host = '*'
port = 6789
ctx = zmq.Context()
pub = ctx.socket(zmq.PUB)
pub.bind('tcp://%s:%s' % (host, port))

cats = ['catA', 'catB', 'catC', 'catD']
hats = ['hatA', 'hatB', 'hatC', 'hatD']
time.sleep(1)

for msg in range(10):
    cat = random.choice(cats)
    hat = random.choice(hats)
    cat_bytes = cat.encode('utf-8')
    hat_bytes = hat.encode('utf-8')
    print('Publish: message %s : %s wears a %s' % (msg, cat, hat))
    pub.send_multipart([cat_bytes, hat_bytes])
```

注意代码是如何用UTF-8来编码话题和值字符串的。

下面是订阅者zmq_sub.py:

```py
import zmq

host = '127.0.0.1'
port = 6789
ctx = zmq.Context()
sub = ctx.socket(zmq.SUB)
sub.connect('tcp://%s:%s' % (host, port))

topics = ['catA', 'catC']
for topic in topics:
    sub.setsockopt(zmq.SUBSCRIBE, topic.encode('utf-8'))

while True:
    cat_bytes, hat_bytes = sub.recv_multipart()
    cat = cat_bytes.decode('utf-8')
    hat = hat_bytes.decode('utf-8')
    print('Subscribe: %s wears a %s' % (cat, hat))
```

在这段代码中，我们订阅了两个不同的比我值，用UTF-8编码的topics中的两个字符串。

假如你要订阅所有话题，需要订阅空比特字符串`b''`，否则什么消息都得不到。

注意，我们在发布者中调用了`send_multipart()`，在订阅者中调用了`recv_multipart()`，这样就可以收到消息的多个部分并使用第一部分来判断话题是否匹配。也可以选择使用一个字符串或者比特字符串来发送话题和消息值，但是把猫和帽子分开发送会更加清晰。

像Redis消息订阅一样，我们先启动订阅者，再启动发布者。

启动订阅者：

```sh
/usr/local/bin/python3 zmq_sub.py
```

此时订阅者会一直等待发布者发送消息，程序一直运行中，控制台没有其他输出。

启动发布者，它会立即发布10条消息并退出：

```sh
/usr/local/bin/python3 zmq_pub.py
Publish: message 0 : catD wears a hatA
Publish: message 1 : catB wears a hatC
Publish: message 2 : catD wears a hatD
Publish: message 3 : catC wears a hatB
Publish: message 4 : catC wears a hatB
Publish: message 5 : catA wears a hatC
Publish: message 6 : catC wears a hatD
Publish: message 7 : catC wears a hatD
Publish: message 8 : catD wears a hatD
Publish: message 9 : catA wears a hatA

Process finished with exit code 0
```

这时，可以在订阅者控制台看到输出了它想要的内容：

```sh
/usr/local/bin/python3 zmq_sub.py
Subscribe: catC wears a hatB
Subscribe: catC wears a hatB
Subscribe: catA wears a hatC
Subscribe: catC wears a hatD
Subscribe: catC wears a hatD
Subscribe: catA wears a hatA
```

我们再运行一次发布者，这时再在订阅者控制台查看，发现会多出几条输出消息：

```sh
/usr/local/bin/python3 /Users/mzh/PycharmProjects/base/zmq_sub.py
Subscribe: catC wears a hatB
Subscribe: catC wears a hatB
Subscribe: catA wears a hatC
Subscribe: catC wears a hatD
Subscribe: catC wears a hatD
Subscribe: catA wears a hatA (# 以下4条输出是多出的消息)
Subscribe: catA wears a hatB
Subscribe: catA wears a hatB
Subscribe: catC wears a hatD
Subscribe: catA wears a hatC
```
