---
title: python高级学习日志第二、三天之网络编程
date: 2019-01-08
tags: Python
---
<meta name="referrer" content="no-referrer" />

##  python高级学习日志第二、三天之网络编程
###  UDP接收数据以及基于UDP的交易聊天器
> 一个Java程序员，但是打算自学一下python，所以把自己学到的东西记录在这里。也免得我自己忘记新学的，年纪大了真的很容易忘东西。

哈哈，我喜欢边写程序边听歌
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=502839372&auto=0&height=66"></iframe>

上次记录的是怎么用udp来发送数据，通过网络调试助手按钮来模拟数据接收方，这次记录的是利用udp代码来接收数据，以及基于两份python代码的简易聊天器。
>话不多说直接开搞

首先是单纯的用udp来接收数据，这里同样用网络调试助手来模拟发送数据，这个时候网络调试助手扮演的角色是发送数据方。

>利用udp接收数据有以下几个主要步骤：

 1. 创建套接字
 2. 绑定本地信息，包括本地ip以及分配端口号
 3. 接收数据
 4. 打印接收到的数据
 5. 关闭套接字
 
 

```python
import socket


def main():
    # 1.创建套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 2.绑定一个本地信息，分配端口号
    localaddr = ("", 7788)
    udp_socket.bind(localaddr)
    # 3.接收数据
    recv_data = udp_socket.recvfrom(1024)
    # 4.打印接收到的数据
    print(recv_data)
    # 5.关闭套接字
    udp_socket.close()


if __name__ == '__main__':
    main()

```

>在绑定本地信息过程中，套接字调用bind()方法，其中绑定的内容是一个元组，(",xxxx),元组的第一个值为本地ip，不用些即可，代码运行时会自动填充，第二个是手动给这个接收的代码片运行分配一个端口号，可在1024至65535之间随意填写，除本机有程序已经在占用这个端口使用以外，这里我随机绑定的端口号是7788，至于元组的第一个元素为什么带有引号，是因为他是一个字符串类型的参数。
>绑定好本地信息以后准备接受来自其它udp发送来的数据
>recv_data = udp_socket.recvfrom(1024)
>调用套接字的recvfrom方法，1024是指最大接收的数据量，这里用1Kb测试，最后把接收到的数据存进变量
recv_data里，然后运行程序：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108142647212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
此时没有任何动作是因为程序正在等待接收数据
然后打开网络调试助手新建一个udp来模拟发送数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108142911501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
同样的把相关配置做好，1号处填上本地的ip地址，2好处给网络调试助手启动服务器分配一个端口，3号处也是填上本地ip地址，因为是同一台电脑发送数据和接收数据，4号处填上代码里绑定的端口号，也就是python代码运行分配的端口号，所以整个过程相当于启动网络调试助手的udp作为数据发送方，然后填好要发给的另一方的信息。启动调试助手：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108143524867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
输入测试发送的信息点击发送：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019010814362627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
回到代码运行处查看：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108143653739.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
可以看到显示的是一个大元组，大元组的第一个袁术是接收到的数据，在这里是中文的ASCII码，时byte类型的数据，然后后面跟着一组小元组，小元组的内容显而易见，小元组的第一个元素是数据发送方的ip，第二个元素是数据发送方的端口号。接下来对代码做一些改变：
```python
import socket


def main():
    # 1.创建套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 2.绑定一个本地信息，分配端口号
    localaddr = ("", 7788)
    udp_socket.bind(localaddr)
    # 3.接收数据

    recv_data = udp_socket.recvfrom(1024)
    # 解析元祖的消息内容
    recv_msg = recv_data[0]  # 存储接收到的数据
    send_addr = recv_data[1]  # 存储发送方的地址信息
    # 4.打印接收到的数据
    print("%s说:%s" % (str(send_addr), recv_msg.decode("gbk")))
    # 5.关闭套接字
    udp_socket.close()


if __name__ == '__main__':
    main()

```
>把原本的recv_data做一个拆分解析，把recv_data的第一个元素赋值给recv_msg，表示收到的消息和数据。把recv_data的第二个元素赋值给send_addr，是一个小元组，表示发送方的信息，其过程和java数组取值类似，最后分别做输出，需要注意的是，str强转和接收到的数据转码。最后重新运行程序：
>![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108144956841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
等待收数据，然后在调试助手输入发送的数据：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108145034234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
则代码最后运行结果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108145056413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
以上这种情况可以看出是收到一条数据则结束程序运行。
接下来对代码进行改进能让代码持续收到书：

```python
import socket


def main():
    # 1.创建套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 2.绑定一个本地信息，分配端口号
    localaddr = ("", 7788)
    udp_socket.bind(localaddr)
    # 3.循环接收数据
    while True:
        recv_data = udp_socket.recvfrom(1024)
        # 解析元祖的消息内容
        recv_msg = recv_data[0]  # 存储接收到的数据
        send_addr = recv_data[1]  # 存储发送方的地址信息
        # 4.打印接收到的数据
        print("%s说:%s" % (str(send_addr), recv_msg.decode("gbk")))
    # 5.关闭套接字
    udp_socket.close()


if __name__ == '__main__':
    main()

```
>可以看出是将套接字收数据和打印数据作为循环体来循环，这个时候测试数据接收如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108145507304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
在网络调试助手连续发送三条信息，代码运行都够收到以后，程序并未退出这个时候就做到了持续等待收数据和信息的效果。

学习了udp收发数据以后，可以做一个基于udp的简单聊天器了，这个聊天器是发一句收一句的，因为还没有采取多线程的处理方式。具体代码如下，这个时候就不需要调试助手来充当角色了：

```python
import socket

def send_msg(udp_scoket,dest_ip,dest_port):
    """"发送消息"""
    #获取要发送的内容
    send_data=input("请输入要发送的消息:")
    udp_scoket.sendto(send_data.encode("gbk"), (dest_ip, int(dest_port)))


def recv_msg(udp_scoket):
    """"接收消息"""
    recv_data=udp_scoket.recvfrom(1024)
    print("%s回消息说:%s" %(str(recv_data[1]),recv_data[0].decode("gbk")))


def main():
    #创建套接字
    udp_scoket=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)

    #绑定信息
    #绑定的是一个元组，里面存放ip信息和端口信息
    udp_scoket.bind(("",7788))
    # 确定对方地址
    dest_ip = input("请输入对方的ip:")
    dest_port = input("请输入对方的port:")

    #循环发送接收消息
    while True:
            #发送
            send_msg(udp_scoket,dest_ip,dest_port)

            #接受并显示
            recv_msg(udp_scoket)
if __name__ == '__main__':
    main()
```
这里测试自己收发
程序运行如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190108150039481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ4Nzk1,size_16,color_FFFFFF,t_70)
可以看出是做到了自己收发消息了的，即在一个程序里，自己发送数据给自己自己接收数据并打印显示，当然也可以用两份程序分别演示一个发送数据，一个接收数据。具体看自己怎么理解和操作。

>下一次记录tcp的有关知识和实践
