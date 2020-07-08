1 NSObject对象占用多少内存?

系统分配了16个字节给NSObject对象，通过objec的源码可得知，但NSObject对象内部只使用了8个字节的空间（64bit环境下，可以通过class_getInstanceSize函数获得）

oc代码底层实现其实是c/c++，顺序如下：
oc -> c/c++ ->汇编语言 -> 机器语言
oc对象是基于c/c++的结构体实现的

将OC转换成C/C++命令:
clang -rewrite-objc main.m -o main.cpp

将OC转换成iphone上的C++代码:
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main_1-arm64.cpp

在64位系统，指针的大小为8个字节

通过runtime获取：

\#import <objc/runtime.h>

\#import <malloc/malloc.h>

// 获得NSObject实例对象的成员变量所占用的大小 >> 8

 NSLog(@"%zd", class_getInstanceSize([NSObject class]));

  // 获得obj指针所指向内存的大小 >> 16

  NSLog(@"%zd", malloc_size((__bridge const void *)obj));



内存对齐：结构体的大小必须是最大成员大小的倍数，OC规定最小对象大小为16字节。



LLDB常用指令：

print，p：打印

po：打印对象



内存读取：

memory read/数量格式字节数 内存地址 

x/数量格式字节数 内存地址	

格式：x16进制	f浮点	d10进制

字节大小：b：byte 1字节	h：half word 2字节

w：word 4字节   g：giant word 8字节

例子: x/4xg	地址

打印4串以16进制格式8个字节的数据，以地址开始

修改内存中的值：

memory write 内存地址 数值







