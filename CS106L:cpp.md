# C++ Learning

*Based on CS106L spring 2023*

***

## Lec1.Basic Intro

**重点：** ***what's cpp?***

**Ans:** ***基本语法 + STL***

+ 基本语法：语句末的分号，原始的类型(int,double)，基本语法规则  
+ the STL：大量的功能， 内置的类(maps,vectors)，通过***std:: ***命名空间实现

### Namespaces

+ 命名空间，许多功能都实现在**std**::中，靠它来引入
+ 通常会使用**using namespace std**在开头，这样可以自动为*cin*,*cout*等增加std前缀来使用，但cs106l并**不推荐**这种写法  

### STL

+ 全称**标准模板库(Standard Template Library)**，包含大量功能(算法，容器，函数，迭代器)
+ STL的命名空间为*std*，是standard的缩写
+ 要从STL访问元素（其实跟调用函数差不多）要用***std***::

***

## Lec2.Types and Structs

### Fundamental Types 基本类型

+ int, char, float, double, bool
+ std::string(#include <string\>)

### C++是一种静态语言(*statically typed*)

+ 所有有名字的东西（函数，变量等）在运行前都会被赋予一个类型
+ 源码编译发生在程序运行前
+ 与之相反，动态语言(***dynamically typed***)如python会在运行时根据值来赋予其类型，逐行运行与编译，因此可能在运行中会遇到不可预知的错误
+ 所以静态类型能帮助**在程序运行前预防错误**

### 函数重载

两个函数可以**同名**，但变量类型不能相等，这样可以减轻命名与调用负担。  

### struct：一种将不同的types捆绑在一起的方法

+ 可以用struct传递一组数据，同样的，也可以使返回一组数据  
+ **初始化方法**：  
    1.定义后创建，创建后逐项初始化  
    2.定义后创建，直接{}大括号括起来，像数组那样初始化  
+ ***std::pair***：STL提供的模板，用于结合两个类型，前者通过.*first*引用，后者通过.*second*引用   


    std::pair <type1, type2\> pairName = \{data1, data2\};  
----------引用STL库---------- | ----------------初始化---------------\|  
    当返回**pair**时，若不想显式的定义出/懒得打字，可以使用`std::make_pair(field1, field2);`来隐式的调用它。    

### auto：方便的自动推理type

+ 用来代替变量前的type，让编译器自己推导类型  
+ 不意味着变量**没有类型**，只是简化  
+ **适当**的使用能使代码变得**简洁**，**过度**的使用反而会**降低可读性甚至错误**。应该在前面或其他地方经过充分的定义才能使编译器推导出类型。  

***

## Lec3.Init & Reference 初始化与引用

### 初始化：为变量提供一个初始的值

+ 初始化可以有多种方式：  
    1.先创建变量，再赋予其值(*int a; a = 0;*)  
    2.创建变量的时候直接赋予其值(*int a= 0;*)  
    3.拥有特殊初始化手段，譬如STL或函数(*std::pair<int, string\> a = std::make_pair(1, "a")*)  
+ 最**统一**的初始化：**花括号{}**，无论什么类型都可以用花括号来进行初始化。  
  一般普通括号()因特性会与花括号{}拥有相同的初始化效果，**但是**在*std::vector*向量的初始化时，{}与()十分不同:  

    std::vector<int\> vec1(3, 5);  
  std::vector<int\> vec2{3, 5};  

    其中，**vec1 =\{5, 5, 5\}**，**vec2 = \{3, 5\}**  

### 进阶技巧：基于auto与pair实现结构化绑定(Structured binding)

+ 以*pair*为例，得到的pair在**初始化时**是有条理的：  

    auto p = std::make_pair("str", 1);  

    而在**赋予其他变量时**却只能一个一个的赋予，反而失去了条理：  

    string str = p.first;  
int num = p.second;  

    纵使pair本身有.*first*与.*second*的逻辑关系，但分两行写又使没有那么清晰，所以有这样的**结构化绑定**：  

    auto [str, num\] = p;  

    十分清晰了。  

+ 放在长串代码里更能体会，这里fun返回pair\<bool, pair\>：  

    auto result = fun(a, b, c);  
    bool ifFound = result.first;  
    if (ifFound)  
    {  
        auto solutions = result.second;  
        std::cout << solutions.first << solutions.second << endl;  
    }  
    else  
    {  
        std::cout << "Can't Found" << endl;  
    }  

    结构化绑定后：

    auto result = fun(a, b, c);  
    auto [ifFound, solutions\] = result;  
    if (ifFound)  
    {  
        auto [x1, x2\] = solutions;  
        std::cout << x1 << x2 << endl;  
    }  
    else  
    {  
        std::cout << "Not Found" << endl;  
    }  

    ***显然更为的整洁易懂。***  

+ 注意，结构化绑定时获得的是**复制**，而非**引用**，要对值进行修改时注意**引用**，见下方。

### 引用：已经拥有的变量的别名

+ 引用不同与值传递与地址传递：

    1.值传递：原来的a->复制一份成为b->对b进行操作i->最后a没有变，b变了。  
    2.地址传递：原来的a->得出a的地址a'->对a'进行解地址操作->最后改变的还是a。  
    3.引用：原来的a->进行一个引用出b->实际上a与b就是一个东西。  

+ 引用的例子：

    int a = 100;  
    int & b = a;//a成为b的引用  
    b = 0;  

    实际上a就是b，变为0了。  

+ 典型的忽略引用bug：  

    auto \[num1, num2\] = nums[i\];  
    num1 ++;  
    num2 ++;  

    在向量（高端数组）的一个循环中，利用上述的结构化绑定技巧，却没有注意到这样的赋值只是给复制进行了操作i，而并没有给要操作的本身进行操作。往往只在函数定义时关注***复制还是引用***的问题，在函数实现的代码中就没有这么做。修改：  

    auto& [num1, num2\] = nums[i\];  

    即可。  
    ***所以，结构化绑定时要注意引用！***

### 左值、右值
+ 左值可以在**等号=**的左边被赋值，也可以在等号右边赋予别的变量值。是一个有自己的**名字**的**非临时值**。譬如平时定义的变量。  
+ 右值是一个没有名字的临时量，只能在等号右边给别的值赋值。  
比如`int x = 1;`中x为左值，1为右值。  
+ **只有左值才能被引用**。  

### const 修饰的变量

+ const表示变量**不能够被修改**。  
+ 那么const与引用的关系？  
    **const ref(&)**一个**变量**，可以，并且*无法通过ref进行修改*，但是可以直接修改变量  
    **ref(&)**一个**const 变量**，***不可以***  
    **const ref(&)**一个**const 变量**，***可以***  
    **ref(&)**一个**const ref(&)**，***不可以***  

### 进行赋值(=)操作时

+ 默认对右值进行一个**复制(copy)**，将**copy**赋予左值  
+ 因此即便写出这样的等式，假定有一个c_ref，是const；和一个n_ref：  
`auto copy = ref;`  
这样得到的copy**并不是const**，**也不是ref**！  
```
const auto c_copy = ref;//**const copy**     
auto& ref = n_ref;//**ref**  
const auto& ref = n_ref;//**const ref**  
```
+ ***由此可知，在进行=操作时，无论右值拥有怎样的特性，赋值给左值后只是一个普通的值，要想让左值有特性(const,&)，必须要手动在式子前面添加。***  

### const 与 ref(&) 使用

+ 当使用一些小型变量(int, double)时，**copy**即可  
+ 当需要对变量换个名字进行修改，用**ref**  
+ 当是一个大变量时且不需要修改（比如huge vector或自定义的结构体）时，选择**const ref**  

### 返回值也可以是ref、const ref

+ 没看懂......  

***

## Lec4. Stream 输入、输出的抽象

### stream的分类

1.根据流的**方向**  

+ 输入流：用来**读取**数据，如*std::istream*, *std::cin*  
+ 输出流：用来**输出**数据，如*std::ostream*, *std::cout*  
+ 输入输出流： **读取**和**输出**均可，如*std::iostream*, *std::stringstream*  

2.根据流的**来源**和**去向**  

+ **控制台(console)流**：从控制台读取或输出到控制台。如*std::cout*, *std::cin*  
+ **文件(flie)流**：从文件读取或输出到文件。如*std::fstream*, *std::ifstream*, *std::ofstream*  
+ **字符串(string)流**：从字符串读取或输出到字符串。如*std::stringstream*, *std::istringstream*, *std::ostringstream*  

### "<<"与">>"

+ **\>\>** 是**流提取运算符**，从流中提取数据并将其放入变量，通常用于读取  
+ **<<** 是**流插入操作符**，将数据放入流中，通常用于输出  

### 输出流 output stream

#### output streams  

+ 类型：***std::ostream***  
+ 原理：通过<<运算符，将**数据**解析为**字符串**发送到**流**  
+ 例如，**std::cout**:  
    + 将所有**原始数据类型**与大部分**STL work**输出到**控制台**流  
    + 不同类型可以通过多个<<来输出  
    + 无法输出**自定义数据类型(struct)**，在后面会自己实现  
    + ***特点***：*std::cout*来自头文件<iostream\>中的一个**全局固定对象(global const object)**，所以可以直接使用而不用初始化出一个对象，因为它本身就是一个对象，而其他的输出流则不同，**必须要进行初始化**  

#### output file streams

+ 类型：***std::ofstream***  
+ 原理类似，不过是输出到**文件流**  
+ 使用前需要**初始化**到一个**对象**。如下，初始化了一个名为out的对象，指向的文件为out.txt，后续使用out：   

    std::ofstream out("out.txt");  
    out << "This str will send to file" << std::endl;  

### 输入流 input stream 的典型：std::cin

+ 类型：***std::istream***  
+ 原理：只能使用"**\>\>**"从**流**中接收**字符串**并将其转换成**数据**，这里*cin*是从控制台流  
+ 可以读取不同类型的数据，用多个">>"按顺序读取
+ 详细过程：调用 **std::cin \>\>** --\> **缓冲区(buffer)中是否有内容** --\> **若有，则按规则读取；若无，则在命令行创建一个新的命令提示符** --\> **用户输入，直到按下enter结束** --\> **读取，规则为：每个\>\>读取到witespace(space,tab,newline)为止** --\> **第一个whitespace后的所有内容会被保存到缓冲区，而whitespace会被吃掉**  
+ 读取原则：**如果**目标type为x，则读取时**一直**读到**非x或whitespace**为止，  
所以int读str会直接读出0，同时将这个stream的fail位标记，这个流不再接受输入。
+ ***初始化？*** 和*cout*相似，包含在头文件<iostream\>中，是一个**global const object**，所以可以直接用，因为它本身就是一个对象，不用再初始化；**但其他输入流就必须要初始化一个对象来，再使用这个对象而不是本身。**

### std::getline()：用于读取字符序列（句子）

+ 函数原型：***std::getline(istream& is, string& str, char delim);*** //delim default = \n  
+ 工作原理：  
    + 清除*str*中的内容，然后从*is*中提取字符放进*str*直到**结束条件**  
    （结束条件：到达文件结尾(**EOF**)、下一个字符是delim（会把delim提出来但不会保存）、str空间不足）  
+ ***getline***与>>对比：  
    + *\>\>*读到**whitespace**就会停止，因而**不能读取句子**；而*getline*可以**在指定处结束**，所以可以**读取整个句子**  
    + *\>\>*可以实现字符串到数据的转换(**string->primitive type**)；而*getline*只能读出字符串  
+ ***不要混合使用\>\>与getline！***  
因为\>\>读到whitespace停止，whitespace**会留在**缓冲区，而*getline*读到delim停止，delim**不会留在**缓冲区  

### input file stream

+ 类型：***std::ifstream***  
+ 原理：从文件流中提取字符串并转换为built-in-type  
+ 必须要进行**初始化**到一个**对象**。如下，初始化了一个名为in的对象，指向的文件为in.txt，后续使用in，会实现in.txt中的第一个单词写进了str：   

    std::ifstream in("in.txt");  
    std::string str;  
    in \>\> str;  

### stringstream 字符串流

+ 可以读写字符串对象的流，允许对字符串进行输入/输出操作  
+ 根据读写权限，可以根据要求，*std::stringstream*, *std::istringstream*, *std::ostringstream*  
+ 一个例子：  

    std::string input = "114514";  
std::stringstream stream(input);  
int num;  
stream \>\> num;  

### 总结
+ 只有*cin、cout*本身就是一个对象，不必初始化，其余的都要先初始化。  
+ 在操作时，io要以程序为中心，从流中input，output到流中。

***

## Lec5.Containers 容器：同类型的统一集合与交互

### 容器类型

*向量*、*栈*、*队列*、*集合*、*图*
*数列(向量的原始形式，严格固定大小)*、*双端队列*、*列表(双向链表)*、*无序集合*、*无序图*  

### container特点

+ **组织** 相同的数据可以被打包在一起  
+ **标准化** 共同的结构被一起预期与实现  
> 不会翻译：  
> common features are expected and implemented together !  

+ **抽象** 复杂的想法更容易被利用  
> Complex ideas made easier to utilize by clients  

+ 当一个结构体实现了基本的功能后，要进行额外的拓展或修改会比较麻烦，就需要进行容器使用高级的方式  

### 基本功能

+ 存储多个相同类型的对象  
+ 允许通过某种(或多种)方式访问集合  
+ 允许遍历整个集合  
+ 允许编辑与删除  

### 容器的安全与效率

#### C++的运行效率极高

程序往往会在速度、功耗和安全之间做选择，C++运行效率高，所以安全性有所欠缺  
**譬如：向量赋值与引用** 在进行时往往不会进行越界检查  
为何？

##### 补充：C++的设计哲学

+ 只提供必要的安全检查  
+ 程序员最清楚一切  

### 容器的分类

1. **序列型**容器  
    + 可以按顺序访问  
    + 存放固有顺序的元素  

2. **关联式**容器(如映射和集合)  
    + 不一定有顺序  
    + 更容易搜索  

### 序列型代表：向量vector

+ 向量是相同类型的元素的有序集合  
+ 可以**改变大小**  
+ 其在内部实现一个数组  
+ 关键的易混淆参数：*_size*:向量中元素的数量，*capacity*：向量为元素分配的空间  

#### 序列型容器的选择

尽管每一个容器都基本可以实现所有功能，但是在效率上不同的容器有着不同的功能。  
譬如：  

+ 插入或移动前方的元素：**vector** < **deque**与**list**;  
中间元素：**vector** < **deque** < **list**;  
后方元素：都很快;  
+ 索引访问：**list**做不到  
+ 内存开销：**deque**与**list**比较大  
+ 组合(分裂与合并)：**List**最快  
+ 稳定性(迭代器/并发)：**List**效果最好  

***总结***：  
+ 信息有强制的顺序性，就选择序列型容器  
+ *std::vector*可以处理大多数  
+ *std::deque*适合在前列数据进行特别快的插入  
+ *std::list*适合连接、协同多列表时  

### 关联式代表：图Map

+ Map是由pairs实现的(*std::pair<const key ,value\>*)  
+ const key必须是不可变的  
+ 在通过*key*索引时，会通过在*pair*集合的*first*对象层里寻找，找到后返回*second*对象(pair:first,second)  
+ **有序/无序**
    + 所有映射map与集合set都有有序与无序两个版本  
    + 有序map/set需要定义一个*比较运算符(comparison operator)*  
    无序map/set需要定义*哈希函数(hash function)*  
    简单的type原生就有，而其他type就要自己定义  
    + **无序通常比有序的快**

***总结***：  
+ set与map差别不大  
+ **无序容器**更快，但与嵌套容器/集合困难，对hash函数了解  
+ 若使用复杂数据类型/不熟悉哈希函数，则使用**有序容器**  

### 容器统一操作

一般的底层容器(underlying container)一般都至少有如下操作：  

+ empty  
+ size  
+ front  
+ back  
+ push_back  
+ pop_front  

### 容器适配器(container adaptor):容器的包装器

通过修改接口(*modify the interface*)来对容器进行排序，并修改与容器进行交互的权限与方式  
~~搞不懂了~~

***

## Lec6.Iterator and Pointer :迭代器与指针

### iterator 与 pointer

+ pointer可以指向代码中的任何一个地方  
+ iterator是指针的一种特殊类型，指向容器中的特定元素  

### iterator：指向容器元素的指针的抽象

**container**是相同对象的集合，那么该如何访问容器里的对象？答案：***迭代器***  
所有容器都实现了一个迭代器来实现。  

+ 迭代器允许以*编程方式*访问容器中*所有数据*  
+ 迭代器有*一定的顺序*，它明白下一个元素是什么  
+ 但**不一定**每次的迭代都相同  

把容器想象成装满文件的抽屉：  
1. 拉出抽屉，可以看到所有文件 ----> 一个迭代器一次可以遍历所有  
2. 你可以看到抽屉的"back"与"front" ----> 容器的属性*back*与*front*  
3. 手指保持着自己的位置，也可以移动 ----> ？  
4. 可以取出任何文件，并进行io ----> 通过迭代器访问  
5. 通过查看文件在抽屉里的位置，就可以比较任何两个文件的相对位置 ----> ?

#### 迭代器基本操作

所有容器都实现了各不相同的迭代器，但都有相同的一些操作：  

+ 初始化 `iter = s.begin();`  
+ 比较 `iter == s.end();`***注:***.beg()与.end()都返回迭代器  
+ 递增 `++iter;`  
+ 解引用 `*iter`  
+ 复制 `new_iter = iter;`  

    ***（为什么是`++iter`而不是`iter++`?***

    + 返回的时间不同，使用迭代器时已经拥有前一个值了，用++iter更高效一点直接返回下一个  
    + ***但是！！！上面的理论过时了***  
    + ***现在`iter++`已经能满足要求了，基本没有什么区别）**  


#### 迭代器分类（根据实现的功能）

***在下面的容器都包含上面的容器的功能并有所新的实现***  

+ **Input** & **output** 迭代器  
  是**最受限**的的迭代器类型，可以执行顺序单次输入或输出操作  
    + Input迭代器出现在"="**右边**，`auto elem = *it;`，从迭代器中取出，**输入到程序**，为其中变量赋值  
    + Output迭代器出现在"="**左边**，`*elum = value;`，将变量赋值给迭代器，**从程序中输出**
+ **Forward** 迭代器  
是**标准容器功能**的**最小级别**，包含Input与Output迭代器的功能，是**单向**的  
+ **Bidirectional** 迭代器，如***List***、***Map***、***Set***  
包含了前向迭代器的功能，同时可以**双向**，即`++iter`与`--iter`均可实现
+ **Random Access** 迭代器，最强大，如***vector***与***deque***  
允许**直接访问值**，而无需按顺序遍历，即`iter+=5`与向量中的`vec[7]orvec[15]or`  
但要注意不能越界  

#### 迭代器的一般使用模式

**`for (auto iter = set.begin(); iter != set.end(); ++iter){}`**  

+ 当只是需要目标元素的独立的副本时，用**`auto elem = *iter;`**即可解引用出值并赋给elem  
+ 当要获取元素的引用而非副本时，用**`const auto& ref = *iter;`**即可使用引用类型来接受引用结果  
+ 可以使用之前提到的**结构化绑定**技巧，下面以map为例：  
    std::map<int, int\> mymap{{1,6},{2,3},{0,8},{3,9}};  
    for(auto iter = mymap.begin(); iter != mymap.end(); iter ++)  
    {  
        const auto& [key,value\] = *iter;//结构化绑定  
    }  

### pointer 

C语言基础内容  

### exercise

+ **第一次用完整的cpp语法！**  
+ 编译cpp时用**`g++ -o oname filename`**的基本形式(编译c时用`gcc`)  
+ 使用了**结构化绑定**的技巧  

***

## Lec7.Classes 类

各类**容器**其实就是在**stl**中定义好的类。还可以自己实现**自定义的类**。

### 定义

类是程序员自己定义的自定义类型，是对象或数据类型的抽象。  

### vs结构体

**难道结构体struct不行吗？**事实上，可以分析结构体的**两大缺点**：

1. **默认状态**下，外部方法可以**自由访问**结构体的所有成员变量
2. 成员变量都需要**显式定义**，否则是垃圾值

所以结构体显然不能够胜任对“类”的要求：智能功能、封装屏蔽、定义良好的功能。  
**而“类”则有了改进。**其为用户提供一个公共接口（public），与私有实现（private）分隔开。  

### 特征

+ **.hpp**中写法：  
	+ 首先定义出一个类class。  
	+ 在**public**部分，定义一些与私有变量交互的接口。  
	+ 在**private**部分，将所有成员变量定义出来，除非公共部分有相关接口来间接接触，不允许直接修改私有部分。
+ **.cpp**中：  
	+ 根据hpp中的定义完善函数实现，要根据namespace来定义好。  

**this关键字**：**命名冲突**时的解决方法。当公共方法中出现了与私有成员重名的情况时，可以通过**this->**来引出私有变量来解决冲突。  
"this"其实是一个指针，指向当前对象，所以`this->`相当于`(*this).`。  
类内的每一个成员函数都有一个隐藏的“this”，是一个指向当前对象本身的指针。对于const成员函数（在签名中有const），其this指向的是一个**const \*classname**指针，而对于非const成员函数，指向的是**\*classname**指针。  

**构造函数**：初始化成员方法  
如`classname::funname():vri1(), vri2(){}`  

**内存管理：**  
+ 形式上与c的malloc类似，但是更加简洁，比如要有一个十个元素的整数数组：  
```
int *array;  
array = new int[10];
```
这样就可以初始化。  
+ 要删除时，通过`delete [] 指针`来删除内存
+ + **destructor**可以用来调用`delete []`，通常做类的一个方法，定义为`classname::~classname`，让封装更为整洁。  

### 对模板类的分析

**Software Engineering 软件工程基本原则之一**：只要添加足够多的中间层，任何问题都可以被解决。  

自定义的类有缺陷，譬如希望向量能够装进任何类别的数据类型，并且不需要自己去手动将所有类都实现，这就需要有智能化的**模板类Template Class**，能够根据输入类型自定义类内变量类型。  

**stl**中的vector、map、set等类型基本都是模板类，能适配各种类别，在定义时要给定内容，比如`std::vector<int> myvector;`；  
除了stl，自己也可以先实现一个模板，写法如下所例：  
```
template<typename First, typename Second> struct myStruct
{
	First first;
	Second second;
}
```
这样定义了一个模板结构体，同样的，改struct为class便成为模板类了：  
```
// in myClass.hpp
template<typename First, typename Second> class myClass
{
	public:
		First getfirst();
		Second getsecond();
		void setfirst(First f);
		void setsecond(Second s);
		
	private:
		First first;
		Second second;
}

// in myClass.cpp
#include "myClass.hpp"

template<typename First, typename Second>
First myClass<First, Second>::getfirst()
{
	return first;
}

template<typename First, typename Second>
Second myClass<First, Second>::getsecond()
{
	return second;
}
……
```

以上在myClass.cpp\/hpp中定义了一个模板类，等待接收两个类型进行实例化。  

#### 深入理解template

模板相较于普通代码，就在于其多了一个形如`template<typename T>`的开头，这个形式是告诉编译器先不要编译，等待传进来具体类型、进行实例化之后再编译。  
因此模板相当于是一个**占位符**，等待实例化后才进行编译，所以有一个很关键的地方：在模板实现上，**不应该**继续在*.cpp中include头文件.hpp*，这样会导致链接错误，因为编译时会跳过，而需要用的时候编译器发现还没有编译；**而应该**反其道而行之，在**.hpp中include.cpp文件**  

同样重要的，在.cpp中要实现模板，需要在每个函数前加上模板开头，因为这代表占位符，编译器并不知道某函数是不是带有模板类，所以必须加上。  

***

## Lec8.Template Class & Const Correctness

### Member Types 成员类

在完成模板类的定义后，有时还需要为模板类内的一种数据类型单独定义一个**名字**，譬如stl模板`vector<int>::iterator`就是用来指代vector内的指针类型（譬如.begin()）。  
这里的**iterator**就是**vector**的一个**成员类 member type**。  

#### 语法

以一个自定义的vector为例：  
```
// vector.hpp
template<typename T>
class vector{
	public:
	using iterator *T;
	
	iterator begin();
}
```
+ 首先定义了模板类vector
+ 然后在公共成员内声明了一个名为begin的**函数**，没有输入值，返回类型为iterator（实际上是\*T）。这里是因为用了**using**这一关键字，相当于用iterator来代替\*T，类似于C中的typedef。  

```
// vector.cpp
template <typename T>
typename vector<T>::iterator vector<T>::begin() {...}
```
+ 对应的在cpp内的实现，注意这里的写法，是先定义了函数名是vector\<T\>这一特定类内已经声明的begin，然后返回值是vector\<T\>类内的iterator成员类，并且加上typename来强调这是一个类。  
+ 有一种错误写法：`... iterator vector<T>::begin() {...}`，这一写法在返回类型上不对，因为编译器并不知道iterator是一个类，因为其的作用域在类内，在类外就不知道了。最后还要加上typename来告诉编译器这就是一个类。  

### Const Correctness 常值正确性

在我们定义的类的成员函数的输入值中，通常会对那些比较复杂的自定义类型（比如自己定义的class对象）用**const class& x**来引用，然后在函数实现中使用类的方法。而这里编译时就容易出现报错。  

原因是cpp编译器会默认我们**不会按照规矩行事**，也就是认为我们所用的类的方法会修改类的值。所以需要手动在**函数签名**后加上**const**来表示这是一个常量函数，即不会对类内变量进行更改，然后编译器会强制进行检查，确实没有改变后就解决了这个问题。  

**补充：函数签名**  
函数签名是没有返回值的函数声明，一般用于函数重载的判断，以及加上常值函数的标签  
函数定义是没有函数具体实现  
函数实现  

#### 何时加const签名

首先看成员函数实现中是否有对类内变量的修改；  
然后确定返回值是否返回的是类内对象的引用，如果虽然函数本身没有修改，但是返回的引用可以为我们修改，就不应该加const。  

在**vector**类内，有一个**at()**方法，类似于**操作符[]**，但是at方法有边界检查，更加安全。  
研读at方法代码，可以发现其在函数内部没有对类内变量进行修改，返回值是类内对应变量的引用，因此**不能在签名后加const**。  
但是倘若想要得到at的一个指定不修改原数据的

### 迭代器

+ iterator：可以++，可以修改
+ const iterator：不能++，可以修改
+ const\_iterator：可以++，不可修改  

### tips

+ 尽可能的把应该const的都const修饰
+ 不要重新发明轮子，用static\_cast与const\_cast把一个版本改为另一个版本
+ auto会丢弃const与引用特征，谨慎使用
+ 为每一个类都加上const\_iterator  

***

## Lec9.模板函数

cpp是强类型的语言，但是模板可以带来通用性，参数化数据类型。  

### Concept

这是cpp20引入的概念特性，是用于约束模板参数的工具。  

在这之前的模板函数中，如果传入的类型type并不支持函数中的操作（比如+），编译器会报冗长错误。  
为了让错误更早、更清晰，就引入这一特性，**在模板声明时就明确写出对类型的要求**。  

concept不是类，而是在**编译期的布尔表达式**，用于描述类型必须满足的条件：  
```
template<typename T>
concept MyConcept = reauires(T x, T y){
	// 需要的条件，譬如
	std::cout << x << y << std::endl;	// 可打印
	x + y;	// 可加
}
```
这样就可以用**MyConcept**来约束模板：  
```
template<typename T> requires MyConcept<T>
T func(T x, T y) {
	std::cout << x << y << std::endl;
	return x + y;
}
```

### 模板元编程

模板能提升效率，模板元编程在编译器就运行一次。  
`template<>`能够特化模板。  

除了模板之外，constexpr也能够实现编译器预计算，传递的参量应该也为const。  
也可以用来修饰变量、函数，最终编译出的文件更小、只在编译器计算，运行时直接出结果。  

值得一提的是，TMP能够像template一样纯粹是偶然发现，然后发掘出其图灵完备的性质。  

模板元编程的使用不频繁，但是有一些场景的用处：  
+ 优化矩阵、树等运算，譬如Eigen将长串计算不再直接生成大量临时对象，而是有复杂类型树，在最后赋值时才进行计算，节省开销
+ 针对策略优化，譬如单线程与多线程，对应不同的策略
+ 游戏，特别是engine中的极致优化、类型反射  

意义就在于**把复杂的逻辑判断和计算，从“运行时”强行搬运到“编译时”，以此换取极致的运行速度**。  

### 用泛型解决问题 generics

模板的重大意义在于**把算法与数据解耦**。  
对于统计字符串中某char的出现次数、统计vector中某数字的出现次数、统计单词在流中出现的次数，本质就是在一个范围内匹配计数一个指定值，方法是一致的，但是针对具体类型需要适配更改。  
而使用模板，这些就可以归结为一个：`std::count(xxx.begin(), xxx.end(), target)`。  

***

## Lec10. 函数与匿名函数

匿名函数不光是简洁好用，更能够自主的携带更多参数来使用。  

譬如对于下面的模板函数，其用于从输入流中按照规则筛选、计数符合要求的元素，通过预测函数UniPred：  
```
template<typename InputIt, typeneme UniPred>
int count_occurrences(InputIt begin, InputIt end, UniPred pred)
{
	int count = 0;
	for (auto iter = begin; iter != end; ++iter)
	{
		if (pred(*iter)) count ++;
	}
	return count;
}
```
若是这个预测函数逻辑限制很简单，比如大于5、小于3这种，算是硬编码在里面的还好，写死；  
但若想要灵活一些，动态的给定一个limit，此时入参大于2，在动态运行时这里是函数指针，根本不知道写什么，这就需要用匿名函数，随用随取：  
```cpp
int limit = 5;
auto isMoreThan = [limit](int n){return n > limit;};
```
使用时如此传参：`count_occurrences(vec.begin(), vec.end(), isMoreThan)`就很自然。  

### 匿名函数好处

综上：  
+ 需要一个简单的临时使用的短函数
+ 需要访问函数的局部变量
+ 需要更多逻辑、重载实现，用函数指针  

### Functor仿函数与Lambda表达式

仿函数是指重载了()运算符的类，普通函数无法记住状态（除非static，不安全），而类有成员可以记住状态，那么只要把类做的像函数一样可以调用，就能够结合两者的优点。  

因此能够做到定制化的函数，用构造函数传入内部参数，把函数定制成想要的样子，**代码逻辑+内部状态**就称为**闭包closure**。  

Lambda也仅是Functor的语法糖，编译器自动帮忙实现。  

### 函数对象

STL有一个标准对象，能够承接所有的调用东西，无论是正经函数、函数指针、仿函数、Lambda，只要满足函数签名，就都能够承接。  

但是通用的代价就是其运行性能开销比其他形式更大，因此在性能敏感部分，尽量不要用std::function。  


