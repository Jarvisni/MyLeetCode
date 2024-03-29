C++面向对象  
对象是对现实世界实体的抽象，由描述实体静态特征的数据以及描述实体行为特征的操作，封装在一起构成统一体。  
类是一种抽象数据结构，是对一类具有共同属性的对象的描述，其中封装了描述对象静态特征的数据和描述对象行为特征的函数。  
类是创建对象的模板，对象是类的具体实例。  
面向对象四个基本特征：抽象、**封装、继承、多态**  
抽象是面向对象开发时首先要分析问题空间实体，根据要解决的问题提取出本质属性并加以描述形成程序中的类，这就是抽象的过程。  
封装有两重含义：1、把数据和操作数据的方法进行封装形成一个实体----对象；2、类可以为其成员设定访问权限，实现数据隐藏，在类外部不能访问类的私有或保护成员，类提供公有接口和外部通信。  
继承指可以从已有的类派生出新的类，已有的类叫基类或父类，新类叫派生类或子类，通过继承，子类获得父类全部属性，同时子类又具有自己特有的新的属性，是对父类的扩展。继承可以最大限度利用已有的程序代码实现代码重用。  
多态指向不同的对象调用相同的成员函数，不同对象会给出不同的响应。通过子类覆盖父类的虚成员函数实现。  
  
  
1、public protected private  
public是类的公有成员，可以在类外对其进行访问。在程序所有函数中可以访问类的公有成员，在类的成员函数中可以直接使用成员名访问类的公有成员，在外部函数中可以使用对象名加点访问类的公有成员。  
protected是类的保护成员，不可以在类外进行访问，能在类的成员函数中访问保护成员，不能再外部函数中直接访问类的保护成员。但是可以在该类的子类的成员函数中访问基类的保护成员。  
private是类的私有成员，不可以在类外进行访问，只能在类的成员函数中访问私有成员，不能再外部函数中直接访问类的私有成员。例如radius私有，则定义公有的成员函数getradius来读取其值，setradius来给他赋值。  
  
  
  
2、创建和使用对象：  
Circle c1,c2;定义了两个对象。**定义的局部对象存储在栈区，如果定义的是一个全局对象则存储在静态存储区** 定义对象后可以通过对象名加点操作访问对象的public成员。c1.radius=10;float areac1=c1.area();  
  
3、内联函数  
incline，把一些代码短调用频繁的函数声明为内联函数，此时不进行常规函数调用而是把内联函数函数体直接嵌入到被调用的地方，既可以代码重用也可以减少调用时间开销。  
使用incline在类外进行定义为显式声明：incline float Circle::area（）{...}  
将函数体放在类体之中不需要incline则是隐式声明，也被认为是内联函数： class Circle{public: float area(){...} float getradius(); };  
  
4、构造函数  
类的构造函数是一种特殊的成员函数，在创建类的对象时，构造函数被编译器自动调用用来初始化对象的数据成员。  
没有返回值，函数名和类名相同，不使用限定符，可以被重载。  
class Circle{public:Circle(float r); float area(); private:float radius;}; Circle::Circle(float r){...} main()中Circle c1(10.5);  
类的不带任何参数的构造函数为默认的构造函数。如果定义一个类时没有声明任何一个构造函数，则编译器自动为该类生成一个默认的构造函数，默认构造函数什么都不做。给类声明一个构造函数后，编译器就不会自动生成默认的构造函数。可以自行声明和定义默认的构造函数使它完成功能。  
public：Circle(); Circle(float r); 类外类的实现: Circle::Circle(){...} Circle::Circle(float r){...} 在main()中 Circle c1;调用默认的构造函数。Circle c2(10.5);使用带参数的构造函数。  
  
5、拷贝构造函数  
使用类的一个已经存在的对象去初始化一个正在创建的对象，Circle c2(c1);或者Circle c2=c1;  
拷贝构造函数的函数名和类名相同，形参为一个同类对象的引用。Circle::Circle(Circle &c){radius=c.radius}  
三种情况编译器自动调用拷贝构造函数：1、用一个已存在的对象初始化被定义的新对象。2、函数的参数为类的对象，参数的传递方式为传值传递，发生函数调用时相当于用实参对象初始化正在创建的形参对象。
3、一个函数返回值为类的对象，函数执行结束时返回主调函数时，返回的对象是一个被调函数的局部变量随着执行结束也会被自动销毁，所以编译器在主调函数中创建一个临时对象存放函数返回对象的值，相当于用被调函数中的返回对象初始化正在创建的临时变量。  
Circle c3=c1;调用一次  
Circle c4=maxCircle(c1,c2);  
Circle maxCircle(Circle cir1,Circle cir2){if(cir1.area()>=cir2.area() return cir1;else return cir2;}  
调用函数时用两个实参对象初始化两个形参对象，调用两次，函数返回时用返回值对象初始化main中的临时对象，调用一次。  
