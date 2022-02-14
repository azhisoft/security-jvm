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

- ### enclass 更多用法：
```c
Usage: EnClass <-command [options]>
    -jar jarfile  加密指定的 .jar 文件
    -cp filepath  加密指定的 .class 文件，或指定的目录
    -key keyfile  指定加密的 RSA 私钥文件
    -i pattern    指定要加密的文件（支持通配符）；不指定时，加密全部文件；指定时，仅加密指定的文件
    -x pattern    指定要排除的文件（支持通配符）
    
示例：
    ./enclass -cp test-v1.0 -key rsa_pri.pem -x *dto*
    ./enclass -jar test.jar -key rsa_pri.pem -i *algorithm*
```

- ### jartool 的用法：
```c
Usage: Jartool <-command [options]>
    -pack jarfile     打包一个 .jar 文件；需要 -dir 搭配使用，即把 -dir 指定的目录打包成 .jar
    -unpack jarfile   解包一个 .jar 文件；通常需要 -dir 搭配使用，即把 .jar 解包到 -dir 指定的目录
    -manifest file    生成一个 MANIFEST.MF 文件，可以搭配 -main 和 -lib 使用
    -update jarfile   更新 .jar 中的某个文件，需要搭配 -file 使用
    -dir path         配合 -pack 和 -unpack 使用，用于指定输入/输出目录
    -main main-class  配合 -manifest 使用，用于指定运行类名，即 Runnable jar 的启动类
    -lib lib-path     配合 -manifest 使用，用于指定依赖库路径
    -file name,file   配合 -update 使用，name 用于指定 .jar 中的路径，file 用于指定本地文件，用逗号分隔
```

## 反馈及意见
当你遇到任何问题时，可以通过在 GitHub 的 repo 提交 issues 来反馈问题，请尽可能的描述清楚遇到的问题，如果有错误信息也一同附带，并且在 Labels 中指明类型为 bug 或者其他。
[通过这里查看已有的 issues 和提交 Bug](https://github.com/azhisoft/security-jvm/issues)
