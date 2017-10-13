我们先来看看**Brainfuck的定义：**

Müller的目标是创建一种简单的、可以用最小的[编译器](https://zh.wikipedia.org/wiki/编译器)来实现的、匹配[图灵完全](https://zh.wikipedia.org/wiki/图灵完全)思想的编程语言。这种语言由八种[运算符](https://zh.wikipedia.org/w/index.php?title=运算符&action=edit&redlink=1)构成，为[Amiga](https://zh.wikipedia.org/wiki/Amiga)机器编写的[编译器（第二版）](https://web.archive.org/web/20070313151559/http://wuarchive.wustl.edu/pub/aminet/dev/lang/brainfuck-2.lha)只有240个[字节](https://zh.wikipedia.org/wiki/字节)大小。

就象它的名字所暗示的，brainfuck程序很难读懂。尽管如此，brainfuck[图灵机](https://zh.wikipedia.org/wiki/图灵机)一样可以完成任何计算任务。虽然brainfuck的计算方式如此与众不同，但它确实能够正确运行。

|  |
| :--- |


| 字符 | 含义 |
| :--- | :--- |
| `>` | 指针加一 |
| `<` | 指针减一 |
| `+` | 指针指向的字节的值加一 |
| `-` | 指针指向的字节的值减一 |
| `.` | 输出指针指向的单元内容（ASCII码） |
| `,` | 输入内容到指针指向的单元（ASCII码） |
| `[` | 如果指针指向的单元值为零，向后跳转到对应的`]`指令的次一指令处 |
| `]` | 如果指针指向的单元值不为零，向前跳转到对应的`[`指令的次一指令处 |

这种语言基于一个简单的机器模型，除了指令，这个机器还包括：一个以字节为单位、被初始化为零的[数组](https://baike.baidu.com/item/数组)、一个指向该数组的[指针](https://baike.baidu.com/item/指针)

（初始时指向数组的第一个字节）、以及用于输入输出的两个字节流。

首先我们要有一个以字节为单位的数组：

```java
byte[] memory = new byte[10000];
```

这个就是通常计算机的内存，现在假设这个内存有10000长度那么大

然后我们需要一个指针：

```java
int point = 0;
```

初始化为0，也就是指向内存的第一个地址

这个语言的源代码只有8个字符，我们假定源码为：

```java
String source;
```

现在我们要读取源码，我们需要一个辅助的属性index来标示我们读到了代码的第几个字符：

```java
int index
```

然后我们来处理这些指令：

```java
switch (b) {
  case '>':
    point++;
    break;
  case '<';
    point--;
    break;
  case '+';
    memory[point]++;
    break;
  case '-';
    memory[point]--;
    break;
  case '.'
    System.out.print(memory[point]);
    break;
  case ',';
    memory[point] = (byte) System.in.read();
    break;
}
```

其实前面6个指令都很简单，关键在于后面2个指令，这个两个指令实际上就是才是逻辑和循环，这其中的核心就是跳转，就是把源代码的指针index的值进行改变，而这个改变是有条件的，下面就是我对这个逻辑的实现：

```java
switch (b) {
  case '[':
    if (memory[point] == 0) {
      while (source.charAt(index) != ']') {
        index++;
      }
    }
    break;
  case ']';
    if (memory[point] != 0) {
      while (source.charAt(index) != '[') {
        index--;
      }
    }
}
```

其实就是用while循环来检查代码，然后改变index的值，乍一看好像没毛病，是的，如果是下面这个输出hello, world!的代码

```
+++++ +++++             initialize counter (cell #0) to 10
[                       use loop to set the next four cells to 70/100/30/10
    > +++++ ++              add  7 to cell #1
    > +++++ +++++           add 10 to cell #2
    > +++                   add  3 to cell #3
    > +                     add  1 to cell #4
    <<<< -                  decrement counter (cell #0)
]
> ++ .                  print 'H'
> + .                   print 'e'
+++++ ++ .              print 'l'
.                       print 'l'
+++ .                   print 'o'
> ++ .                  print ' '
<< +++++ +++++ +++++ .  print 'W'
> .                     print 'o'
+++ .                   print 'r'
----- - .               print 'l'
----- --- .             print 'd'
> + .                   print '!'
> .                     print '\n'
```

确实没有毛病，但是对于下面这个乘法

```
 ,>,,>++++++++[<------<------>>-]
 <<[>[>+>+<<-]>>[<<+>>-]<<<-]
 >>>++++++[<++++++++>-],<.>.
```

就有问题了，因为这里面有个多重循环，如何界定“\[”对应的“\]”就不准确了，这才是整个程序的难点。

我们先来看看，如何理解对应这个意思，我的理解为：**如果字符\[\]中间有n个\[和n个\]字符，那么他们是一对，n&gt;=0**

按照这个理论，我们开始写代码

先定义一个计数器，出现\[就加1，出现\]就减1，这和上面那个理解是一个意思，只不过简化为一个变量表示

```java
int count = 0;
```

然后在定义一个index移动的偏移量

```java
int offset = 0;
```

然后开始跳转

```java
// 起始偏移量为1，一直到代码结束
for (int offset = 1; index + offset < source.length(); offset++) {
  // 找到跳转字符]且n相等，也就是count=0，计算index的值并跳出循环
  if (source.charAt(index + offset) == ']' && count == 0) {
    index += offset;
    break;
  }
  if (source.charAt(index + offset) == '[') count++;
  if (source.charAt(index + offset) == ']' && count != 0) count--;
}
```

下面是完整的代码：

```java
package com.github.xiongdi.study;

public class BrainfuckInterpreter {

    private byte[] memory = new byte[1000];

    private int point = 0;

    public void run(String source) throws Exception {
        for (int index = 0; index < source.length(); index++) {
            switch (source.charAt(index)) {
                case '>':
                    point++;
                    break;
                case '<':
                    point--;
                    break;
                case '+':
                    memory[point]++;
                    break;
                case '-':
                    memory[point]--;
                    break;
                case '.':
                    System.out.print((char) memory[point]);
                    break;
                case ',':
                    memory[point] = (byte) System.in.read();
                    break;
                case '[':
                    if (memory[point] == 0) {
                        int count = 0;

                        for (int offset = 1; index + offset < source.length(); offset++) {
                            if (source.charAt(index + offset) == ']' && count == 0) {
                                index += offset;
                                break;
                            }
                            if (source.charAt(index + offset) == '[') count++;
                            if (source.charAt(index + offset) == ']' && count != 0) count--;
                        }
                    }
                    break;
                case ']':
                    if (memory[point] != 0) {
                        int count = 0;

                        for (int offset = 1; index - offset >= 0; offset++) {
                            if (source.charAt(index - offset) == '[' && count == 0) {
                                index -= offset;
                                break;
                            }
                            if (source.charAt(index - offset) == ']') count++;
                            if (source.charAt(index - offset) == '[' && count != 0) count--;
                        }
                    }
                    break;
            }
        }
    }
}
```



