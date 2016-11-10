#String源码


```java
/**
 * Created by nurmemet on 2016/11/9.
 */
public class StringTest  {

    public  static void main(String[] ars){
        String str="nurmemet";
        str+="dfsdfdsf";
        System.out.println(str);
    }
}


```
String 是不可变的，通过下面代码的可以看出，拼接字符串之前的str跟拼接之后的str不是同一个内存对象
```smali
.class public LStringTest;
.super Ljava/lang/Object;
.source "StringTest.java"


# direct methods
.method public constructor <init>()V
    .registers 1

    .prologue
    .line 4
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    return-void
.end method

.method public static main([Ljava/lang/String;)V
    .registers 4
    .param p0, "ars"    # [Ljava/lang/String;

    .prologue
    .line 7
    const-string v0, "nurmemet"

    .line 9
    .local v0, "str":Ljava/lang/String;
    new-instance v1, Ljava/lang/StringBuilder;

    invoke-direct {v1}, Ljava/lang/StringBuilder;-><init>()V

    invoke-virtual {v1, v0}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string v2, "dfsdfdsf"

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    invoke-virtual {v1}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;

    move-result-object v0

    .line 11
    sget-object v1, Ljava/lang/System;->out:Ljava/io/PrintStream;

    invoke-virtual {v1, v0}, Ljava/io/PrintStream;->println(Ljava/lang/String;)V

    .line 12
    return-void
.end method


```

下面是String hashCode方法源码,可以看出只要字符串相同，hashCode就相同
```java
   public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }

```

通过源码可以发现String是用 final char[] value来实现的，StringBuffer是线程安全的，StringBuilder不是线程安全的