#java 面试题

###静态初始化块儿，实例初始化块儿执行顺序
```java
/**
 * Created by nurmemet on 10/13/2016.
 */
public class Test2 {
    private int t=3;
    //1 静态初始化块儿
    static {
        m=2;
    }
    public Test2(){
        t=5;
    }

    public static  int m ;
    //2 实例初始化块儿
    {
        t=4;
    }

    public static void main(String[] arvs) {
        System.out.println(m);
    }
}

```

转换成smali之后
```smali
.class public LTest2;
.super Ljava/lang/Object;
.source "Test2.java"


# static fields
.field public static m:I


# instance fields
.field private t:I


# direct methods
.method static constructor <clinit>()V
    .registers 1

    .prologue
    .line 7
    const/4 v0, 0x2

    sput v0, LTest2;->m:I

    .line 8
    return-void
.end method

.method public constructor <init>()V
    .registers 2

    .prologue
    .line 9
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    .line 5
    const/4 v0, 0x3

    iput v0, p0, LTest2;->t:I

    .line 16
    const/4 v0, 0x4

    iput v0, p0, LTest2;->t:I

    .line 10
    const/4 v0, 0x5

    iput v0, p0, LTest2;->t:I

    .line 11
    return-void
.end method

.method public static main([Ljava/lang/String;)V
    .registers 3
    .param p0, "arvs"    # [Ljava/lang/String;

    .prologue
    .line 21
    sget-object v0, Ljava/lang/System;->out:Ljava/io/PrintStream;

    sget v1, LTest2;->m:I

    invoke-virtual {v0, v1}, Ljava/io/PrintStream;->println(I)V

    .line 23
    return-void
.end method

```

* 执行顺序  静态初始化函数（.method static constructor <clinit>()V）-> 实例初始化函数 （.method public constructor <init>()V）
* 所有静态变量的初始化都在静态初始化函数内执行
* 所有实例变量的初始化都在实例初始化函数（构造函数）内执行
* 静态初始化块儿也在静态初始化函数内执行
```java 
static {
    m=6;
}
private static int m=5;

```
相当于
```java
static {
    m=6;
}
private  static int m;
static{
    m=5;
}
```
所有静态初始化块儿都合并一起编译生成静态初始化函数，上面代码生成以下静态初始化函数
```smali
# direct methods
.method static constructor <clinit>()V
    .registers 1

    .prologue
    .line 6
    const/4 v0, 0x6

    sput v0, LTest2;->m:I

    .line 8
    const/4 v0, 0x5

    sput v0, LTest2;->m:I

    return-void
.end method
```
所以最后m的值为5
* 实例初始化块儿都在实例初始化块儿内执行

```java 
 {
    m=6;
}
private  int m=5;

```
相当于
```java
 {
    m=6;
}
private  int m;
{
    m=5;
}
```
所有实例初始化块儿都合并一起编译生成实例初始化函数，按照定义顺序合并,生成的samli代码为
```smali
# direct methods
.method public constructor <init>()V
    .registers 2

    .prologue
    .line 4
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    .line 6
    const/4 v0, 0x6

    iput v0, p0, LTest2;->m:I

    .line 8
    const/4 v0, 0x5

    iput v0, p0, LTest2;->m:I

    return-void
.end method
```

关于实例初始化块儿还有一点
```java
 {
        m=6;
    }
    private  int m=5;

    public Test2(){
        m=7;
    }

```
转换之后
```smali
# direct methods
.method public constructor <init>()V
    .registers 2

    .prologue
    .line 10
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    .line 6
    const/4 v0, 0x6

    iput v0, p0, LTest2;->m:I

    .line 8
    const/4 v0, 0x5

    iput v0, p0, LTest2;->m:I

    .line 11
    const/4 v0, 0x7

    iput v0, p0, LTest2;->m:I

    .line 12
    return-void
.end method
```
最后m的值为7,注意以上转换顺序



####下面是一个面试题
```java
public class Test1 {

    public static int k=0;
    public static Test1 t1=new Test1("t1");
    public static Test1 t2=new Test1("t2");
    public static int i=print("i");
    public static int n=99;

    public int j=print("j");

    {
        print("构造块");
    }

    static {
        print("静态块");
    }



    private static int print(String str) {
        System.out.println(++k+":"+str+"   i="+i+"   n="+n);
        ++n;
        return ++i;
    }

    public Test1(String str){
        System.out.println(++k+":"+str+"   i="+i+"   n="+n);
        ++n;++i;
    }
    public static void main(String[] args) {
       Test1 test1=new Test1("init");
    }


}

```

```smali
.class public LTest1;
.super Ljava/lang/Object;
.source "Test1.java"


# static fields
.field public static i:I

.field public static k:I

.field public static n:I

.field public static t1:LTest1;

.field public static t2:LTest1;


# instance fields
.field public j:I


# direct methods
.method static constructor <clinit>()V
    .registers 2

    .prologue
    .line 3
    const/4 v0, 0x0

    sput v0, LTest1;->k:I

    .line 4
    new-instance v0, LTest1;

    const-string v1, "t1"

    invoke-direct {v0, v1}, LTest1;-><init>(Ljava/lang/String;)V

    sput-object v0, LTest1;->t1:LTest1;

    .line 5
    new-instance v0, LTest1;

    const-string v1, "t2"

    invoke-direct {v0, v1}, LTest1;-><init>(Ljava/lang/String;)V

    sput-object v0, LTest1;->t2:LTest1;

    .line 6
    const-string v0, "i"

    invoke-static {v0}, LTest1;->print(Ljava/lang/String;)I

    move-result v0

    sput v0, LTest1;->i:I

    .line 7
    const/16 v0, 0x63

    sput v0, LTest1;->n:I

    .line 16
    const-string v0, "\u9759\u6001\u5757"

    invoke-static {v0}, LTest1;->print(Ljava/lang/String;)I

    .line 17
    return-void
.end method

.method public constructor <init>(Ljava/lang/String;)V
    .registers 5
    .param p1, "str"    # Ljava/lang/String;

    .prologue
    .line 27
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    .line 9
    const-string v0, "j"

    invoke-static {v0}, LTest1;->print(Ljava/lang/String;)I

    move-result v0

    iput v0, p0, LTest1;->j:I

    .line 12
    const-string v0, "\u6784\u9020\u5757"

    invoke-static {v0}, LTest1;->print(Ljava/lang/String;)I

    .line 28
    sget-object v0, Ljava/lang/System;->out:Ljava/io/PrintStream;

    new-instance v1, Ljava/lang/StringBuilder;

    invoke-direct {v1}, Ljava/lang/StringBuilder;-><init>()V

    sget v2, LTest1;->k:I

    add-int/lit8 v2, v2, 0x1

    sput v2, LTest1;->k:I

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(I)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string v2, ":"

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    invoke-virtual {v1, p1}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string v2, "   i="

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    sget v2, LTest1;->i:I

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(I)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string v2, "   n="

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    sget v2, LTest1;->n:I

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(I)Ljava/lang/StringBuilder;

    move-result-object v1

    invoke-virtual {v1}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;

    move-result-object v1

    invoke-virtual {v0, v1}, Ljava/io/PrintStream;->println(Ljava/lang/String;)V

    .line 29
    sget v0, LTest1;->n:I

    add-int/lit8 v0, v0, 0x1

    sput v0, LTest1;->n:I

    sget v0, LTest1;->i:I

    add-int/lit8 v0, v0, 0x1

    sput v0, LTest1;->i:I

    .line 30
    return-void
.end method

.method public static main([Ljava/lang/String;)V
    .registers 3
    .param p0, "args"    # [Ljava/lang/String;

    .prologue
    .line 32
    new-instance v0, LTest1;

    const-string v1, "init"

    invoke-direct {v0, v1}, LTest1;-><init>(Ljava/lang/String;)V

    .line 33
    .local v0, "test1":LTest1;
    return-void
.end method

.method private static print(Ljava/lang/String;)I
    .registers 4
    .param p0, "str"    # Ljava/lang/String;

    .prologue
    .line 22
    sget-object v0, Ljava/lang/System;->out:Ljava/io/PrintStream;

    new-instance v1, Ljava/lang/StringBuilder;

    invoke-direct {v1}, Ljava/lang/StringBuilder;-><init>()V

    sget v2, LTest1;->k:I

    add-int/lit8 v2, v2, 0x1

    sput v2, LTest1;->k:I

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(I)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string v2, ":"

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    invoke-virtual {v1, p0}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string v2, "   i="

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    sget v2, LTest1;->i:I

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(I)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string v2, "   n="

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    sget v2, LTest1;->n:I

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(I)Ljava/lang/StringBuilder;

    move-result-object v1

    invoke-virtual {v1}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;

    move-result-object v1

    invoke-virtual {v0, v1}, Ljava/io/PrintStream;->println(Ljava/lang/String;)V

    .line 23
    sget v0, LTest1;->n:I

    add-int/lit8 v0, v0, 0x1

    sput v0, LTest1;->n:I

    .line 24
    sget v0, LTest1;->i:I

    add-int/lit8 v0, v0, 0x1

    sput v0, LTest1;->i:I

    return v0
.end method

```

输出结果为  
1:j   i=0   n=0  
2:构造块   i=1   n=1  
3:t1   i=2   n=2  
4:j   i=3   n=3  
5:构造块   i=4   n=4  
6:t2   i=5   n=5  
7:i   i=6   n=6  
8:静态块   i=7   n=99  
9:j   i=8   n=100  
10:构造块   i=9   n=101  
11:init   i=10   n=102  