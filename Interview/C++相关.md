1、new 和 malloc 区别联系  
见纸上，未更新，不全面  
malloc与free是c++/c语言的标准函数，new/delete是C++的运算符。  
都可用于申请动态内存和释放内存。new/delete比malloc/free更加智能，其实底层也是执行的malloc/free。更加的智能体现在new和delete在对象创建的时候自动执行构造函数，对象消亡之前会自动执行析构函数。不把malloc/free淘汰出局呢？因为c++程序经常要调用c函数，而c程序只能用malloc/free管理动态内存。  
new返回指定类型的指针，并且可以自动计算出所需要的大小。malloc必须用户指定大小，并且默然返回类型为void*,必须强行转换为实际类型的指针。  
malloc实质：  
一个将可用的内存块连接为一个长长的列表的所谓空闲链表。调用malloc函数时，它沿连接表寻找一个大到足以满足用户请求所需要的内存块。然后，将该内存块一分为二（一块的大小与用户请求的大小相等，另一块的大小就是剩下的字节）。接下来，将分配给用户的那块内存传给用户，并将剩下的那块（如果有的话）返回到连接表上。调用free函数时，它将用户释放的内存块连接到空闲链上。  
从堆里面获得空间。也就是说函数返回的指针是指向堆里面的一块内存。  

2、程序中 堆、栈 区别  
见纸上总结，此处尚未更新，不够全面  
堆(heap)需要程序员自己申请，并指明大小，向高位扩展，malloc函数p = (char * )malloc(5);  
C++ new运算符q = new char[5]; // (char * )malloc(5);  
但是p、q本身是在栈中的。  
操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序，堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。  
栈(stack)由系统自动分配,向低位扩展。 例如，声明在函数中一个局部变量int b; 系统自动在栈中为b开辟空间。只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。  
如果堆中的内容非常多超出预期大小，把栈的数据填充，会出现缓冲区溢出， 使用精心准备好的汇编代码（shellcode）填充栈区就可以获取系统或者管理员权限。  
什么是堆：堆是大家共有的空间，分全局堆和局部堆。全局堆就是所有没有分配的空间，局部堆就是用户分配的空间。堆在操作系统对进程 初始化的时候分配，运行过程中也可以向系统要额外的堆，但是记得用完了要还给操作系统，要不然就是内存泄漏.  
什么是栈：栈是线程独有的，保存其运行状态和局部自动变量的。栈在线程开始的时候初始化，每个线程的栈互相独立。每个函数都有自己的栈，栈被用来在函数之间传递参数。操作系统在切换线程的时候会自动的切换栈，就是切换SS/ESP寄存器。栈空间不需要在高级语言里面显式的分配和释放。  
  
3、C++多态  
见纸上，未更新不全面  
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
  
6、引用和指针的区别和联系： 
引用是C++引入的重要机制（C语言没有引用），它使原来在C中必须用指针来实现的功能有了另一种实现的选择.
可以把引用理解成变量的别名。定义一个引用的时候，程序把该引用和它的初始值绑定在一起，而不是拷贝它。计算机必须在声明r的同时就要对它初始化，并且，r一经声明，就不可以再和其它对象绑定在一起了。引用不能脱离被引用对象独立存在.实际上也可以把引用看做是通过一个常量指针来实现的，它只能绑定到初始化它的对象上。  
（1）引用和指针汇编代码除了pi与ri变量名不同，其余汇编代码完全一样。所以，引用变量在功能上等于一个指针常量，即一旦指向某一个单元就不能在指向别处。  
（2）在底层，引用变量由指针按照指针常量的方式实现。  
关于指针和引用的对比：引用只能在定义时被初始化一次，之后不可变，但是指针可变；引用没有 const，指针有 const；引用的一个优点是它一定不为空，因此相对于指针，它不用检查它所指对象是否为空，这增加了效率；引用使用时无需解引用(* )，指针需要解引用；sizeof 引用得到的是所指向的变量(对象)的大小，而sizeof 指针得到的是指针本身(所指向的变量或对象的地址)的大小；指针和引用的自增(++)运算意义不一样；引用自增被引用对象的值，指针自增内存地址。  
```C++
int a,b,*p,&r=a;//正确
r=3;//正确：等价于a=3
int &rr;//出错：引用必须初始化
p=&a;//正确：p中存储a的地址，即p指向a
*p=4;//正确：p中存的是a的地址，对a所对应的存储空间存入值4
p=&b//正确：p可以多次赋值，p存储b的地址
```
  
7、static静态：  
（1）函数体内静态变量具有记忆功能，一个声明为静态的变量只会被初始化一次。  
（2）在文件内函数体外，用于限制变量或者函数的作用域为当前文件，即静态变量只可以被当前文件内函数访问，不能被其他文件中函数访问。静态函数作用域仅限本文件，只能被当前文件中的其他函数调用，不能被其他文件中的函数调用。  
（3）在C++类内数据成员被定义为静态，则为类的静态数据成员。非静态数据成员则每一个对象有一个副本，静态数据成员只有唯一的副本，被所有对象共享。  
（4）静态成员变量属于类而不属于对象，即使没有实例化的对象也可以使用静态变量，类名：静态成员变量  
（5）静态成员变量仍然遵守public\protected\private访问规则，static成员变量的初始化必须在类外（由于类的声明可能被多处引用，如果初始化放在类内则每次引用都会初始化一次且分配一次空间，和static要求不符），初始化不用static关键字，protected和private静态成员变量虽然可以类外初始化，但是不可以类外访问。  
（6）静态函数是类的静态成员函数，属于类而不属于某一个特定对象，被所有对象共享。this指向类的对象本身，可以this->调用普通成员函数，由于静态成员函数属于类，不属于对象。没有this指针。类的静态成员函数无法访问对象成员、普通成员函数。只能访问静态成员函数、静态成员。  
  
static变量相比全局变量优势：  
全局变量作用范围整个工程，static变量仅所在文件，减少了命名冲突可能性，可以不同文件中命名相同的static变量。  
可以实现信息隐藏，静态数据成员可以是private，而全局变量不可以。  
  
  
8、(const int* p   =  int const * p )   vs    int * const p  
const int * p ： const右边接近于int这个类型声明，意思是有个指针p，指向的是一个int型的整数常量。即p可变，* p不可变。  
int* const  p ： const接近于* 这个类型声明，意思是有个指向整数的常指针。即p不可变，* p可变。  
  
部分内容来源网络：  
1、https://www.cnblogs.com/diligenceday/p/5764443.html  
2、https://www.zhihu.com/question/37608201/answer/72766337    
3、《程序员面试笔试宝典》  何昊 等   
4、  
