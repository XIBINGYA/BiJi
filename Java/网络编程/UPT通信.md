# UDP协议的特点 

* UDP是一种无连接，不可靠传输的协议
* 将数据源IP，目的地IP和端口以及数据封装成数据包，大小限制在64KB内，直接发送出去即可

![image-20220608160400684](.\UPT通信.assets\image-20220608160400684.png)



## DatagramPacket: 数据包对象（韭菜盘子)

![image-20220608160822205](.\UPT通信.assets\image-20220608160822205.png)

## DatagramSocket：发送端和接收端对象（人）

![image-20220608160945161](.\UPT通信.assets\image-20220608160945161.png)



客户端实现步骤:

![image-20220608163722362](.\UPT通信.assets\image-20220608163722362.png)

```java
        // 1.创建发送端数据，发送端自带默认的端口号(人)
        DatagramSocket socket = new DatagramSocket();

        //2、创建一个数据包对象封装数据（非菜盘子）
        /**
        public DatagramPacket(byte buf[],int length,
        InetAddress address,int port)
        参数一：封装要发送的数据（韭菜）
        参数二：发送数据的大小
        参数三：服务端的主机IP地址
        参数四：服务端的端口
        */
        byte[] buffer = "我是开乐韭菜".getBytes();
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length, InetAddress.getLocalHost(), 8888);

        // 发送数据出去
        socket.send(packet);

        // 释放资源
        socket.close();
```

接收端实现步骤

![image-20220608163833905](.\UPT通信.assets\image-20220608163833905.png)

```java
        // 1.创建接收端对象，注册端口（人）
        DatagramSocket socket = new DatagramSocket(8888);

        // 2.创建一个数据包对象接收数据 （韭菜盒子）
        byte[] buffer = new byte[1024 * 64];
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

        // 3.等待接收数据
        socket.receive(packet);

        // 4.取出数据
        int len = packet.getLength();
        String rs = new String(buffer, 0, len);
        System.out.println("收到："+rs);

        socket.close();
```

![image-20220608163942864](.\UPT通信.assets\image-20220608163942864.png)

### 拓展:获取发送端IP端口

* packet.getScoketAddress().toString();



## 使用UDP通信实现：多发多收信息

![image-20220608164259485](.\UPT通信.assets\image-20220608164259485.png)

![image-20220608164442333](.\UPT通信.assets\image-20220608164442333.png)

```

```

![image-20220608164512115](.\UPT通信.assets\image-20220608164512115.png)

![image-20220609075916534](.\UPT通信.assets\image-20220609075916534.png)