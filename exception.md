1.java.lang.nullpointerexception     这个异常大家肯定都经常遇到，     异常的解释是     ”     程序遇上了空指针     ”     ，     简单地说就是调用     了未经初始化的对象或者是不存在的对象，     这个错误经常出现在创建图片，     调用数组这些操     作中，比如图片未经初始化，或者图片创建时的路径错误等等。对数组操作中出现空指针，     很多情况下是一些刚开始学习编程的朋友常犯的错误，     即把数组的初始化和数组元素的初始     化混淆起来了。     数组的初始化是对数组分配需要的空间，     而初始化后的数组，     其中的元素并     没有实例化，依然是空的，所以还需要对每个元素都进行初始化（如果要调用的话）   
  2.java.lang.classnotfoundexception     这个异常是很多原本在     jb     等开发环境中开发的程序员，把     jb     下的程序包放在     wtk     下编     译经常出现的问题，     异常的解释是     ”     指定的类不存在     ”     ，     这里主要考虑一下类的名称和路径是     否正确即可，如果是在     jb     下做的程序包，一般都是默认加上     package     的，所以转到     wtk     下    后要注意把     package     的路径加上。     3.java.lang.arithmeticexception     这个异常的解释是     ”     数学运算异常     ”     ，     比如程序中出现了除以零这样的运算就会出这样的     异常，     对这种异常，     大家就要好好检查一下自己程序中涉及到数学运算的地方，     公式是不是     有不妥了。     4.java.lang.arrayindexoutofboundsexception     这个异常相信很多朋友也经常遇到过，     异常的解释是     ”     数组下标越界     ”     ，     现在程序中大多     都有对数组的操作，     因此在调用数组的时候一定要认真检查，     看自己调用的下标是不是超出     了数组的范围，一般来说，显示（即直接用常数当下标）调用不太容易出这样的错，但隐式     （即用变量表示下标）     调用就经常出错了，     还有一种情况，     是程序中定义的数组的长度是通     过某些特定方法决定的，不是事先声明的，这个时候，最好先查看一下数组的     length     ，以免     出现这个异常。     5.java.lang.illegalargumentexception     这个异常的解释是     ”     方法的参数错误     ”     ，很多     j2me     的类库中的方法在一些情况下都会引     发这样的错误，比如音量调节方法中的音量参数如果写成负数就会出现这个异常，再比如     g.setcolor(intred,intgreen,intblue)     这个方法中的三个值，如果有超过２５５的也会出现这个     异常，     因此一旦发现这个异常，     我们要做的，     就是赶紧去检查一下方法调用中的参数传递是     不是出现了错误。     6.java.lang.illegalaccessexception     这个异常的解释是     ”     没有访问权限     ”     ，     当应用程序要调用一个类，     但当前的方法即没有对     该类的访问权限便会出现这个异常。对程序中用了     package     的情况下要注意这个异常。   

                                                                                   
异常指的是java在运行期出现的错误，不是敲javac时出现的错误。
用法：抛出（throw）异常，捕获（catch）异常。
其中ae是自己起的名字，是定义的对象
Error 系统错误，处理不了。Exception,自己能处理的





eg：



数组：



这样可以将字符串类型数组args 转换成double类型，☝



常用类：
Srting类，代表不可变的字符序列。
String s1="12345678",s2="abcdefghigklmnopqrstuvwswz";
s1.charAt(int x);//返回字符串中的第x个字符。
s1.length;//返回字符串长度。
s1.indexOf("xyz");返回字符串中第一个出现xyz的位置，int类型。
s1.indexOf("xyz",3);//返回字符串中第三个字符以后出现xyz的位置。
s1.equalsIgnoreCase(s2);//判断字符串s1与s2是否一样（忽略大小写）。
s1.replace('1','7');//将s1字符串中的1用7来替换。









将“2.0”先转换为Double类对象，在调用后面方法。
分别把括号里数用2,8,16进制表示出来。

枚举类型：


容器：1136：一个图    ，一个类：collections，3个知识点: 增强for，泛型，自动（打，解包）；和6个接口：collection，set    ,list,     map   iterator    ,comperable。

图☝



contains：是不是包含这个对象，equals哪个对象
retainall：求两个集合类交集。


iterator:遍历元素的接口。



list：

类：java.util.Collections





put:扔进一对键和值；    get：通过key找到对应的value；   remove：吧这个key对应的value去掉，随之key也去掉了
contains，，是不是包含；   size，多少对对象；   isEmpty:是不是空的；
jdk1.5后支持



泛型：































































































