


在进行close之前，先flush一下。

对应writer里面将字符数组写进去，这个方法就是将字符串转化为字符型数组。
节点流类型：

导入文件：

读文件：                                            
 
bufferedInputStream:
                
bufferedWirter使用方法：                                                                           

BufferedReader:有一个很好用的方法：.readLile。能直接读出文本的某一行。
转换流： 其中有个：

latin-1到-9，西方语言（IS08859_1到_9）。
打印流（输出流）print：


Data

提供可以将int  double类型的数据直接写进去。

字节数组
输出流：
将默认达到控制台的东西换到ps里面；
eg：


translent:透明的。
serializable：标记性接口，标记后可以被序列化
externalizable:自己控制序列（不建议）。
应用：

结果将i,j,d,k挨个读出来

（转换流）只能把字节转化为字符。
从键盘输入：

线程：


启动线程，必须调用Thread类里面的start方法。
启动线程的两种方法：建议用接口。



sleep方法：


结束线程：：：，可以用使上述（flag=false）来执行。也可以用thread.ShutDown();
join：与当前线程合并

yield：让出cpu。

sleep：
线程优先级：


线程同步：
 
锁住当前对象。互斥锁。只有当这个进程执行完了，别的才能访问此对象。

程序死锁。

上述wait是Object类里面的方法，只有当线程锁锁住的时候，wait才生效，目的是停止该线程。
生产者与消费者问题：

网络编程：

用法：

while（ture）使客户端一直等待链接，可以连接多个。


Server端：

Socket s=new Socket();
OutputStream=s.getOutputStream();     Socket类中，有套用输入输出流的方法：get方法
Client端：

栗子：
Server端：其中是。getInetAddress()和s1.getPort()分别是客户端的ip和端口。

UDP:
首先声明一个”包袱“，来接收数据。容量自己定，

用udp协议配合输入输出流来输出：

GUI:
pace:可以调整窗口大小。setVisible:窗口可见。repaint:重新画paint.


component图形元素：容器 container
window：独立显出来 panel：不能作为应用程序的独立窗口显现出来。

applet：

window：frame ：
 
dialog（不点掉程序不能继续执行）：


三元色：最大255.
布局管理器：

1，


BorderLayout:

GridLayout:






 






内部类：

graphics类：




关于mouseAdapter:是实现MouseListener接口的类；


 






















































