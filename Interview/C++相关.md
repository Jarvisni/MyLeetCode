1、new 和 malloc 区别联系  
malloc与free是c++/c语言的标准函数，new/delete是C++的运算符。  
都可用于申请动态内存和释放内存。new/delete比malloc/free更加智能，其实底层也是执行的malloc/free。更加的智能体现在new和delete在对象创建的时候自动执行构造函数，对象消亡之前会自动执行析构函数。不把malloc/free淘汰出局呢？因为c++程序经常要调用c函数，而c程序只能用malloc/free管理动态内存。  
new返回指定类型的指针，并且可以自动计算出所需要的大小。malloc必须用户指定大小，并且默然返回类型为void*,必须强行转换为实际类型的指针。  
malloc实质：  
一个将可用的内存块连接为一个长长的列表的所谓空闲链表。调用malloc函数时，它沿连接表寻找一个大到足以满足用户请求所需要的内存块。然后，将该内存块一分为二（一块的大小与用户请求的大小相等，另一块的大小就是剩下的字节）。接下来，将分配给用户的那块内存传给用户，并将剩下的那块（如果有的话）返回到连接表上。调用free函数时，它将用户释放的内存块连接到空闲链上。  
从堆里面获得空间。也就是说函数返回的指针是指向堆里面的一块内存。  

2、程序中 堆、栈 区别  
堆(heap)需要程序员自己申请，并指明大小，向高位扩展，malloc函数p = (char * )malloc(5);  
C++ new运算符q = new char[5]; // (char * )malloc(5);  
但是p、q本身是在栈中的。  
操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序，堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。  
栈(stack)由系统自动分配,向低位扩展。 例如，声明在函数中一个局部变量int b; 系统自动在栈中为b开辟空间。只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。  
如果堆中的内容非常多超出预期大小，把栈的数据填充，会出现缓冲区溢出， 使用精心准备好的汇编代码（payload）填充栈区就可以获取系统或者管理员权限。  
什么是堆：堆是大家共有的空间，分全局堆和局部堆。全局堆就是所有没有分配的空间，局部堆就是用户分配的空间。堆在操作系统对进程 初始化的时候分配，运行过程中也可以向系统要额外的堆，但是记得用完了要还给操作系统，要不然就是内存泄漏.  
什么是栈：栈是线程独有的，保存其运行状态和局部自动变量的。栈在线程开始的时候初始化，每个线程的栈互相独立。每个函数都有自己的栈，栈被用来在函数之间传递参数。操作系统在切换线程的时候会自动的切换栈，就是切换SS/ESP寄存器。栈空间不需要在高级语言里面显式的分配和释放。  
  
3、C++多态  
当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。  
调用函数 area() 被编译器设置为基类中的版本，这就是所谓的静态多态，或静态链接----函数调用在程序执行前就准备好了。有时候这也被称为早绑定，因为 area() 函数在程序编译期间就已经设置好了。对程序稍作修改，在Shape 类中area() 的声明前放置关键字 virtual，此时，编译器看的是指针的内容，而不是它的类型。因此，由于 tri 和 rec 类的对象的地址存储在 * shape 中，所以会调用各自的 area() 函数。  
每个子类都有一个函数 area() 的独立实现。这就是多态的一般使用方式。有了多态，您可以有多个不同的类，都带有同一个名称但具有不同实现的函数，函数的参数甚至可以是相同的。  
  
4、C++虚函数  
虚函数 是在基类中使用关键字 virtual 声明的函数。在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。我们想要的是在程序中任意点可以根据所调用的对象类型来选择调用的函数，这种操作被称为动态链接，或后期绑定。  
想要多态，在基类中又不想对虚函数给出有意义的实现，这个时候就会用到纯虚函数。我们可以把基类中的虚函数 area() 改写如下：virtual int area() = 0;告诉编译器，函数没有主体，上面的虚函数是纯虚函数。  
  
5、构造函数和析构函数  
类的构造函数是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于为某些成员变量设置初始值。
Line::Line(void)  
{  
 cout << "Object is being created" << endl;  
}  
主函数中Line line;时执行构造函数会输出上面字符串。  
Line::Line( double len)  
{  
 cout << "Object is being created, length = " << len << endl;  
 length = len;  
}  
主函数中Line line(10.0);时执行构造函数会输出上面字符串以及参数值。  
类的析构函数是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。  





部分内容来源网络：  
1、https://www.cnblogs.com/diligenceday/p/5764443.html  
2、  
