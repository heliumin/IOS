
1 NSObject对象占用多少内存?
oc代码底层实现其实是c/c++，顺序如下：
oc -> c/c++ ->汇编语言 -> 机器语言
oc对象是基于c/c++的结构体实现的

将OC转换成C/C++命令:
clang -rewrite-objc main.m -o main.cpp

将OC转换成iphone上的C++代码:
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main_1-arm64.cpp
