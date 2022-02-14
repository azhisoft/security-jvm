# security-jvm

这是一个对 java 字节码进行加密的工具集，采用了 2048 位 RSA 算法，可以有效的进行 java 代码保护，也可以用于代码授权；
工具集同时支持 Windows 和 Linux 平台（也可以支持 MacOS，但尚未测试）。

工具集的使用：
- ### 先准备 2048 位 RSA 公钥和密钥，可以利用 openssl 生成：
```c
openssl genrsa -out rsa_pri.pem 2048
openssl rsa -in rsa_pri.pem -pubout -out rsa_pub.pem
```

- ### 利用 enclass 对代码进行加密：
```c
./enclass -jar test.jar -key rsa_pri.pem
```

- ### 修改 java 命令行，加载解密模块对代码进行实时解密：
```c
// 在 Linux 环境下，需要把 libdeclass.so 复制到 /usr/lib 下，或者自行设置 LD_LIBRARY_PATH；
java -agentlib:declass -jar test-encrypted.jar -Ddeclass.key=rsa_pub.pem
```

- ### declass 更多用法：

- ### jartool 的用法：
