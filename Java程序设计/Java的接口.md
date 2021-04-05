# 接口

由于《Java编程思想》第四版在2007年完成，基于当时最新的Java，而工业界流行的Java8在14年就出来了，并且Java8对于接口更新了很多特性，所以《Java编程思想》中的部分内容就是过时的了。我们后面更多得会提到的是[《On Java 8》](https://lingcoder.github.io/OnJava8/#/)这本书，以及使用Java8标准（不知道考试的时候会不会因为这个被扣分。。。）

## 接口的定义和设计背景

我觉得接口是Java区分于C++的一个非常重要的特征，接口让Java中程序的耦合性不再那么的紧密，让Java更容易完成各种“设计模式”（至少我是这么认为的）。我们用一个例子来说明，顺便介绍接口的声明方法之类的基础的问题：（这里的代码来自《On Java 8》）

### “完全解耦”

假设我们有这么一个需求，或者说主代码

```java
public class Applicator {
    public static void apply(Processor p, Object s) { // 顺便，这里也体现了Java让所有类都是Object的子类的好处
        System.out.println("Using Processor " + p.name());
        System.out.println(p.process(s));
    }

    public static void main(String[] args) {
        String s = "We are such stuff as dreams are made on";
        apply(new Upcase(), s);
        apply(new Downcase(), s);
        apply(new Splitter(), s);
    }
}
```

`Applicator`的`apply`方法接受一个`Processor`、可以看作一个车间，和一个`Object`、可以看作需要被处理的东西

我们很容易给出一些实现，比如讲`s`的字符全部转化成大写、小写，如下：

```java
class Processor {
    public String name() {
        return getClass().getSimpleName(); // 这里会返回RTT
    }

    public Object process(Object input) {
        return input; // 什么都不处理
    }
}
class Upcase extends Processor {
    // 返回协变类型
    @Override
    public String process(Object input) {
        return ((String) input).toUpperCase();
    }
}
class Downcase extends Processor {
    @Override
    public String process(Object input) {
        return ((String) input).toLowerCase();
    }
}
```



但是如果我们现在有一个`Filter`类，其也有一个方法`process`，我们很想让其也可以用在`apply`中，因为它也有方法`process`，但是很明显由于`apply`里面接受的是`Processor`，我们并不能让两个不在一条继承链上的类互相转化，所以即使`Filter`类也有`process`方法、即使我们把`apply`的第一个参数改成`Filter`就可以跑起来，但是实际上还是做不到的。我们称这种情况为`Applicator`的`apply`方法和`Processor`过于耦合，这阻止了`Applicator`的`apply()`方法被复用



我们的需求就是如果两个类都有`process`方法，能不能让他们都可以作为某个参数输入到另一个方法中呢？答案是：使用接口

我们设计一个接口叫`Processable`（大多数接口都使用形容词来命名），它应该让所有拥有这个接口的类都拥有`process`方法。具体如下：

```java
package TestInterfaceProcess;

// interfaces/Applicator.java
import java.util.*;

interface Processable {
    default String name() {
        return getClass().getSimpleName(); // 这里会返回RTT的名字
    }

    default Object process(Object input) {
        return input; // 什么都不处理
    }

    String s="This final and static"; // right, can create static instance
    public final int cnt=0; // 默认是final的，所以这里的cnt是没有办法改变值的
    int num=0;
    static void getcnt(){ // right, can create public final static method
//        ++num; // wrong, num is final
    }

    public static void main(String[] args) {
//        new Processable(); // wrong, can't create a interface
        
    }
}
class Upcase implements Processable {
    @Override
    public String process(Object input) { // must be 'public'
        return ((String) input).toUpperCase();
    }
}
class Downcase implements Processable {
    @Override
    public String process(Object input) {
        return ((String) input).toLowerCase();
    }
}

public class Applicator {
    public static void apply(Processable p, Object s) {
        System.out.println("Using Processor " + p.name());
        System.out.println(p.process(s));
    }

    public static void main(String[] args) {
        String s = "We are such stuff as dreams are made on";
        apply(new Upcase(), s);
        apply(new Downcase(), s);
    }
}

```

这里逐一介绍下：

### 接口的声明和内容

一个接口表示：所有实现了该接口的类看起来都像这样

声明`interface`只需要把声明类的时候的`class`改成`interface`就行，但是要注意`interface`一定是`abstract`的，即使没有写明，所以我们并不能`new`一个接口类。另外接口的访问控制和类的是一样的就不介绍了

接口中的所有方法都会被默认是`public`的，而且一个类实现接口的方法的时候一定要把覆写部分的访问控制设定成`public`，这是Java规定的（相对的，在继承中一个继承类的方法和基类的被覆写的方法的访问控制权限完全可以不一样）

接口中可以有方法的实现（Java8特性），但是需要增加域`default`修饰

接口中可以有静态方法（Java8特性），这样可以方便存放一些应该属于接口的方法

接口中也可以有变量，但是需要注意变量默认是`public static final`的，即使你不这么声明（声明成别的样的会被抱错。。。另外实际上`static`就意味着只能隐藏不会被覆写，不太懂为什么一定要加上`final`）。另外和类的`static`量一样，Java会尽量优化这个过程，但是一旦`static final`的量被赋值就不能再改变（或指向别处）



### 实现接口的方法

如果你的类实现了某个接口，需要在类名和继承内容后面加上`implements [interface,...]`。值得注意的是，一个类可以实现多个接口，我们稍后会介绍解决命名冲突等实现多个接口的时候需要注意的问题

在类内应该覆写所有没有`default`的方法，如果`default`在此类里的实现应该不一样，也需要覆写。覆写最好加上关键字`@Override`

覆写的方法访问权限必须是`public`



## 一次实现多个接口

Java支持一个类实现多个接口——注意这并不会导致数据的重复问题，因为接口里并没有非`static`的属性（或者说变量）

想要实现多个接口，只需要在`implements`后面多写接口就行

下面是一个例子，这里`TesInterface`同时有两个功能：实现比较接口实现Java自带排序的比较功能；实现`Readable`接口使之可以构造`Scanner`

```java
package TestInterfaceProcess;

import org.jetbrains.annotations.NotNull;

import java.io.IOException;
import java.nio.CharBuffer;
import java.util.*;

public class TesInterface implements Comparator<Integer> ,Readable {
    @Override
    public int compare(Integer o1, Integer o2) {
        System.err.println("o1: " + o1 + " ");
        System.err.println("o2: " + o2 + " ");
        return o1.compareTo(o2);
    }

    private static final char [] CHS= "0123456789".toCharArray();
    private int count;
    private final Random r=new Random();
    public TesInterface(int count){
//        System.err.println("count: " + count + " ");
        this.count=count;
    }
    @Override
    public int read(@NotNull CharBuffer cb) throws IOException {
        if (count == 0) {
            return -1; // indicates end of input
        }
        --count;
        int len=r.nextInt(Integer.MAX_VALUE)%8+1; // r.nextInt()貌似会出现负数。。。
        System.err.println("len: " + len + " ");
        for(int i=0;i<len;++i){
            cb.append(CHS[r.nextInt(CHS.length)]);
//            System.out.println(cb);
        }
        cb.append(' ');
        return len+1;
    }

    public static void main(String[] args) {
        int len=10;
        Scanner cin=new Scanner(new TesInterface(len));
        Integer [] nums=new Integer[len];
        int tot=0;
        while(cin.hasNext()){
            nums[tot++]= cin.nextInt(); // 没有任何问题。。。包装类会在后面介绍
            System.err.println("tot: " + tot + " ");
        }
        Arrays.sort(nums,0,tot,new TesInterface(0));
        for(int i=0;i<tot;++i){
            System.out.println(nums[i]);
        }
    }
}

```

当然这里的例子很生硬，但是确实反映了这种特性的特征和写法



## 接口的继承

接口是可以继承自接口的，产生的新接口会拥有基接口的所有方法和自己的所有方法。在单继承的时候，一个基接口的方法无论有没有`default`关键字，都可以在继承接口里使用`default`来覆写。当然你也可以选择不覆写，这样实现继承接口的类的时候，就需要覆写所有基接口没有实现并且在继承接口也没有实现的、和继承接口新增的没有实现的方法

但是Java中接口是支持多继承的：C++中类默认支持多继承，但是这样会导致非常多的问题，比如如果`A`是`B`和`C`的基类，而`D`继承自`B`和`C`，那`D`的实例中应该有几个`A`的数据呢？这种简单的多继承C++还能控制，但是对于更复杂的继承情况，多继承就显得很笨重了。在Java中类只有单继承，但是接口可以多继承

接口的多继承实际上也没什么特别的。。。因为接口不存在成员变量，所以少了很多类里继承的麻烦事；同时接口的方法在多继承的过程中的表现实际上和单继承差别不大，值得注意的只有：如果有接口`A`和`B`，其有相同形式的方法`fun`，`A`实现了这个方法而`B`没有，接口`C`继承自`A`和`B`，那么由于`fun`在`A`和`B`中一个有一个没有默认实现，所以需要专门覆写；还是`A`、`B`、`C`，如果`A`和`B`都没有实现`fun`，那么`C`就不会有问题；如果`A`和`B`都实现了`fun`，`C`需要专门覆写`fun`（实现与否无所谓）

另外在一个类同时实现接口并继承的时候，如果基类中和接口中有一个相同形式的方法的话，也需要专门去实现



和类的表现一样，接口中的静态不变内容是不会被覆写的，甚至我觉得都不能说被隐藏了。。。因为即使是在继承接口中也只能使用`Base.num`的方式访问/调用静态内容









