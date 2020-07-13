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



sizeof是一个运算符，在编译时就已经确定了变量的大小。



2 OC对象的分类

主要分三类：

instance对象（实例对象）

通过类alloc出来的对象，每次调用alloc都会产生新的instance对象 

NSObject *object2 = [[NSObject alloc] init];

instance对象在内存中存储的信息包括：

isa指针，其他成员变量



class对象（类对象）

​    // class方法返回的一直是class对象，类对象

​    Class objectClass1 = [object1 class];

​    Class objectClass2 = [object2 class];

​    Class objectClass3 = object_getClass(object1);

​    Class objectClass4 = object_getClass(object2);

​    Class objectClass5 = [NSObject class];

类对象在内存中只有一份，指向同一个地址，因此以上5个类对象的地址是相同的。

class对象在内存中存储的信息主要包括：

isa指针

superclass指针

类的属性信息（@property），类的对象方法信息（instance method）

类的协议信息（protocol），类的成员变量信息（ivar）



meta-class对象（元类对象）：描述类的对象

​    // meta-class对象，元类对象

​    // 将类对象当做参数传入，获得元类对象

Class objectMetaClass = object_getClass(objectClass5)

每个类中内存中只有一个元类对象

元类对象和类对象的内存结构是一样的，但是用途不一样，在内存中存储的信息主要包括：

isa指针

superclass指针

类的类方法信息（class_method）



 1.Class objc_getClass(const char *aClassName)

 1> 传入字符串类名

 2> 返回对应的类对象

 

 2.Class object_getClass(id obj)

 1> 传入的obj可能是instance对象、class对象、meta-class对象

 2> 返回值

 a) 如果是instance对象，返回class对象

 b) 如果是class对象，返回meta-class对象

 c) 如果是meta-class对象，返回NSObject（基类）的meta-class对象

 

 3.- (Class)class、+ (Class)class

 1> 返回的就是类对象

 \- (Class) {

   return self->isa;

 }

 \+ (Class) {

   return self;

 }































