#类型转换

```java
/**
 * Created by nurmemet on 2016/10/27.
 */
public class main {


    public static void main(String[] argrs) {
        int num = 10;
        String str01 = String.valueOf(num < 30 ? 20 : 30);
        String str02 = String.valueOf(num < 30 ? 20 : 30.0);
        System.out.println("str0与str1是否相等：" + str01.equals(str02));

    }

}


```

```smali
.class public Lmain;
.super Ljava/lang/Object;
.source "main.java"


# direct methods
.method public constructor <init>()V
    .registers 1

    .prologue
    .line 4
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    return-void
.end method

.method public static main([Ljava/lang/String;)V
    .registers 7
    .param p0, "argrs"    # [Ljava/lang/String;

    .prologue
    const/16 v4, 0x1e

    .line 8
    const/16 v0, 0xa

    .line 9
    .local v0, "num":I
    if-ge v0, v4, :cond_31   #如果条件成立，往下执行，如果不成立，则跳转到cond_31执行，这里是20<30成立往下执行

    const/16 v3, 0x14   #20  v3赋值20,继续往下执行

    :goto_8
    invoke-static {v3}, Ljava/lang/String;->valueOf(I)Ljava/lang/String;    #调静态函数String.valueOf病把v3作为参数传过去

    move-result-object v1    #运算结果放到v1

    .line 10
    .local v1, "str01":Ljava/lang/String;   #本地寄存器v1
    if-ge v0, v4, :cond_33    #如果条件成立往下执行，不成立则跳转到cond_33执行,这里10<30条件成立，继续往下执行

    const-wide/high16 v4, 0x4034000000000000L    # 20.0 给寄存器赋20.0

    :goto_10
    invoke-static {v4, v5}, Ljava/lang/String;->valueOf(D)Ljava/lang/String;   #调静态函数String.valueOf并把v4,v5作为参数传过去

    move-result-object v2   #调用结果放到v2里

    .line 11
    .local v2, "str02":Ljava/lang/String;
    sget-object v3, Ljava/lang/System;->out:Ljava/io/PrintStream;

    new-instance v4, Ljava/lang/StringBuilder;

    invoke-direct {v4}, Ljava/lang/StringBuilder;-><init>()V

    const-string v5, "str0\u4e0estr1\u662f\u5426\u76f8\u7b49\uff1a"

    invoke-virtual {v4, v5}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v4

    invoke-virtual {v1, v2}, Ljava/lang/String;->equals(Ljava/lang/Object;)Z

    move-result v5

    invoke-virtual {v4, v5}, Ljava/lang/StringBuilder;->append(Z)Ljava/lang/StringBuilder;

    move-result-object v4

    invoke-virtual {v4}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;

    move-result-object v4

    invoke-virtual {v3, v4}, Ljava/io/PrintStream;->println(Ljava/lang/String;)V

    .line 13
    return-void

    .end local v1    # "str01":Ljava/lang/String;
    .end local v2    # "str02":Ljava/lang/String;
    :cond_31
    move v3, v4  #v4赋值给v3

    .line 9
    goto :goto_8   #跳到goto_8处继续执行

    .line 10
    .restart local v1    # "str01":Ljava/lang/String;
    :cond_33
    const-wide/high16 v4, 0x403e000000000000L    # 30.0  给寄存器v4赋30.0

    goto :goto_10    #跳到goto_10处继续执行
.end method


```

