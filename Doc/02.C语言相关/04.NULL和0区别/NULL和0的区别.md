# NULL和0区别

在C语言中，我们有时候看到NULL，0，'\0',"0","\0"区别？

## 本质

本质来说，NULL，0，'\0'都是一样的，都是值0。是的，你没有听错。说到这本文差不多应该结束了。不过为了不被打，还是继续说一说。它们虽然值都是0，但是含义却是不一样的。

## NULL

虽然值是0，但是它的含义不一样，或者说它的类型不一样。NULL是指针类型，不过它是空指针，即值为0。


```
#include<stdio.h>
int main(void)
{
    int a = NULL;
    printf("%p\n",a);
    return 0;
}
```

我们编译它：


```
$ gcc -o null null.c
null.c: In function ‘main’:
null.c:14:10: warning: initialization makes integer from pointer without a cast [-Wint-conversion]
  int a = NULL;
          ^
```

它给了我们一个警告，提示尝试将指针转换为整数。这也就正验证了我们前面的说法。

实际上NULL通常是如下定义：


```
#define NULL (void*)0
```

所以，如果要给一个指针类型初始化，那么你给它一个NULL，使得能够明显的看到这是一个指正。当然，在C++中，你更应该使用nullptr，而不是NULL。

## '\0'

我们都知道\是转义符，用单引号包起来，再加转义，实际上就是0，只不过它表示的是字符。就向下面这样：


```
#include<stdio.h>
int main(void)
{
    char a = '\0';
    char b = '0';
    printf("a = %d,b = %d\n",a,b);
    return 0;
}
```

编译运行：


```
$ gcc -o nul nul.c
./nul
a = 0,b = 48
```

我们最常见到的就是它作为字符串的结束符。所以我们常常会看到下面这样的写法：


```
char str[16];
/*do something*/
str[15] = '\0';
```

还记得printf是如何打印字符串，以及strcmp比较停止规则吗？是的，它们都以遇到'\0'结束。

注意，它和'0'完全不一样。通过打印就可以看到了，实际上'\0'的值就是0。

需要特别注意的是，如果'\0'的0后面跟八进制的数，则会被转义。所以'\60'与'0'的值一致。

## 0

这个不用多解释。


```
int a = 0;
```

## "0"

用双引号包裹的0是字符串，我们看不到的是它结尾还有一个’\0‘


```
#include<stdio.h>
int main(void)
{
    char str[] = "0";
    printf("sizeof str is %d,string len is %d\n",sizeof(str),strlen(str));
    return 0;
}
```

运行结果：


```
sizeof str is 2,string len is 1
```

## "\0"

这也是字符串，只不过是两个空字符。使用strlen计算字符串长度为0。

## " "

字符串。字符串长度为1，占用空间2字节，是一个空格加空字符。

## 总结

到这里你应该明白了，它们的值可能一样，但赋予的含义却不一样，为了代码良好的可读性，你应该在恰当的时候使用合适的值。

