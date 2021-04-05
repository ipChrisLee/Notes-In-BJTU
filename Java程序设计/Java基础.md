# 行文问题

《？》是有疑问的点

《！》是需要填坑的点

# 基础知识

## Java特性

* 对于任何一个变量（无论基础类型和类），Java要求必须经过new才能使用，否则通不过编译

* Java不会将所有空变量都看作`false`，也就是需要`boolean`的地方语句的返回值必须是`boolean`

  比如在C++中我们经常写`while(cnt--)`，但是在Java中就必须写`while(cnt-->0)`（~~看起来像cnt趋向于0~~）

* Java的函数如果不用接受参数的话不能使用`void`占位

  `static public void fun(void){}`会报错

* 注意Java没有`const`，但是`const`是一个保留字，所以并不能声明一个叫做`const`的变量

* C++中函数在Java中一般叫做方法，虽然两者似乎并没有实质上的区别

  Java中(几乎)一切都是对象，就连`main`一般都是放在`Main`类的`main`方法中

* 尽管一切都看作对象，但是标识符的操纵实际上是对象的一个“引用”，并且在Java中程序员并不需要在意对象的生存周期。在C++中我们一般使用`delete`来清除不再被使用的对象，并且还需要特别注意析构函数的设计，但是在Java中我们无需这么做，Java会自己控制类的回收

* Java里除了基本数据类型(`int`，`boolean`，`char`，`byte`，`short`，`long`，`float`，`double`)之外的类，本质都是引用

  另外从某种意义上来说这里的引用说成指针会更好（具体相关知识可以查看[Java 到底是值传递还是引用传递？ - Intopass的回答 - 知乎 ](https://www.zhihu.com/question/31203609/answer/50992895)）

* Java是半编译半解释型语言，具体过程见下图

  ![图源知乎](资源库/45e5e8e74ed0fec7c782e30ac8c4edd7_r.jpg)

  也就是Java还是会编译的，实际上很多错误也会在编译的时候就被发现

* Java的输入一般使用的是`Scanner`类及其类函数(一般被命名成`next...`)

  需要特别注意的是，`.nextLine`会读取所有行内容直到`'\n'`，所以如果一道题说"第一行为一个整数n表示n组样例，接下来n行样例"，我们可以这么写

  ```Java
  Scanner cin=new Scanner("System.in");
  int n=cin.nextInt();
  cin.nextLine(); // 吸收掉多余的'\n';
  while(n-->0){
  	String[] strs=cin.nextLine().split(" "); // 这样数组里每一个位置都是一个存储信息的String，当然，这么做会导致效率其低
  }
  ```

* Java支持javadoc，下面的就会成一个javadoc

  ```java
  /**
   *
   *
   */
  /** */
  ```

  会自动变成Javadoc，可以在IDEA里面看效果



## 类相关

* `native`是什么：

  在Java中`Object`类(后面介绍)中，有一个方法`hashCode()`，有一个关键字叫`native`，那这个关键字是干嘛用的呢？

  根据[简书：JAVA中的native是什么？](https://www.jianshu.com/p/429dc9aa2ce4)的内容，认为这表示这个方法是一个不在Java中实现的接口，所以看到这个方法(主要是在Java源码中)就可以默认它有着一种实现方式，只不过不在Java类内实现
  
* Java的文件名`FileName.java`里面必须只有`FileName`这一个类是public的，其他的不可是`public`

* 在C++中我们对于一个对象很多时候会默认其有一种`==`的运算符重载，但是在Java中并不存在运算符重载！实际上

  ```Java
  Scanner cin=new Scanner("System.in");
  String str1=cin.nextLine();String str2=cin.nextLine();
  if(str1==str2){
      // todo
  }
  ```

  里面的`str1==str2`比较的是两个引用是否相等！对于Java来说，如果想要比较`String`（或者说两个非基本类）是否相等，应该使用`str1.equals(str2)`的形式

* Java是没有运算符重载的！比如Java有一个类叫做高精度整数`BigInteger`，这个可以看成一个位数不限的整数(常用在算法竞赛中)，但是它本质是一个类，所以用起来很麻烦，比如下例

  这个计算的是`(x-y)%z`

  ```c++
  import java.math.BigInteger;
  import java.util.Scanner;
  public class Main {
      public static void main(String[] args) {
          Scanner cin = new Scanner(System.in);
          BigInteger x=BigInteger.valueOf(100);
          BigInteger y=new BigInteger(String.valueOf(50)); // 本质是使用字符串初始化
  //        BigInteger z=new BigInteger(BigInteger.TWO); // 这个过不了编译，似乎是因为BigInteger并没有去实现复制构造函数
          BigInteger z=BigInteger.TWO; // BigInteger.TWO 就是valueOf(2)
          System.out.print(x.subtract(y).mod(z));
      }
  }
  ```

  这里绝对不能写`x++`的语句

* 在Java中只能使用`TYPE t=new TYPE() `的方式来新建对象，而不存在在C++中的`TYPE t(xx);`的方式



## 其他进阶特性

* 我们在C++中可以在声明函数的时候标注上其**期望**抛出的异常，如下：

  ```c++
  void func(void) throw (expt,int){
      // ...
  }
  ```

  这里的`throw (expt,int)`表示`func`函数期望抛出`expt`或者`int`的异常，但是注意这不是强制的，也就是如果有其他的异常被也是可以的，编译器不会对此报错

  但是在Java中，这里标注的必须是所有可能被抛出的类，而且这些类必须继承自异常类

  具体可以参考下面代码

  ```java
  class tes {
      public void expt() throws intExpt,doubleExpt{
          Random r=new Random();
          if(r.nextInt()>0){
              throw new intExpt();
          }else{
              throw new doubleExpt();
          }
      }
  }
  
  class intExpt extends Exception{
      int x;
  }
  
  class doubleExpt extends Exception{
      double x;
  }
  ```

* 我们知道C++里面如果一个类我们写成下面这样

  ```c++
  // in tes.hpp
  class tes{
  private:
  	int t;double x;    
  public:
  	tes(){
          t=0;x=0;
      }    
  }; // 这里还有个不同，Java的类最后并不需要分号
  
  // in main.cpp
  tes tt;
  ```

  main里面的tt变量实际上会调用默认构造函数

  但是如果我们在Java中类似地写

  ```java
  public class tes{
      int t;double x;
  }
  public class Main{
      public static void main(){
          tes tt;
          tt.x=10;
      }
  }
  ```

  就会被报错，应该写成`tes tt=new tes();`

  这是Java比较好的一点，如果没有默认初始化/赋值的话，编译器会检查出来并直接抛ERROR，而在C++中这种错误只会被抛WARNING

  

  另外，拓展一下，Java里面的数组如果使用C++的方式可能会出错

  ```c++
  class tes{
  private:
      int len;
      char * str;
  public:
      tes():len(0){
          str=(char*)malloc(sizeof(char)*(len+1));
  		str[len]='\0';
      }
  };
  // in main
  tes ts[20];
  ```

  在C++里上述语句会调用20次`tes`类的默认构造函数，但是如果等效到Java里就会出现意想不到的效果

  ```java
  public class tes {
      public int num;
      public String str; // 别忘了这里是一个引用！
      public tes(){
          num=0;str=new String(Integer.toString(num));
      }
  }
  import java.util.Scanner;
  public class Main {
      public static void main(String[] args) {
          tes [] tts=new tes[5];
      }
  }
  ```

  看起来`tts`里面每个元素应该都有值了，但是实际上这里的元素根本就没有被赋值！

  应该加上

  ```java
  for(int i=0;i<tts.length();i++){
      tts[i]=new tes();
  }
  ```

* Java并不支持默认形参

* 如果在C++中提前return函数，导致后面的部分没有办法被执行到，顶多会被报WARNING，但是在Java中类似行为(比如下面的语句)会过不去编译

  ```java
  public class Main {
      public static void main(String[] args) {
          Scanner cin = new Scanner(System.in);
          System.out.printf("%s\n","ABC"+100);
          return;
          Object oo=new Object();
          System.out.print(oo.toString());
      }
  }
  ```

* 在C++中我们可以使用`for(auto & x:vec)`来安全地循环一个vec，并且可以通过`x`这个引用改变vec里面数据的值，但是在Java中的`foreach`并不具有这样的性质。具体见下

  ```java
  int [] x=new int[4];
  for (Integer xx:
       x) {
      xx=1;
  }
  for (int xx:
       x) {
      System.out.println(xx);
  }
  ```

  这里输出出来xx都是0，也就是Java给出的初始值，并不会得到1

  即使把上面的int改成`Integer`也不行

* Java也有命令行

  

* 





# 竞赛Java

虽然使用Java参加算法竞赛有点蠢，但是这里还是介绍下吧

## 快读快写

~~MXY曾经说过，Java不用快读就是自杀~~

但是C++的流就已经够慢了（还是为了和`scanf`、`printf`同步），Java的流慢也很正常，下面的网址有Java的快读快些模版，在本地（指LYS的macOS上）环境，输入`_quick_reader`就可以获得快读模版，输入`_quick_writer`就可以获得快写模版

[CSDN:ACM javaIO 快速 读写](https://blog.csdn.net/weixin_38481963/article/details/84799757)



