## JAVA基础之--JAVA面向对象


# java是解释型语言，c是编译型语言，编译型语言报错时，不往下进行执行，而解释型语言报错，会进行网下编译。


检查Javajdk版本


boolean类型不可以转换为其他的数据类型

byte  i=1;
byte i2=4;
byte3=(byte)（i+i2）;必须强制转换。
--------------------------------------------------------------------------------
方法调用：
同类方法调用：eg:        m();
对象关系：
一般和特殊的关系（运动员和游泳运动员），整体和部分关系（球队和队员，队长的关系），实现关系



new出来的再堆内存里面。


构造方法和类名完全一致。
创建一个新的对象，是使用 new+构造方法来创造的。


构造方法：
             没有任何返回值，系统将默认给类添加一个无参构造，当你新添加一个构造方法时，系统默认无参构造将取消，类如：
      class Fab1{
    int id;
    int age;
    Fab1(int i,int j){
        id=i;
        age=j;
        System.out.println(id+" "+age);
    }
}
 class Fab{

    public static void main(String[] args){
        Fab1 f=new Fab1();


    }
}
这时，系统将报错，报错类型：   
  


举个栗子：：：
class BirthDate{
     private int day;
     private int month;
     private int year;
     public BirthDate(int d,int m,int y){
         day=d;month=m;year=y;

     }
     public void setDay(int d){
         day=d;
     }
     public void setMonth(int m){
         month =m;
     }
     public void setYear(int y){
         year=y;
     }
     public int getDay(){
         return day;
     }
     public int getMonth(){
         return month;
     }
     public int getYear(){
         return year;
     }
     public void display(){
         System.out.println(day+" -"+month+"-"+year);
     }
 }
 public class Test{
     public static void main(String[] args){
        Test test=new Test();
        int date=9;
        BirthDate d1=new BirthDate(7,7,1970);
        BirthDate d2=new BirthDate(1,1,2000);
        test.change1(date);//只把date值复制给i,并没有改变date的值。
        test.change2(d1);//(解释：
 ☞
    只改变b  没改变d1;

)
        test.change2(d2);
        System.out.println("date="+date);
        d1.display();
        d2.display();
     } 
     public void change1(int i){
         i=1234;
     }
     public void change2(BirthDate b){
         b=new BirthDate(22,2,2004);
     }
     public void change3(BirthDate b){
         b.setDay(22);
     }
 }

方法重载：
       一个类中，可以有定义相同的名字，但参数不同的多个方法，调用时，根据不同的参数来选择对应的方法。
但是这种情况不构成重载。

this关键字：
    
它的值是当前对象引用。
1：在这，this用来处理成员变量与参数重名的情况。
2：

谁调用这个方法，this就代表谁，图中☝leaf.increament().increament().print();可以理解为：
(leaf.increament)被return this,而，this=(leaf)因为当前调用的是leaf，所以，该方法被调用两次。
static关键字：

1：可以用静态成员变量来计数。
2：不可以从静态方法中访问非静态成员eg：
                        
                                                ☟


3：可以通过对象引用或者类名直接访问静态成员eg：

          ☞              
packge关键字：
  1：  创建类引用包名，必将该类放到指定目录下。
2：当引用此类创建对象时 ，必须将包名写进去eg：
            
或者：
                
其中*代表将此目录下的所有类都引用过来。

继承父类的所有方法，包括private,但是，子类不能访问父类私有变量和方法。
方法重写：
当在父类的方法不满意时，可以进行方法重写。重写时注意一下：
    

super关键字：
看一个栗子：
其中，在main方法中，只要没有调用的变量，初始值都为0，其中，只要没有super指示，调用的是子类的变量，有super时，指向父类变量，以上编译结果为：
    
继承中的构造方法法：


如果调用super，必须写在子类构造方法的第一行。
如果子类的构造方法中没有显示调用父类构造方法，系统默认调用的是父类无参构造，若此时父类中没有无参构造，则编译出错。

方法重写和继承中构造方法的继承练习：

分析：其中，先从mian看：首先分析B b=new B();  引用子类无参构造方法之前，并没有发现使用super来引用父类的构造方法，系统将默认访问父类无参构造 ；即A（）{print("A()")};打印出“A()”,然后访问子类构造：打印出“B（）”在分析b.f();其中方法被重写，故只访问子类中方法：“B：f（）”.
注意：当this（形参）时，指的是这个类的其他和形参相匹配的构造方法的引用
Object类：
根基类，从类的声明中没有使用extends关键字，则默认根基类为Object类。
1；toString类：

2：equals
重写：

其中，instanceof用来比较。，用法：

对象转型（casting）;

eg：
判断对象是不是属于此类或子类。
对象转型，向上转型方法。
多态之动态绑定：

    
多态条件：1：要有继承，2：要用重写，3：父类引用指向子类对象。
抽象类：
当一个类中有抽象方法时，该类必须要定义为抽象类abstract。        父类中有抽象方法时子类必须重写


final关键字：
final，固定常量，不能被改变不能被重写

接口 interface：
接口是一种特殊的抽象类，  作为接口来说，可以实现接口或叫从接口继承，也是多继承（java中不能多继承，只能通过接口实现），它里面成员不属于专门对象。

接口所有方法都是抽象方法。实现接口方法：（class  类名 implements 接口名）




实现接口中存在多态：
    Singer s1=new Student("le");
接口之间可以相互继承，但实现接口时必须重写此接口和继承接口的所有方法。


面向对象总结：提纲’‘’‘’‘’‘

细节注意： 构造方法，和类同名，没有返回值（连void都不能写）；
继承，继承所有成员，包括private，但无法访问private成员或方法。
抽象类：父类中有抽象方法时子类必须重写，








