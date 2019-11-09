[TOC]
# Java环境配置及在Dos命令下运行Java程序
## 1. JDK下载
下载地址：https://www.oracle.com/technetwork/java/javase/downloads/index.html

点击DOWNLOAD

![JDk安装](https://github.com/LC-SZJ/picture/blob/master/JDK%E5%AE%89%E8%A3%851.PNG?raw=true)

首先点击安装许可协议

![JDK安装](https://github.com/LC-SZJ/picture/blob/master/JDk%E5%AE%89%E8%A3%852.PNG?raw=true)

然后点击相应的版本下载，如果是windows点击如下：

![JDK下载](https://github.com/LC-SZJ/picture/blob/master/JDK%E5%AE%89%E8%A3%853.PNG?raw=true)
安装成功以后
打开命令提示符，输入：java，出现如下提示表示安装成功！
![JDK安装成功](https://github.com/LC-SZJ/picture/blob/master/JDk%E5%AE%89%E8%A3%85%E6%88%90%E5%8A%9F.PNG?raw=true)

## 2. Java环境配置
找到我的电脑—>(鼠标右击)属性—>高级系统设置—>高级—>环境变量—>系统变量。
在系统变量找到Path，点击编辑
![Java环境配置](https://github.com/LC-SZJ/picture/blob/master/Java%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE1.PNG?raw=true)
点击新建，输入你安装JDK的目录下bin的地址
（这里安装的是jdk-12.0.2版本）
![Java环境配置](https://github.com/LC-SZJ/picture/blob/master/Java%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE2.PNG?raw=true)

## 3. 在Dos命令下运行Java程序
1. 编写好一个Java程序，输出九九乘法表
```Java
//package package1;
//！！！这里一定要注意：将包注释掉：
/*否则在Dos命令下会报错：
错误：找不到或无法加载主类 原因: java.lang.ClassNotFoundException 的方法
*/
public class jiujiu {
    public static void main(String[] args) {
       System.out.println("九九乘法表");
        System.out.print("\t");
        for(int i = 1; i <= 9; i++) {
            System.out.print(i + "\t");
        }
        System.out.println();
        for(int j = 1; j <= 9; j++){
            System.out.print(j+"\t");
            for(int i = 1; i <= 9; i++){
                if(j >= i)
                   System.out.print(i*j+"\t");
            }
            System.out.println();

        }
    }
}
```
2. 在Dos下进入jiujiu.java所在的目录
使用Dos命令进入到指定的文件下：`cd filename`
如果是在D盘，需要先切换，在Dos中输入：`d:`
即可切换到D盘
进入到jiujiu.java所在的目录后，在Dos下输入以下命令，先编译一下，生成`.class`文件。
`javac jiujiu.java`
如果出现以下错误：
`jiujiu.java:5: 错误: 编码 GBK 的不可映射字符 (0xA8)
       System.out.println("涔濅節涔樻硶琛?");
                                  ^
1 个错误`
输入`javac -encoding UTF-8 jiujiu.java`命令，指定编码方式为utf-8
3. 最后再输入`java jiujiu`即可，不需要带后缀名，效果如下：
![九九乘法表](https://github.com/LC-SZJ/picture/blob/master/%E4%B9%9D%E4%B9%9D%E4%B9%98%E6%B3%95%E8%A1%A8%E6%95%88%E6%9E%9C%E5%9B%BE.PNG?raw=true)


