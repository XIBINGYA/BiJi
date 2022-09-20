# TCP协议

* TCP是一种面向连接，安全，可靠的传输数据的协议
* 传输前，采用“***三次握手***”方式，点对点通信，是可靠的
* 在连接中可进行大数据量的传输

![image-20220609083750550](.\TCP通信.assets\image-20220609083750550.png)

***注意：在java中只要使用java.net.Socket类实现通信，底层即是使用了TCP协议***



## Socket

| 构造器                               | 说明                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| public Socket(String host, int port) | 创建发送端的Socket对象与服务端连接，参数为服务端程序的ip和端口 |

## Socket类成员方法

| 方法                           | 说明               |
| ------------------------------ | ------------------ |
| OutputStream getOutputStream() | 获得字节输出流对象 |
| InputStream getInputStream()   | 获得字节输入流对象 |

```java

/**
 * 目标：完成Socket网路偏程入门案例的客户端开发，实现1发1收，
 */
public class Client {
    public static void main(String[] args) {

        // 1.创建Socket通信管道请求有服务器的连接
        // public Socket(String host, int port)
        // 参数一：服务端的IP地址
        // 参数二：服务端的端口
        try {
            Socket socket = new Socket(InetAddress.getLocalHost(), 8888);


            // 2.从socket通信管道中得到一个字节输出流，负责发送数据
            OutputStream os = socket.getOutputStream();

            // 3.把低级的字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            // 4.发送信息
            ps.println("你好，TCP服务端");
            ps.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }
        // 关闭资源
        // socket.close();

    }
}

```



***客服端发送信息***

![ ](.\TCP通信.assets\image-20220609084729617.png)

## ServerSocket(服务端)

| 构造器                        | 说明           |
| ----------------------------- | -------------- |
| public ServerSocket(int port) | 注册服务端端口 |



ServerSocket类成员方法

| 方法                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| public Socket accept() | 等待接收客户端的Socket通信连接，连接成功后返回Socket对象与客户端建立端到端通信 |

```java
public class server {

    public static void main(String[] args) {

        try {
            // 1.注册端口
            ServerSocket serverSocket = new ServerSocket(8888);
            // 2.必须调用accept方法：等待接收客户端的Socket连接请求，建立Socket.通信管道
            Socket socket = serverSocket.accept();
            // 3.从socket.通信管道中得到·个字节输入流
            InputStream is = socket.getInputStream();
            // 4.把字节输入流包装成履冲字符输入流进行消息的接收
            BufferedReader br = new BufferedReader(new InputStreamReader(is));

            // 5.按照行读取消息
            String msg;
            if ((msg = br.readLine()) != null) {
                System.out.println(socket.getRemoteSocketAddress());
                System.out.println(msg);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}

```



### 使用TCP通信实现，多发多收消息

![ ](.\TCP通信.assets\image-20220609091426583.png)

客户端

```java
public class client {
    public static void main(String[] args) {

        try {
            Socket socket = new Socket(InetAddress.getLocalHost(),6666);
            OutputStream os = socket.getOutputStream();

            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);

            String data = null;
            while(true){
                data = sc.nextLine();


                ps.println(data);
                ps.flush();

                if(data.equals("exit")){
                    System.out.println(socket.getRemoteSocketAddress()+"离线成功");
                    socket.close();
                    break;
                }
            }


        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}

```

服务端

```java
public class server {
    public static void main(String[] args) {

        try {
            ServerSocket serverSocket = new ServerSocket(6666);

            Socket socket = serverSocket.accept();

            InputStream is = socket.getInputStream();

            BufferedReader br = new BufferedReader(new InputStreamReader(is));

            String msg;
            while((msg = br.readLine()) != null){

                if(msg.equals("exit")){
                    System.out.println("离线成功");
                    socket.close();
                    serverSocket.close();
                    break;
                }

                System.out.println(socket.getRemoteSocketAddress()+":"+msg);
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

## 同时处理多个客户端消息

![image-20220609093028407](.\TCP通信.assets\image-20220609093028407.png)



客户端

```java
/**
 * 客户端
 */
public class client {
    public static void main(String[] args) {

        try {
            Socket socket = new Socket(InetAddress.getLocalHost(),6666);
            OutputStream os = socket.getOutputStream();

            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);

            String data = null;
            while(true){
                data = sc.nextLine();


                ps.println(data);
                ps.flush();
            }


        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}

```

服务端

```java
/**
 * 服务端
 */
public class server {
    public static void main(String[] args) {

        try {
            ServerSocket serverSocket = new ServerSocket(6666);

            while(true){
                Socket socket = serverSocket.accept();
                System.out.println(socket+"上线了!!!");

                new ServerReaderThread(socket).start();

            }


        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

线程类

```java
/**
 * 线程处理类
 */
public class ServerReaderThread extends Thread {
    private Socket socket;

    public ServerReaderThread(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {

            InputStream is = socket.getInputStream();

            BufferedReader br = new BufferedReader(new InputStreamReader(is));

            String msg;
            while ((msg = br.readLine()) != null) {
                System.out.println(socket.getRemoteSocketAddress() + ":" + msg);
            }
        } catch (Exception e) {
            System.out.println(socket.getRemoteSocketAddress()+"下线了！！！");
        }

    }
}

```

![image-20220609095024687](.\TCP通信.assets\image-20220609095024687.png)