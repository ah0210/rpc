

远程方法调用
采用客户机/服务器模式实现两个进程之间相互通信,RPC是在Socket的基础上实现的，它比socket需要更多的网络和系统资源。除了Socket，RPC还有其他的通信方法，比如：http、操作系统自带的管道等技术来实现对于远程程序的调用。

php 获得 java 结果 
PHP自定义协议请求JAVA的服务，JAVA解析该协议，在本地实例化类并实现方法，然后把结果返回给PHP

---
解耦，获得socket结果

```
while (true)
 {
// disconnected every 5 seconds...
receive_message('127.0.0.1','85',5);
 }

 function receive_message($ipServer,$portNumber,$nbSecondsIdle)
 {
   // creating the socket...
   $socket = stream_socket_server('tcp://'.$ipServer.':'.$portNumber, $errno, $errstr);
   if (!$socket)
   {
     echo "$errstr ($errno)<br />\n";
   }
   else
    {
     // while there is connection, i'll receive it... if I didn't receive a message within $nbSecondsIdle seconds, the following function will stop.
     while ($conn = @stream_socket_accept($socket,$nbSecondsIdle))
     {
      $message= fread($conn, 1024);
      echo 'I have received that : '.$message;
      fputs ($conn, "OK\n");
      fclose ($conn);
     }
     fclose($socket);
   }
 }
?>

 The following code is for the "client". It send a message, and read the respons...

<?php

 send_message('127.0.0.1','85','Message to send...');

 function send_message($ipServer,$portServer,$message)
 {
   $fp = stream_socket_client("tcp://$ipServer:$portServer", $errno, $errstr);
   if (!$fp)
   {
      echo "ERREUR : $errno - $errstr<br />\n";
   }
   else
   {
      fwrite($fp,"$message\n"); //把内容写入客户端
      $response =  fread($fp, 4); //返回处理的结果
      if ($response != "OK\n")
         {echo 'The command couldn\'t be executed...\ncause :'.$response;}
      else
         {echo 'Execution successfull...';}
      fclose($fp);
   }
 }
```
