# ARTS

ARTS which is 

- Algorithm
- Review
- Tips
- Share

Do the above things every week

# Punch record

- [1st week](#1)
- [2st week](#2)
- [3st week](#3)
- [4st week](#4)
- [5st week](#5)
- [6st week](#6)
- [7st week](#7)


这个目录是用来跳转到第N周的打卡记录(得先展开才能跳转)，防止以后内容多了浏览起来麻烦。

&nbsp;
&nbsp;
&nbsp;
&nbsp;


<details>
	<summary>1st week(点击即可展开)</summary>
	
### <span id="1">Algorithm</span>

```
1. Two Sum(Easy)

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

solution:
```golang
# 两次遍历，暴力破解，时间复杂度为 O(n^2)
func twoSum(nums []int, target int) []int {
    for i:=0;i<len(nums);i++{
        for j:=0;j<len(nums);j++{
            if i!=j&&nums[i]+nums[j]==target{
                return []int{i,j}
            }
        }
    }
    return []int{}
}
```



### review

最近在学MIT的 [6.828](https://pdos.csail.mit.edu/6.828/2018/schedule.html),一门教你怎么动手设计与实现一个操作系统内核的课程。在学习的过程中有个作业是熟悉汇编语言，所以我打算翻译一下这本书的中的汇编语言部分的1.3一小节。
[PC Assembly Language Book](https://pdos.csail.mit.edu/6.828/2018/readings/pcasm-book.pdf)

<details>
  <summary>(点击即可展开翻译的内容)</summary>

## 1.3 汇编语言
### 1.3.1机器语言
每一种类型的CPU都只能明白它独自的机器语言，机器语言中的指令是以字节的形式存储在内存中的数据，每个指令都拥有着它唯一的数字码，也被称为操作码(operation code),许多指令同时还包含着数据。

机器语言远没有高级语言那么简单易读，比如把EAX和EBX寄存器加起来的值赋给EAX可以写为：

    03 C3
这确实难以读懂，所幸的是一个程序可以调用汇编器代替人类完成这些沉闷的工作

### 1.3.2 汇编语言
一个汇编程序将会以文本的形式存储起来，每个汇编指令都精确地代表着一个机器指令。比如用汇编描述上面的机器码可以写为：

    add eax,ebx
这么看起来确实比机器语言简单易读多了，add这个词是加法指令的助记符。汇编指令的通用格式为：

    助记符(指令)   操作数(运算对象)
一个汇编器就是一个阅读带有汇编指令的文本文件并将其转换为机器指令的程序。而编译器就是为高级语言做类似转换的程序，一个汇编器会比编译器要简单得多，每一条汇编声明都直接表示着一个机器指令，高级语言则更为复杂且需要更多的机器指令表示。

  高级语言和汇编语言之间还有一个很重要的差异，每一种CPU除了有自己的机器语言外还有自己才能读懂的汇编语言，所以通过汇编语言在不同机器架构之间移植程序要比高级语言复杂得多。
  
  本书的例子使用Netwide的汇编，简称为 NASM，它可以在互联网上免费获取。更通用的汇编是微软的汇编 (MASM)或者 Borland的汇编 (TASM), MASM/TASM 与 NASM的汇编语言在语法上有些差异
  
  ### 1.3.3 运算对象
  不同机器码的运算对象在数量与类型上都有所不同，每个指令本身都有一个固定的运算对象(0到3)，操作数可以拥有以下类型：
  
- 寄存器(register):这些操作数可以直接调用ＣＰＵ寄存器的内容
- immediate(直译过来好怪):这些是指令自身固定的值
- implied:这些操作数没有直接的显示出来，比如使用增加指令把一个数加到寄存器或内存中，那个数就是隐式的
- 
### 1.3.4 基本指令
最基本的指令是 MOV指令，它将数据从一个地方移动另一个地方(就像高级语言的赋值操作符)。例如：

    mov dest,src
这个指令将 src中的数据复制到dest中，需要注意的是两个运算对象都不能为内存运算对象，((直译过来的，我也没明白什么是内存运算对象)。而且在使用各种指令的时候需要注意一些专有的规则，比如两个运算对象大小必须相等。例子：
    
    mov eax,3; 将3存入EAX寄存器中(3是immediate 操作数)
    mov bx,ax; 将AX的值存入BX寄存器中

ADD指令通常用于整数的相加

    add eax,4; eax = eax + 4
    add al,ah; al = al + ah

SUB指令用于整数的相减

    sub bx,10; bx = bx - 10
    sub ebx,edi; ebx = ebx -edi

INC和DEC为自增和自减运算符
    inc ecx ; ecx++
    dec dl ; dl--

### 1.3.5 Directives(指令?)
指令是汇编器的工件而不是CPU的(A directiveis an artifact of the assembler not the CPU)，它们通常用于命令汇编器执行某些任务，他们不需要翻译成机器码，directives通常用于:

- 定义常量
- 申请内存
- 将内存分段
- 按条件选择源码/条件编译？(conditiionally include source code)
- 包含其他文件
- 
NASM汇编像C语言一样拥有预处理器这个机制，而且还有很多预处理器命令。不同之处在于，NASM的预处理命令以%开头，而不是#

**equ 命令**

equ命令用于定义一个符号(symbol)，符号被命名为常量，可以在汇编程序中使用，格式如下：

    sybol equ value ;符号的值不可以再重新定义
    
**%define 命令**

%define 就像c中的#define，被用来定义常量宏，例如：

    %define SIZE 100
    move eax,SIZE
上面这段代码定义了一个值为100的常量宏SIZE，并将SIZE赋值给eax。宏比符号(symbol)更加灵活，可以重新给定义(赋值)，而符号(symbol)不可以

**数据命令(data directives)**

数据命令(data directives)用于为数据段(data segment)申请内存空间，有两种方式可以让内存得以保留，一是单纯地申请一块内存空间，二是申请内存空间并初始化值。第一种方法可以通过RES X命令来实现，X表示对象的大小，下面有一张表格说明了X的大小


|    Unit    | Letter |
|------------|--------|
| byte       | B      |
| word       | W      |
| doble word | D      |
| quad word  | Q      |
| ten bytes           |T        |

第二中方法用DX命令，X代表对象的大小，下面的例子演示了两种方法的不同，通常用一个标签标记内存中的位置，可以通过这个标签来引用这块内存

    L1    db     0        ; byte labeled L1 with initial value 0
    L2    dw     1000     ; word labeled L2 with initial value 1000
    L3    db     110101b  ; byte initialized to binary 110101 (53 in decimal)
    L4    db     12h      ; byte initialized to hex 12 (18 in decimal)
    L5    db     17o      ; byte initialized to octal 17 (15 in decimal)
    L6    dd     1A92h    ; double word initialized to hex 1A92
    L7    resb   1        ; 1 uninitialized byte
    L8    db     "A"      ; byte initialized to ASCII code for A (65)
    
    单引号和双引号效果都一样，所定义的数据会被存储在连续的内存中，如L2会紧跟着L1的位置

    L9    db     0, 1, 2, 3              ; defines 4 bytes   
    L10   db     "w", "o", "r", ’d’, 0   ; defines a C string = "word"  
    L11   db     ’word’, 0               ; same as L10

    当定义一个很长的序列的时候，NASM的times就可以排上用场了，这个命令以指定的次数重复一个操作数
    L12   times 100 db 0                 ; equivalent to 100 (db 0)’s
    L13   resw   100                     ; reserves room for 100 words

代码中的标签可以用来引用数据，有两种使用标签的方式。如果只是单纯一个标签，它会被当成一个数据的地址(或偏移)。如果标签被放在一个方括号("[]")里，他会被当成那块地址上的数据。换句话说，第一种用法就像C中的指针，标签表示一个内存地址。第二种就像C中的反引用操作符(* pointer)，以指针反引变量的值(MASM/TASM的语法则稍有些不同)。注意在32位模式下，地址也是32位的，标签使用例子如下：

    mov    al, [L1]      ; copy byte at L1 into AL2
    mov    eax, L1       ; EAX = address of byte at L1
    mov    [L1], ah      ; copy AH into byte at L1
    mov    eax, [L6]     ; copy double word at L6 into EAX
    add    eax, [L6]     ; EAX = EAX + double word at L6
    add    [L6], eax     ; double word at L6 += EAX
    mov    al, [L6]      ; copy first byte of double word at L6 into AL
    
例子的最后一行演示了NASM汇编的一个重要特性，汇编器并不知道它所引用的那个数据的类型，能否正确地使用标签取决于程序员的水平。稍后你会看到很多例子，将数据的地址存入寄存器中，并且就像使用C的指针变量一样使用寄存器。同样地，汇编器也不会检查指针是否正确地被使用，所以汇编语言比C更容易出错。

思考下列指令：

    mov    [L6], 1             ; store a 1 at L6
这条声明出现了 "未指定操作大小的错误"，为什么呢？因为汇编器不知道把要1当成byte、
word、还是doble word类型，所以应该改为这样：

    mov    dword [L6], 1             ; store a 1 at L6
    
等于告诉了汇编器将1当成 doble word类型，或者你改为其他类型也可以：WORD、BYTE。

### 1.3.6 输入输出

输入输出是非常依赖于系统的活动，它调用了系统硬件的接口。比如像C这类高级语言，提供了为执行日常任务的标准库，标准库中拥有着简单、统一的I/O编程接口。而汇编语言只提供非标准库，它们可以直接访问硬件(保护模式下的特权操作)，或使用操作系统系统底层调用。

汇编语言与C语言的组合的情况很常见，有一个好处就是汇编可以使用C标准库中的I/O接口，但得清楚它们之间调用的规则，而且规则很复杂，暂时就不介绍了(稍后有介绍)。为了简化这些规则，作者封装了底层的细节，提供了一系列简单的接口。如下：

    print_int 打印存储在EAX中的整型值
    print_char 打印存储在AL中的ASCII值
    print_string 打印存储在EAX中的字符串内容
    print_nl 打印换行符
    read_int 从键盘读入输入，将其存入EAX寄存器中
    read_char 从键盘读取输出，将其ASCII码存如EAX寄存器中

除了读接口，其他接口都保留了寄存器的值，这些接口修改寄存器的值。为了能使用这些接口，得先用预处理命令声明，格式为: %include ，调用作者的接口则使用以下命令:

    %include "asm_io.inc"
如要使用打印接口，先载入包含正确值的EAX，然后使用CALL指令调用它。CALL命令就相当于高级语言的函数调用，它会跳转到函数调用的地方，执行完毕后再返回。下面将有几个例子演示I/O接口的使用：

### 1.3.7 调试

作者的库中还包含了一些调试程序，这些接口显示了计算机的状态，接口实际上是保存CPU当前状态的宏，然后开启一个子进程(subroutine)调用。这些宏定义在"asm_io.in"文件中，宏可以像使用普通命令一样被使用，宏中的运算对象以分号隔开。

这里有四个调试接口，dumpregs ,dumpmem, dumpstack, dumpmath，分别显示寄存器、内存、栈(stack)、数字协处理器(math coprocessor)的值.

- dump_regs:这个宏将计算机寄存器的值打印到标准输出中(十六进制格式)，同时也显示FLAGS寄存器中的位集合(bits set),比如，0 FLAGS为1，则显示ZF，如果为0，则不显示(f the zero flag is 1,ZFis displayed. If it is 0, it is not displayed),同时它还打印一个整型参数,这个可以用来区别不同的dumpregs命令
- dump_mem：这个宏打印内存中的值(十六进制)，同时作为ASCII字符。它们是由分号分隔的3个参数，第一个是标记输出的整数(仅作为dumpregs的参数)，第二个参数是一个地址，最后一个参数是在地址后显示的16字节段落(paragraph)的数量,显示的内存会在请求地址的第一个段落边界处开始。
- dump_stack:这个宏会输出CPU栈中的值(栈会在第四章中提到),栈被组织为doble word类型，接口也会以这种类型显示它，它也是由分号分隔的3个参数，第一个参数是整型的标签(就像 dump_regs),第二个是地址之下EBP寄存器拥有的double word个数，第三个是EBP地址之上的double word个数(the second is the number of double words to displaybelowthe addressthat theEBPregister holds and the third argument is the number ofdouble words to displayabovethe address inEBP)
- dump_math:这个宏会输出数字协处理(math coprocessor)寄存器中的值，它用单个参数标记输出，就像dump_regs所做的一样。

    


</details>

**感想**

这应该算是我第一次真正意义上的翻译技术文档，我发现翻译文档是件苦活，而且我的英语水平很一般连四级都没过(虽然我觉得我已经超过了这个水平)，翻译起来就更不容易了，但为了提高英语阅读能力，还是咬牙下决心把它翻译出来。在翻译过程中所遇到的一些词和句子直译起来很奇怪，这种情况我就把原文粘贴下来放在括号里，还有些翻译过来太啰嗦了，会使读者混淆其义，我就做一些适当的更改，使其读起来更流畅些。虽然只是翻译一小节，但却整整花了我一天的时间，同时收获也不小，发现自己慢慢地适应了这个过程。

我之所以翻译这个文档，一是觉得自己的英语阅读水平很一般，而这种水平的阅读能力在阅读英语原文时，有种似懂非懂的云里雾里的感觉，这会使我在阅读时漏掉某些重要的概念，很多概念往往隐含在简单的行里字间的描述，而如果把它翻译过来就会有种"踏实"的感觉，因为翻译的过程首先是你先清楚每个单词的意思，然后按照感觉把这些单词的顺序进行整理，组合成一个通顺流畅的句子。(好吧，我承认在面对一些复杂的从句的时候，我的语法水平已经帮不上忙了，愧对老师)有时候为了使句子更符合英文的原意，不能直译，还得适当的进行添加或删除某些部分。这个过程一开始很痛苦，但慢慢地适应后收获还是不错的。

### Tips

分享一个有意思的markdown语法技巧，比如有时候我们在写作的时候，为了提供一个简洁的大纲，希望把某些细节隐藏起来，方便读者阅读(其实我上面就有用到这个技巧，那个三角号，点击即可展开内容)，下面做一个演示：

**BASH语法**

<details>
	<summary>变量声明</summary>

```bash
NAME="John"
echo $NAME
echo "$NAME"
echo "${NAME}!"
```
</details>


<details>
	<summary>条件执行</summary>

```bash
git commit && git push
git commit || echo "Commit failed"
```
</details>


<details>
	<summary>函数声明与使用</summary>

```bash	
get_name() {
  echo "John"
}
echo "You are $(get_name)"
```

</details>

嘻嘻，是不是很有意思，点一下就展开，再点一下就收缩了。其实它是通过两个标签实现的，格式如下 

	<details>
		<summary>这里写概要</summary>

		这里写你想收缩起来的内容(注意，最好空一行，否则有些markdown语法会解释失败)
	</details>
好了，就这么简单。


### Share

在学习汇编的过程中，遇到两种风格的汇编，NASM和GNU的汇编，前者使用Intel语法，后者使用AT&T语法。它们之间的语法有些不同(在我看来是很大的不同),这里有篇[文章](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html)简单的介绍它们之间的不同

比如 命名寄存器：

	AT&T:  %eax
	Intel: eax

寄存器的顺序(AT&T和Intel顺序相反)：

	AT&T: movl %eax ,%ebx
	Intel: mov ebx ,eax

常量/immedite 值，AT&T多了个前缀"%":

	AT&T: movl %_booga ,%eax
	Intel: mov  eax,_booga


虽然作者说他是AT&T汇编的粉丝，还声称AT&T语法比较有逻辑性，但我怎么看都觉得AT&T语法有些反人类，多了很多没有必要的东西，远不如Intel语法简洁，或许是我刚接触汇编的原因把。	
</details>

	
<details>
	<summary>2st week</summary>

### <span id="2">Algorithm</span>

<details>
	<summary>题目</summary>

```
70. Climbing Stairs(Easy)
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:

Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

</details>

<details>
	<summary>解法</summary>

**思路**

爬楼梯，每次可以上一阶或两阶，当有n阶楼梯时，那么走到第n阶的方法数等于第n-1和第n-2的方法书之和，这其实就是一个fibonacci数列:

f(n) = f(n-1)+f(n-2)

**代码**
```golang
func climbStairs(n int) int {
    if n<=2{
        return n
    }
    a,b:=1,2
    
    for i:=0;i<n-2;i++{
        a,b=b,a+b
    }
    return b
    
}
```
</details>

### Review

这周忙于找工作，和赶毕业论文，弄得焦头烂额。本想拖到周日再写，但是周日还要面试，趁今晚有点空闲时间就写了，本周的review打算翻译与linux 系统日志相关的文章。

我们在学习linux过程中都某些分水岭，比如刚开始基础linux的时候只会简单的命令，如 ls、cd、ping 等简单命令，后来慢慢开始了解如何关闭一个进程，如何创建一个用户，如何更改文件的权限等。但我们肯定不会只满足于此，而如果想向下一个阶段迈进，达到能独立编写shell脚本，处理一些日常任务，就需要系统地接触linux中各方面的知识。分享一个系统地学习linux基础的网站,[点击即可](https://linuxjourney.com/)。

这个网站从最基本的文件系统操作，到简单的字符串处理，再到内核相关的知识都有涵盖，而且每一个重要的知识点都专门分一个主题来介绍，这周打算翻译系统日志部分

<details>
	<summary>点击查看翻译内容</summary>

### 1. 系统日志

你系统中的后台服务、内核、守护进程无时不刻都在运行，自然也会产生一些数据以日志的形式保存在系统中，它以人类可读的日记方式记载着操作系统中发生的事件，而这些数据通常保存在 /var目录下，/var目录保存着可变化的数据，比如日志。

这些日志信息是如何从系统中获取的呢？它是由一个叫 "syslog"的服务把这些信息发送给系统日志器(system logger)

syslog通常包含许多组件，其中一个重要的组件是"syslogd"守护进程(更新的linux发行版中使用rsyslogd)，它等待系统产生的事件消息，并过滤出它想要的，然后将其以文件的格式存起来或发送给终端，也可能会将它丢弃。

你可能会觉得这个系统日志器会统一存放并管理日志，但恰恰相反，你会看到许多应用都有自身的日志规则并生成不同的日志文件，不过通用格式的日志都会包含时间戳(timestamp)和日志细节

这里从日志中摘抄一行作为例子：
```

pete@icebox:~$ less /var/log/syslog

Jan 27 07:41:32 icebox anacron[4650]: Job `cron.weekly' started
```

可以看到cron服务在1月27 07:41:32运行cron.weekly，当然你也可以查看收集在/var/log/syslog中的所有日志信息

### 2. syslog服务

syslog服务管理日志和将日志发送到系统日志器(system logger)中，Rsyslog则是syslog的升级版，大多数linux发行版应该都会使用这个升级版，所有syslog所收集的日志都可以在/var/log/syslog中找到(除了认证信息)。

为了找出系统日志器(system logger)所维护的文件，得查找/etc/rsyslog.d中配置文件

```
pete@icebox:~$ less /etc/rsyslog.d/50-default.conf 

# First some standard log files.  Log by facility.

#

auth,authpriv.*                 /var/log/auth.log

*.*;auth,authpriv.none          -/var/log/syslog

#cron.*                         /var/log/cron.log

#daemon.*                       -/var/log/daemon.log

kern.*                          -/var/log/kern.log

#lpr.*                          -/var/log/lpr.log

mail.*                          -/var/log/mail.log

#user.*                         -/var/log/user.log
```

文件中的日志规则代表，左边的选择器将日志输出到到右边的文件中。有一点需要注意，并不是所有的应用或服务都是用rsyslog管理它们的日志，所以如果你想知道他们具体使用什么管理日志，得在这个目录里找

让我们来看一下日志是如何运作的，你可以手动的用日志器命令发送一条日志：

```
logger -s Hello
```
好了，你可以查看 /var/log/syslog文件，你会发现多了一条日志。

### 3. 通用日志(general logging)

系统中包含了许多的日志文件，很多重要的日志都放在/sys/log目录下，但我们并不会分析所有这些日志文件，我们选择几个主要的来讨论。

有两种通用的日志文件可以查看系统正在做的事

**/var/log/messages**
这个日志包含了非紧急(non-critical)、非调试(not-debug)消息，包括启动(bootup)、验证(auth)、计划任务(cron)、守护进程(daemon)中所输出的日志,可以帮助你查看你的机器是如何运作的

**/var/log/syslog**
这个文件包含了除验证(auth)外的所有消息，对于在你机器上的错误调试非常有用。

这两个日志文件对于追查错误来说，已经足够了。但是，如果您只想查看特定的日志组件，那么也会有单独的日志

### 4. 内核日志

**/var/log/dmesg**

在启动时，系统会记录内核缓冲区中的信息，它展示了系统在启动时硬件驱动、内核和内核状态的一些信息。你可以在/var/log/dmesg 文件中找到这些信息，这些信息每次在启动的时候都会被刷新。或许你现在暂时用不上，但当你遇到某些与启动相关或硬件驱动的问题时，这个日志就派上用场了，你甚至可以用dmesg 命令查看这些信息。

**/var/log/kern.log**

另一你可以查看内核信息的文件是/var/log/kern.log ，它记录了系统中的事件与内核信息，也包括dmesg的输出。


### 5. 验证日志(Authentication Logging)

当你登录出现问题时，身份验证日志可以帮到你。

**/var/log/auth.log** 这个文件包含了系统授权日志，如用户登录和使用的认证方法。

样例：

	Jan 31 10:37:50 icebox pkexec: pam_unix(polkit-1:session): session opened for user root by (uid=1000)



### 6. 管理日志文件

日志文件生成了许多数据，这些数据被存储在磁盘中，同时也带来了许多问题。大多数情况下，我们只想查看更新的日志，同时更高效地管理磁盘空间。如何做到这点呢?答案是： logrotate

logrotate 工具为我们提供了日志管理，它有一个配置文件允许我们指定数目或指定哪些日志需要保存，也可以压缩日志节省存储空间。logrotate工具作为计划任务(cron)每天运行一次，配置文件可在/etc/logrotate.d.中找到。

虽然还有其他管理日志的工具，但logrotate是最常用的一个。


</details>

### Tips

分享一个有意思的技巧，系统中(包含Window和类UNIX)的hosts文件。

我们都知道当在浏览器输入域名的时候，浏览器会调用系统的接口向DNS查询对应的IP，但其实在DNS查询之前，它是先查找hosts文件中域名映射的IP，也就是说hosts文件的优先级是比DNS查询要高的。

那么这有什么用呢？你想啊，如果我把百度的域名 www.baidu.com 映射到0.0.0.0，那就意味着你每次访问百度都会映射到一个无效的IP，自然就上不了网了。

你可以用来搞怪别人，写一个脚本把一些主流的域名都映射到无效的IP，然后他会发现怎么突然上不了网了，但是我建议你在干这个之前先买好保险。

你甚至可以用来屏蔽一些广告，把一些广告的域名映射到一个无效的IP中，这样每次都加载失败自然就等于把广告屏蔽了。

下面以Ubuntu为例，以管理员权限权限打开/etc/hosts文件(因为文件权限只允许管理员写入)：

	vim /etc/hosts

然后你会看到类型的内容:

```
127.0.0.1	localhost
127.0.1.1	jack-VirtualBox

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

然后在第三行下添加:

	0.0.0.0   www.baidu.com

用vim保存退出的时候记得带上！符号，即: wq!  (因为文件权限只允许管理员写入)

好了，然后你上一下百度，发现上不了，^_^.

### Share

本周的Share打算分享一位网友的博客，他把Leetcode上大部分题都刷完了，而且很多题还给了几种解法并详细地分析思路，然后做一个汇总。我其实真的很佩服这些人，上千道算法题，即使每天刷一道，而且得连续不断地刷，这样也要两年多的时间。这还没完，还把解过的题都整理起来，附上解题思路，这个过程想想都觉得十分不容易。

我特别的佩服这些人的毅力，他们一直都是我学习的榜样，而且我也打算要这么干，md,想想都刺激。

好了，链接在这，[点就好](https://www.cnblogs.com/grandyang/p/4606334.html)
</details>

<details>
	
<summary>3st week</summary>

### <span id="3">Algorithm</span>

<details>

<summary>题目</summary>

```
Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example 1:

Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
Example 2:

Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```
</details>

<details>

<summary>解法</summary>

要求in-place操作移除给定数组上的指定元素，很明显用双指针的思想，遍历一遍即可。用一个"指针"记录非val元素的索引，一个"指针"遍历整个数组，如果遇到非val元素，两个"指针"交换位置即可，时间复杂度O(n)

```golang
func removeElement(nums []int, val int) int {
    if len(nums)<1{
        return 0
    }
    index:=-1
    
    for i:=0;i<len(nums);i++{
        if nums[i]!=val{
            index++
            nums[i],nums[index]=nums[index],nums[i]
        }
        
    }
    return index+1
    
}
```

</details>

### Review

这周给找工作的事弄得有些迷茫了，面了两家结果都不太理想，算了车到山前必有路。

本周翻译与系统初始化相关的一部分内容

<details>
	
<summary>翻译内容(点击即可展开)</summary>

英语还没过四级，翻译水平有限，轻喷。

### System V 概述

init主要作用是用来启动或关闭系统中主要的进程，在 Linux中主要有三种init的实现，分别是System V, Upstart 和 systemd，在本节中将介绍最传统的init版本，也叫System V init或Sys V (发'System Five'的音)

鉴别你系统中是否使用System V init很简单，如果你的系统中包含 /etc/initab 文件，则说明很有可能就是使用该版本的init。

Sys V 按顺序地启动或关闭进程，比如你想启动foot-a 和foot-b进程，必须确保foot-a已经运行了才能执行foot-b。Sys V是以脚本的形式执行这些操作，你可以编写自己的脚本或使用系统内置的。(直译过来感觉有些啰嗦，就删了部分内容)

使用init的这种实现的优点是，解决依赖关系相对容易，但一次只能启动或关闭一个进程，执行foo-b前得先执行foo-a，所以性能并不理想。

当运行Sys V时，机器的状态由运行级别定义，运行级别被设定为0-6。这些模式在不同的发行版上有些不同，但大致如下:

	0: 关闭(Shutdown)
	1: (单一用户模式)Single User Mode
	2: (无网络多用户模式)Multiuser mode without networking
	3: (有网络多用户模式)Multiuser mode with networking
	4: (未使用)Unused
	5: (带图形界面有网络的多用户模式)Multiuser mode with networking and GUI
	6: (重启)Reboot

当你的系统启动时，它会查看你所运行的级别，并执行位于该运行级别配置中的脚本。这些脚本位于 /etc/rc.d/rc[运行级别数字].d/ or /etc/init.d下，以S(start)和K(kill)开头的脚本名则代表启动和关闭，字符旁的数字代表运行在其中的序列，比如:

	pete@icebox:/etc/rc.d/rc0.d$ ls

	K10updates  K80openvpn        

表明当运行级别切换到0时，机器将尝试运行脚本终止 updates和openvpn服务。你可以在/etc/inital文件中查看机器启动时的运行级别，当然也可以改变默认的运行级别。

有一点需要注意，Sys V正在缓慢地被取代，或许不是今天或几年后。但是即便被取代，你也会看到运行级别这个概念出现在其他init的实现中，这主要是为了支持那些仅使用System V init脚本启动或停止的服务

### System V 服务

你可以使用许多命令行工具来管理 System V 服务

列出所有服务

	service --status-all

开启一个服务
	
	sudo service  networking start

停止一个服务

	sudo service networking stop

重启一个服务

	sudo service networking restart

这些命令并非只针对System V服务有效，同时也可以用来管理Upstart 服务。因为linux正在尝试从传统的Sys V init 脚本中迁移，所以还得需要一些工具帮助它过渡。

### Upstart 概述

Upstart按标准(canonical)开发，所有曾经有一段时间是ubuntu上的init实现。但是现代化的Ubuntu init实现时systemd。

Upstart被创造来改善了Sys V中的某些问题，比如严格的启动过程、阻塞任务等。Upstart的事件和工作驱动模型(job driven model)允许它在事件发生时对事件做出响应

鉴别你系统中是否使用Upstart很简单，只要看是否存在/usr/share/upstart目录即可

jobs是upstart 执行的操作，事件(events)是从其他进程接收的消息用于触发作业(jobs)，下面来看一下jobs列表与它的配置

```

pete@icebox:~$ ls /etc/init

acpid.conf                   mountnfs.sh.conf

alsa-restore.conf            mtab.sh.conf

alsa-state.conf              networking.conf

alsa-store.conf              network-interface.conf

anacron.conf                 network-interface-container.conf
```

在job的配置里，包含了如何开始和什么时候开启job的信息

比如在networking.conf文件中，可以简单的描述为

```

start on runlevel [235]

stop on runlevel [0]
```

这意味着在运行级别2、3、5时会开启networking设置，在运行级别为0时关闭它。当查看不同地job配置文件时，会有许多种编写配置文件的方式.


Upstart工作流程

1. 首先从/etc/init文件中加载job配置
2. 一旦启动事件发生，它将运行由事件触发的作业(job)
3. 这些作业(job)会产生新的事件，而新的事件又会触发更多的作业(job)
4. upstart持续地重复这个过程，直到执行完待完成的作业


</details>



### Tips

本周Tips分享如何在Ubuntu下更好软件源的方式，这是以前写的一篇[文章](https://segmentfault.com/a/1190000017244124)。 这几天没在状态，没怎么输入，都在翻老底了。

### Share

本周要分享一段代码，或许与最初陈皓前辈所提的ARTS标准有些偏移，但我觉得很有意思，所以拿出来分享一下。

<details>
<summary>shell版俄罗斯方块代码</summary>

```bash
#!/bin/bash
 
# Tetris Game
# 10.21.2003 xhchen<[email]xhchen@winbond.com.tw[/email]>
 
#APP declaration
APP_NAME="${0##*[\\/]}"
APP_VERSION="1.0"
 
 
#颜色定义
cRed=1
cGreen=2
cYellow=3
cBlue=4
cFuchsia=5
cCyan=6
cWhite=7
colorTable=($cRed $cGreen $cYellow $cBlue $cFuchsia $cCyan $cWhite)
 
#位置和大小
iLeft=3
iTop=2
((iTrayLeft = iLeft + 2))
((iTrayTop = iTop + 1))
((iTrayWidth = 10))
((iTrayHeight = 15))
 
#颜色设置
cBorder=$cGreen
cScore=$cFuchsia
cScoreValue=$cCyan
 
#控制信号
#改游戏使用两个进程，一个用于接收输入，一个用于游戏流程和显示界面;
#当前者接收到上下左右等按键时，通过向后者发送signal的方式通知后者。
sigRotate=25
sigLeft=26
sigRight=27
sigDown=28
sigAllDown=29
sigExit=30
 
#七中不同的方块的定义
#通过旋转，每种方块的显示的样式可能有几种
box0=(0 0 0 1 1 0 1 1)
box1=(0 2 1 2 2 2 3 2 1 0 1 1 1 2 1 3)
box2=(0 0 0 1 1 1 1 2 0 1 1 0 1 1 2 0)
box3=(0 1 0 2 1 0 1 1 0 0 1 0 1 1 2 1)
box4=(0 1 0 2 1 1 2 1 1 0 1 1 1 2 2 2 0 1 1 1 2 0 2 1 0 0 1 0 1 1 1 2)
box5=(0 1 1 1 2 1 2 2 1 0 1 1 1 2 2 0 0 0 0 1 1 1 2 1 0 2 1 0 1 1 1 2)
box6=(0 1 1 1 1 2 2 1 1 0 1 1 1 2 2 1 0 1 1 0 1 1 2 1 0 1 1 0 1 1 1 2)
#所有其中方块的定义都放到box变量中
box=(${box0[@]} ${box1[@]} ${box2[@]} ${box3[@]} ${box4[@]} ${box5[@]} ${box6[@]})
#各种方块旋转后可能的样式数目
countBox=(1 2 2 2 4 4 4)
#各种方块再box数组中的偏移
offsetBox=(0 1 3 5 7 11 15)
 
#每提高一个速度级需要积累的分数
iScoreEachLevel=50        #be greater than 7
 
#运行时数据
sig=0                #接收到的signal
iScore=0        #总分
iLevel=0        #速度级
boxNew=()        #新下落的方块的位置定义
cBoxNew=0        #新下落的方块的颜色
iBoxNewType=0        #新下落的方块的种类
iBoxNewRotate=0        #新下落的方块的旋转角度
boxCur=()        #当前方块的位置定义
cBoxCur=0        #当前方块的颜色
iBoxCurType=0        #当前方块的种类
iBoxCurRotate=0        #当前方块的旋转角度
boxCurX=-1        #当前方块的x坐标位置
boxCurY=-1        #当前方块的y坐标位置
iMap=()                #背景方块图表
 
#初始化所有背景方块为-1, 表示没有方块
for ((i = 0; i < iTrayHeight * iTrayWidth; i++)); do iMap[$i]=-1; done
 
 
#接收输入的进程的主函数
function RunAsKeyReceiver()
{
        local pidDisplayer key aKey sig cESC sTTY
 
        pidDisplayer=$1
        aKey=(0 0 0)
 
        cESC=`echo -ne "\033"`
        cSpace=`echo -ne "\040"`
 
        #保存终端属性。在read -s读取终端键时，终端的属性会被暂时改变。
        #如果在read -s时程序被不幸杀掉，可能会导致终端混乱，
        #需要在程序退出时恢复终端属性。
        sTTY=`stty -g`
 
        #捕捉退出信号
        trap "MyExit;" INT TERM
        trap "MyExitNoSub;" $sigExit
 
        #隐藏光标
        echo -ne "\033[?25l"
 
 
        while :
        do
                #读取输入。注-s不回显，-n读到一个字符立即返回
                read -s -n 1 key
 
                aKey[0]=${aKey[1]}
                aKey[1]=${aKey[2]}
                aKey[2]=$key
                sig=0
 
                #判断输入了何种键
                if [[ $key == $cESC && ${aKey[1]} == $cESC ]]
                then
                        #ESC键
                        MyExit
                elif [[ ${aKey[0]} == $cESC && ${aKey[1]} == "[" ]]
                then
                        if [[ $key == "A" ]]; then sig=$sigRotate        #<向上键>
                        elif [[ $key == "B" ]]; then sig=$sigDown        #<向下键>
                        elif [[ $key == "D" ]]; then sig=$sigLeft        #<向左键>
                        elif [[ $key == "C" ]]; then sig=$sigRight        #<向右键>
                        fi
                elif [[ $key == "W" || $key == "w" ]]; then sig=$sigRotate        #W, w
                elif [[ $key == "S" || $key == "s" ]]; then sig=$sigDown        #S, s
                elif [[ $key == "A" || $key == "a" ]]; then sig=$sigLeft        #A, a
                elif [[ $key == "D" || $key == "d" ]]; then sig=$sigRight        #D, d
                elif [[ "[$key]" == "[]" ]]; then sig=$sigAllDown        #空格键
                elif [[ $key == "Q" || $key == "q" ]]                        #Q, q
                then
                        MyExit
                fi
 
                if [[ $sig != 0 ]]
                then
                        #向另一进程发送消息
                        kill -$sig $pidDisplayer
                fi
        done
}
 
#退出前的恢复
function MyExitNoSub()
{
        local y
 
        #恢复终端属性
        stty $sTTY
        ((y = iTop + iTrayHeight + 4))
 
        #显示光标
        echo -e "\033[?25h\033[${y};0H"
        exit
}
 
 
function MyExit()
{
        #通知显示进程需要退出
        kill -$sigExit $pidDisplayer
 
        MyExitNoSub
}
 
 
#处理显示和游戏流程的主函数
function RunAsDisplayer()
{
        local sigThis
        InitDraw
 
        #挂载各种信号的处理函数
        trap "sig=$sigRotate;" $sigRotate
        trap "sig=$sigLeft;" $sigLeft
        trap "sig=$sigRight;" $sigRight
        trap "sig=$sigDown;" $sigDown
        trap "sig=$sigAllDown;" $sigAllDown
        trap "ShowExit;" $sigExit
 
        while :
        do
                #根据当前的速度级iLevel不同，设定相应的循环的次数
                for ((i = 0; i < 21 - iLevel; i++))
                do
                        sleep 0.02
                        sigThis=$sig
                        sig=0
 
                        #根据sig变量判断是否接受到相应的信号
                        if ((sigThis == sigRotate)); then BoxRotate;        #旋转
                        elif ((sigThis == sigLeft)); then BoxLeft;        #左移一列
                        elif ((sigThis == sigRight)); then BoxRight;        #右移一列
                        elif ((sigThis == sigDown)); then BoxDown;        #下落一行
                        elif ((sigThis == sigAllDown)); then BoxAllDown;        #下落到底
                        fi
                done
                #kill -$sigDown $$
                BoxDown        #下落一行
        done
}
 
 
#BoxMove(y, x), 测试是否可以把移动中的方块移到(x, y)的位置, 返回0则可以, 1不可以
function BoxMove()
{
        local j i x y xTest yTest
        yTest=$1
        xTest=$2
        for ((j = 0; j < 8; j += 2))
        do
                ((i = j + 1))
                ((y = ${boxCur[$j]} + yTest))
                ((x = ${boxCur[$i]} + xTest))
                if (( y < 0 || y >= iTrayHeight || x < 0 || x >= iTrayWidth))
                then
                        #撞到墙壁了
                        return 1
                fi
                if ((${iMap[y * iTrayWidth + x]} != -1 ))
                then
                        #撞到其他已经存在的方块了
                        return 1
                fi
        done
        return 0;
}
 
 
#将当前移动中的方块放到背景方块中去,
#并计算新的分数和速度级。(即一次方块落到底部)
function Box2Map()
{
        local j i x y xp yp line
 
        #将当前移动中的方块放到背景方块中去
        for ((j = 0; j < 8; j += 2))
        do
                ((i = j + 1))
                ((y = ${boxCur[$j]} + boxCurY))
                ((x = ${boxCur[$i]} + boxCurX))
                ((i = y * iTrayWidth + x))
                iMap[$i]=$cBoxCur
        done
 
        #消去可被消去的行
        line=0
        for ((j = 0; j < iTrayWidth * iTrayHeight; j += iTrayWidth))
        do
                for ((i = j + iTrayWidth - 1; i >= j; i--))
                do
                        if ((${iMap[$i]} == -1)); then break; fi
                done
                if ((i >= j)); then continue; fi
 
                ((line++))
                for ((i = j - 1; i >= 0; i--))
                do
                        ((x = i + iTrayWidth))
                        iMap[$x]=${iMap[$i]}
                done
                for ((i = 0; i < iTrayWidth; i++))
                do
                        iMap[$i]=-1
                done
        done
 
        if ((line == 0)); then return; fi
 
        #根据消去的行数line计算分数和速度级
        ((x = iLeft + iTrayWidth * 2 + 7))
        ((y = iTop + 11))
        ((iScore += line * 2 - 1))
        #显示新的分数
        echo -ne "\033[1m\033[3${cScoreValue}m\033[${y};${x}H${iScore}         "
        if ((iScore % iScoreEachLevel < line * 2 - 1))
        then
                if ((iLevel < 20))
                then
                        ((iLevel++))
                        ((y = iTop + 14))
                        #显示新的速度级
                        echo -ne "\033[3${cScoreValue}m\033[${y};${x}H${iLevel}        "
                fi
        fi
        echo -ne "\033[0m"
 
 
        #重新显示背景方块
        for ((y = 0; y < iTrayHeight; y++))
        do
                ((yp = y + iTrayTop + 1))
                ((xp = iTrayLeft + 1))
                ((i = y * iTrayWidth))
                echo -ne "\033[${yp};${xp}H"
                for ((x = 0; x < iTrayWidth; x++))
                do
                        ((j = i + x))
                        if ((${iMap[$j]} == -1))
                        then
                                echo -ne "  "
                        else
                                echo -ne "\033[1m\033[7m\033[3${iMap[$j]}m\033[4${iMap[$j]}m[]\033[0m"
                        fi
                done
        done
}
 
 
#下落一行
function BoxDown()
{
        local y s
        ((y = boxCurY + 1))        #新的y坐标
        if BoxMove $y $boxCurX        #测试是否可以下落一行
        then
                s="`DrawCurBox 0`"        #将旧的方块抹去
                ((boxCurY = y))
                s="$s`DrawCurBox 1`"        #显示新的下落后方块
                echo -ne $s
        else
                #走到这儿, 如果不能下落了
                Box2Map                #将当前移动中的方块贴到背景方块中
                RandomBox        #产生新的方块
        fi
}
 
#左移一列
function BoxLeft()
{
        local x s
        ((x = boxCurX - 1))
        if BoxMove $boxCurY $x
        then
                s=`DrawCurBox 0`
                ((boxCurX = x))
                s=$s`DrawCurBox 1`
                echo -ne $s
        fi
}
 
#右移一列
function BoxRight()
{
        local x s
        ((x = boxCurX + 1))
        if BoxMove $boxCurY $x
        then
                s=`DrawCurBox 0`
                ((boxCurX = x))
                s=$s`DrawCurBox 1`
                echo -ne $s
        fi
}
 
 
#下落到底
function BoxAllDown()
{
        local k j i x y iDown s
        iDown=$iTrayHeight
 
        #计算一共需要下落多少行
        for ((j = 0; j < 8; j += 2))
        do
                ((i = j + 1))
                ((y = ${boxCur[$j]} + boxCurY))
                ((x = ${boxCur[$i]} + boxCurX))
                for ((k = y + 1; k < iTrayHeight; k++))
                do
                        ((i = k * iTrayWidth + x))
                        if (( ${iMap[$i]} != -1)); then break; fi
                done
                ((k -= y + 1))
                if (( $iDown > $k )); then iDown=$k; fi
        done
 
        s=`DrawCurBox 0`        #将旧的方块抹去
        ((boxCurY += iDown))
        s=$s`DrawCurBox 1`        #显示新的下落后的方块
        echo -ne $s
        Box2Map                #将当前移动中的方块贴到背景方块中
        RandomBox        #产生新的方块
}
 
 
#旋转方块
function BoxRotate()
{
        local iCount iTestRotate boxTest j i s
        iCount=${countBox[$iBoxCurType]}        #当前的方块经旋转可以产生的样式的数目
 
        #计算旋转后的新的样式
        ((iTestRotate = iBoxCurRotate + 1))
        if ((iTestRotate >= iCount))
        then
                ((iTestRotate = 0))
        fi
 
        #更新到新的样式, 保存老的样式(但不显示)
        for ((j = 0, i = (${offsetBox[$iBoxCurType]} + $iTestRotate) * 8; j < 8; j++, i++))
        do
                boxTest[$j]=${boxCur[$j]}
                boxCur[$j]=${box[$i]}
        done
 
        if BoxMove $boxCurY $boxCurX        #测试旋转后是否有空间放的下
        then
                #抹去旧的方块
                for ((j = 0; j < 8; j++))
                do
                        boxCur[$j]=${boxTest[$j]}
                done
                s=`DrawCurBox 0`
 
                #画上新的方块
                for ((j = 0, i = (${offsetBox[$iBoxCurType]} + $iTestRotate) * 8; j < 8; j++, i++))
                do
                        boxCur[$j]=${box[$i]}
                done
                s=$s`DrawCurBox 1`
                echo -ne $s
                iBoxCurRotate=$iTestRotate
        else
                #不能旋转，还是继续使用老的样式
                for ((j = 0; j < 8; j++))
                do
                        boxCur[$j]=${boxTest[$j]}
                done
        fi
}
 
 
#DrawCurBox(bDraw), 绘制当前移动中的方块, bDraw为1, 画上, bDraw为0, 抹去方块。
function DrawCurBox()
{
        local i j t bDraw sBox s
        bDraw=$1
 
        s=""
        if (( bDraw == 0 ))
        then
                sBox="\040\040"
        else
                sBox="[]"
                s=$s"\033[1m\033[7m\033[3${cBoxCur}m\033[4${cBoxCur}m"
        fi
 
        for ((j = 0; j < 8; j += 2))
        do
                ((i = iTrayTop + 1 + ${boxCur[$j]} + boxCurY))
                ((t = iTrayLeft + 1 + 2 * (boxCurX + ${boxCur[$j + 1]})))
                #\033[y;xH, 光标到(x, y)处
                s=$s"\033[${i};${t}H${sBox}"
        done
        s=$s"\033[0m"
        echo -n $s
}
 
 
#更新新的方块
function RandomBox()
{
        local i j t
 
        #更新当前移动的方块
        iBoxCurType=${iBoxNewType}
        iBoxCurRotate=${iBoxNewRotate}
        cBoxCur=${cBoxNew}
        for ((j = 0; j < ${#boxNew[@]}; j++))
        do
                boxCur[$j]=${boxNew[$j]}
        done
 
 
        #显示当前移动的方块
        if (( ${#boxCur[@]} == 8 ))
        then
                #计算当前方块该从顶端哪一行"冒"出来
                for ((j = 0, t = 4; j < 8; j += 2))
                do
                        if ((${boxCur[$j]} < t)); then t=${boxCur[$j]}; fi
                done
                ((boxCurY = -t))
                for ((j = 1, i = -4, t = 20; j < 8; j += 2))
                do
                        if ((${boxCur[$j]} > i)); then i=${boxCur[$j]}; fi
                        if ((${boxCur[$j]} < t)); then t=${boxCur[$j]}; fi
                done
                ((boxCurX = (iTrayWidth - 1 - i - t) / 2))
 
                #显示当前移动的方块
                echo -ne `DrawCurBox 1`
 
                #如果方块一出来就没处放，Game over!
                if ! BoxMove $boxCurY $boxCurX
                then
                        kill -$sigExit ${PPID}
                        ShowExit
                fi
        fi
 
 
 
        #清除右边预显示的方块
        for ((j = 0; j < 4; j++))
        do
                ((i = iTop + 1 + j))
                ((t = iLeft + 2 * iTrayWidth + 7))
                echo -ne "\033[${i};${t}H        "
        done
 
        #随机产生新的方块
        ((iBoxNewType = RANDOM % ${#offsetBox[@]}))
        ((iBoxNewRotate = RANDOM % ${countBox[$iBoxNewType]}))
        for ((j = 0, i = (${offsetBox[$iBoxNewType]} + $iBoxNewRotate) * 8; j < 8; j++, i++))
        do
                boxNew[$j]=${box[$i]};
        done
 
        ((cBoxNew = ${colorTable[RANDOM % ${#colorTable[@]}]}))
 
        #显示右边预显示的方块
        echo -ne "\033[1m\033[7m\033[3${cBoxNew}m\033[4${cBoxNew}m"
        for ((j = 0; j < 8; j += 2))
        do
                ((i = iTop + 1 + ${boxNew[$j]}))
                ((t = iLeft + 2 * iTrayWidth + 7 + 2 * ${boxNew[$j + 1]}))
                echo -ne "\033[${i};${t}H[]"
        done
        echo -ne "\033[0m"
}
 
 
#初始绘制
function InitDraw()
{
        clear
        RandomBox        #随机产生方块，这时右边预显示窗口中有方快了
        RandomBox        #再随机产生方块，右边预显示窗口中的方块被更新，原先的方块将开始下落
        local i t1 t2 t3
 
        #显示边框
        echo -ne "\033[1m"
        echo -ne "\033[3${cBorder}m\033[4${cBorder}m"
 
        ((t2 = iLeft + 1))
        ((t3 = iLeft + iTrayWidth * 2 + 3))
        for ((i = 0; i < iTrayHeight; i++))
        do
                ((t1 = i + iTop + 2))
                echo -ne "\033[${t1};${t2}H||"
                echo -ne "\033[${t1};${t3}H||"
        done
 
        ((t2 = iTop + iTrayHeight + 2))
        for ((i = 0; i < iTrayWidth + 2; i++))
        do
                ((t1 = i * 2 + iLeft + 1))
                echo -ne "\033[${iTrayTop};${t1}H=="
                echo -ne "\033[${t2};${t1}H=="
        done
        echo -ne "\033[0m"
 
 
        #显示"Score"和"Level"字样
        echo -ne "\033[1m"
        ((t1 = iLeft + iTrayWidth * 2 + 7))
        ((t2 = iTop + 10))
        echo -ne "\033[3${cScore}m\033[${t2};${t1}HScore"
        ((t2 = iTop + 11))
        echo -ne "\033[3${cScoreValue}m\033[${t2};${t1}H${iScore}"
        ((t2 = iTop + 13))
        echo -ne "\033[3${cScore}m\033[${t2};${t1}HLevel"
        ((t2 = iTop + 14))
        echo -ne "\033[3${cScoreValue}m\033[${t2};${t1}H${iLevel}"
        echo -ne "\033[0m"
}
 
 
#退出时显示GameOVer!
function ShowExit()
{
        local y
        ((y = iTrayHeight + iTrayTop + 3))
        echo -e "\033[${y};0HGameOver!\033[0m"
        exit
}
 
 
#显示用法.
function Usage
{
        cat << EOF
Usage: $APP_NAME
Start tetris game.
 
  -h, --help              display this help and exit
      --version           output version information and exit
EOF
}
 
 
#游戏主程序在这儿开始.
if [[ "$1" == "-h" || "$1" == "--help" ]]; then
        Usage
elif [[ "$1" == "--version" ]]; then
        echo "$APP_NAME $APP_VERSION"
elif [[ "$1" == "--show" ]]; then
        #当发现具有参数--show时，运行显示函数
        RunAsDisplayer
else
        bash $0 --show&        #以参数--show将本程序再运行一遍
        RunAsKeyReceiver $!        #以上一行产生的进程的进程号作为参数
fi
```
</details>

我没想到俄罗斯方块也有Shell版的，第一次发现的时候很惊讶，也很佩服这个作者，代码是完整版的，你可以直接复制粘贴拿去玩。

</details>



<details>
	
<summary>4st week</summary>

### <span id="4">Algorithm</span>

<details>
	
<summary>算法</summary>

```
26. Remove Duplicates from Sorted Array(Easy)

Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Example 1:

Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
Example 2:

Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```
</details>

<details>
	
<summary>题解</summary>

本题是使用in-place操作，在一个已排序的数组中删除多余的元素，返回非重复元素的个数。

既然是in-place操作，意味着不能使用额外的空间复杂度，而且还是已排序的数组，自然就使用双指针来解了，跟第三周的算法题(27.Remove Element)差不多

两个指针，第一个指针代表非重复的个数，第二个指针遍历数组，如果遇到非重复的个数，将其移动到左边，逐一累加。

这样，当遍历整个数组之后，就将非重复的数移动到数组的左边，然后忽略掉数组右边的元素，返回第一个指针就相当于删掉多余的元素了。

注意：最后的index+1，因为第一个指针从下标0开始，但是需要返回的是元素的长度，所以需要加1.

```golang
func removeDuplicates(nums []int) int {
    if len(nums)==0{return 0}
    
    index:=0
    for i:=1;i<len(nums);i++{
        if nums[i]!=nums[index]{
            index++
            nums[index],nums[i]=nums[i],nums[index]
        }
    }
    return index+1
}
```
</details>

### Review

由于种种原因，上周的ARTS没做，到时候会找个时间补回来。

最近在看 [effective go](https://golang.org/doc/effective_go.html),所以这周打算翻译effective go 中的一小部分，虽然很多内容没什么营养价值，但翻译使得我的英语提高速度快过囫囵吞枣式地阅读

<details>
	
<summary>点击查看翻译内容</summary>

# 介绍

Go是一门新语言，虽然它从现有的语言中借鉴了一些特性，但它所拥有的特殊性使得，go程序会与之相近的语言所编写的程序不同。C++与Java的程序直接翻译到Go并不会输出满意的结果。另一方面，从go的角度考虑一个问题会产生一个成功但完全不同的程序，换句话说，为了写好go程序，明白它的特性和习语(idiom)很重要。同时明白go编程中的既定规范(established conventions)也是必要的，比如命名、格式化、程序结构等，也因为你遵循规范，写的程序才使得其他程序员更容易看懂。

该文档给出了一些如何写出清晰、惯用的go代码技巧，它是 [language specification](https://golang.org/ref/spec), [the Tour of Go](https://tour.golang.org/),和 [How to Write Go Code](https://golang.org/doc/code.html) 的补充，所以阅读该文档之前先读这三个文档。

**例子**

[go源码包](https://golang.org/src/)不只是打算作为核心库服务，同时也是作为如何使用这门语言的例子。此外，许多包中还包含了完整的运行代码，你可以直接在https://golang.org/ 网站上运行，例如[这个](https://golang.org/pkg/strings/#example_Map)。如果你遇到了问题或想知道某些东西是怎么实现的，库中的文档、代码和例子会提供答案、思路、背景。

# 格式化

格式化是最受争议但也是最无关紧要的问题，人们可以编写不同格式的代码，但如果不是必须的，最好不要那么做。

如果每个人都遵循相同的风格，那么专注于该主题的时间就会减少。但是问题是如何在没有很长的规定风格指南的情况下接近这个乌托邦呢。

在go中我们采取了一种不寻常的方法，让机器处理大多数格式问题。gofmt程序读取标准缩进和垂直对齐的样式发出源代码，保留并在必要时重新格式化注释。如果你想知道如何处理一些新的布局问题，运行 gofmt，如果结果看起来不对劲，从新组织你的程序，再试着运行(这一段都是直译过来的，读起来好奇怪)。

这里有个例子，没有必要把时间花费在结构体字段注释的排列上，gofmt会自动帮你完成。

```
type T struct {
    name string // name of the object
    value int // its value
}
```
gofmt会自动排列

```
type T struct {
    name    string // name of the object
    value   int    // its value
}
```

所有在标准库中的源码都已经过gofmt排列过，

gofmt格式化细节如下：

缩进：

gofmt使用tab作为默认缩进

行长度：

go中没有行长度限制，所以不用担心你的长度会超出，

括号：

go中不想c或java中那么多括号，控制流程(if for switch )语法中都不需要括号。同时运算符优先级层次也更简洁清晰，所以
	
	x<<8 + y<<16

意味着间距的含义，不像其他语言(means what spacing implies,unlike ohter language,直译过来好奇怪)


吐槽一下其中一些句子，翻译的时候怎么译意思都不通顺，似乎不太符合上下文的意思，给人的感觉好奇怪：

比如

```
Also, the operator precedence hierarchy is shorter and clearer, so
x<<8 + y<<16
means what the spacing implies, unlike in the other languages.
```
句子每个都单词都懂，前半句也没问题，但是后半句直译就成了：

	x<<8 + y<<16意味着间隔的含义，不像其他语言



</details>

### Tips

一开始，我以为在go中没有内置的一个数组直接拓展另一个数组的解决办法，需要用一个循环来解决，比如：

```
	arrA := []int{1, 2, 3}
	arrB := []int{4, 5, 6}

for i:=0;i<len(arrB);i++{
	arrA=append(arrA,arrB[i])
}
```

后来我发现一个神奇的东西，通过这种方式就可以直接拓展数组，注意就是在arrB后加三个点

```
	arrA := []int{1, 2, 3}
	arrB := []int{4, 5, 6}
	arrA=append(arrA,arrB...)
```
好神奇

### Share

本周的share打算分享一位大牛的博客：[Rob Pike](https://commandcenter.blogspot.com/),wiki了一下他的成就吓一大跳.

- go语言的创始人之一
- UTF-8 共同创造者之一
- 贝尔实验室成员

还有一大堆我虽然没听过，但肯定是很高的成就，仰慕之心油然而生，得找个时间好好翻他的博客才行，最好是翻烂的那种，以表心中的敬仰。

</details>

<details>
    
<summary>5st week</summary>

### <span id="5">Algorithm</span>

<details>
    <summary>905. Sort Array By Parity(Easy)</summary>

``` 
Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.

You may return any answer array that satisfies this condition.

 

Example 1:

Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
 

Note:

1 <= A.length <= 5000
0 <= A[i] <= 5000

```

</details>

<details>
    <summary>解法</summary>

这道题是奇数和偶数分类，奇数放前面，偶数放后面，我一开始的思路是用双指针，一个指针从左开始遍历，一直往右直到遇到奇数才停下，一个指针从右开始，一直往左直到遇到偶数才停下，然后交换两个指针的元素，这样就能实现奇数全在左，偶数全在右了。但是遇到一种临界条件，比如[0,2,1],输出的结果为[0,1,2]。才发现这种思路行不通

后来看一下解法的提示，提示用排序，然后自定义比排序的比较，然后我就用了冒泡排序(原谅我的水平，只能手写冒泡排序)，得出以下解法

```
func sortArrayByParity(A []int) []int {
    if len(A) == 0 {
        return A
    }
    for i := 0; i < len(A); i++ {
        for j := 1; j < len(A)-i; j++ {
            if A[j-1]%2 != 0 {
                A[j-1], A[j] = A[j], A[j-1]
            }
        }
    }
    return A
}
```
</details>

### Review

本周review分享Linux日志相关的内容
<details>
    <summary>点击即可展开</summary>

在介绍Linux日志相关的内容之前，先来点开胃菜，比如有时候我们希望查看同时在线的用户，可以用 who命令,可以看到如下输出

```bash
root     pts/0        2019-06-14 09:42 (183.14.18.123)
root     pts/1        2019-06-14 09:42 (183.14.18.321)
root     pts/2        2019-06-15 09:39 (183.14.18.652)
root     pts/3        2019-06-15 09:41 (183.14.18.132)
```
在centos中或debian中，who命令是从 /var/run/utmp日志文件中获取登陆用户信息。

而如果你想查看root用户登陆的历史，可以用last命令，你会看到类似的输出：
```bash
root     pts/3        183.14.18.123     Sat Jun 15 09:41   still logged in   
root     pts/2        183.14.18.149     Sat Jun 15 09:39   still logged in   
root     pts/4        183.14.18.151     Fri Jun 14 16:20 - 19:00  (02:40)    
root     pts/4        183.14.18.152     Fri Jun 14 14:49 - 16:20  (01:30)    
```

> 这里顺便介绍一个小技巧，比如你想查看系统重启的记录，可以使用这条命令 last reboot

而如果想查看所有用户最近一次的登陆时间，可以用lastlog命令，你会看到类似的输出：

```
Username         Port     From             Latest
root             pts/0    183.66.77.88    Tue Jun 18 09:40:20 +0800 2019
bin                                        **Never logged in**
daemon                                     **Never logged in**
adm                                        **Never logged in**
lp                                         **Never logged in**
sync                                       **Never logged in**
shutdown                                   **Never logged in**
halt                                       **Never logged in**
mail                                       **Never logged in**
operator                                   **Never logged in**
games                                      **Never logged in**
ftp                                        **Never logged in**
nobody                                     **Never logged in**
systemd-network                            **Never logged in**
dbus                                       **Never logged in**
polkitd                                    **Never logged in**
ntp                                        **Never logged in**
sshd                                       **Never logged in**
postfix                                    **Never logged in**
chrony                                     **Never logged in**
tss                                        **Never logged in**
jack       pts/0                     Tue Jun 18 09:49:04 +0800 2019

```


开胃菜讲完了，下面来进入主题，介绍一下如系统日志相关的内容，主要包含如下方面：

- 系统日志是怎么产生的
- 产生的系统日志都去了哪里
- 日志的组成部分有哪些，如何查看
- 介绍日志的规则，即日志的来源和去处(来源意味着需要记录哪些进程产生的记录，去处是指这些记录需要重定向到终端还是写入日志文件中
)
- 最后通过一个完整的例子，教你如何自定义规则，并产生一条日志，然后可在文件中查看(包含完整的命令)

### 系统日志是怎么产生的

系统日志的是由一个叫rsyslog的守护进程，它会监听系统的各个模块所生成的消息，比如用户通过ssh登陆的时候，操作系统的sshd模块会生产一条类似这样的记录

    pam_unix(sshd:session): session opened for user root by (uid=0)

然后rsyslog会将这条记录存入到系统特定的目录中

### 产生的日志都去了哪里

你们会不会好奇，系统生产的日志都去了什么地方，它其实存放在/var/log中(centOS),如果你用 ls /var/log，就会看到如下输出：

```
anaconda           btmp           dmesg       grubby              maillog            myTestInfo.log  secure            tallylog
boot.log           chrony         dmesg.old   grubby_prune_debug  maillog-20190616   ntpstats        secure-20190616   tuned
boot.log-20190614  cron           ethcon.log  imageboot.log       messages           qemu-ga         spooler           wtmp
boot.log-20190617  cron-20190616  firewalld   lastlog             messages-20190616  rhsm            spooler-20190616  yum.log
```
可以看到里面包含了许多日志文件

### 日志的组成部分由哪些

我们先来看一下具体系统日志的长什么样：

```
Jun 17 10:12:13 vultr sshd[21546]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Jun 17 10:12:14 vultr sshd[21546]: Failed password for root from 218.92.0.206 port 33909 ssh2
```
先说一下，这是一条很有意思的记录，这条记录的意思是218.92.0.206这个ip的人，尝试用root用户登陆我的主机，我不知道他是登陆的时候输错IP，还是想暴力破解我的服务器密码，反正我看到的时候吓一跳。

好了，接着说日志的格式，日志由四个部分组成，分别是：日期+主机名+模块+日志信息

格式|实例
-|-|
日期|Jun 17 10:12:14|
主机名|vultr|
模块|sshd[21546] |
日志信息|Failed password for root from 218.92.0.206 port 33909 ssh2|



每条日志都会标明时间，主机，和产生日志的模块，所以你可以很清晰地区别这条日志是什么时候产生的，由谁产生的。

### 日志规则介绍

前面介绍了日志是怎么来的，也介绍了日志都去了哪里，即日志的来源和去处都介绍了。你会不会有那么一点冲动，自己定义日志的来源和去处，你可以指使日志从哪里来，到哪里去。

下面我给你介绍一下，如何定义日志的来源和去处。

其实系统定义日志的规则，是放在/etc/rsyslog.conf文件种，我摘抄其中一段给你介绍一下日志的规则

```
cron.info                                               /var/log/cron
auth.*                                                  @prep.ai.mit.edu
auth.*                                                  root,amrood
```

可以看到日志分两个部分，左边的部分就是日志的来源，来源分中也分两个部分，格式为：设备+日志级别

比如cron.info ，cron是指产生日志的设备(facility),info是指日志的级别

去处就简单了，就一个部分，你可以指定本地的一个文件，也可以指定远程的一台服务器，比如 @prep.ai.mit.edu 或/var/log/cron

好了，规则你知道了，下面列个表，详细地介绍各个部分

### 例子

下面通过一个具体的例子，向你介绍如果自定义日志规则，并产生一条日志消息。

**第一步，定义日志规则**

在 /etc/rsyslog.d/下创建一个文件，名字你随便取，在这里我用myLogRules.conf，你可以直接复制以下命令(centOS)

```bash
vim /etc/rsyslog.d/myLogRules.conf
```

然后输入以下内容

    *.info  /var/log/myTestInfo.log

退出保存(:wq)

**第二步，重启rsyslog守护进程**

```bash
service rsyslog restart
```

**第三步，生成一条日志**

    logger -p local4.info " This is a info message from local 4"

好了，你已经生成了一条日志消息了，输入以下命令：

     tail /var/log/myTestInfo.log

你可以在/var/log/myTestInfo.log文件了看到如下日志
```
Jun 19 10:31:40 vultr root: This is a info message from local 4

```

到目前为止，我们已经介绍了日志是怎么产生的，产生的日志都去了哪里，日志由哪几部分组成，还介绍了如何自定义日志的来源和去处，并通过了一个具体的例子向你介绍如何实现这个过程，虽然我认为还有一些内容没有在这个review里介绍到(因为我自己都没搞懂，包括我查了一些文档也没有清晰地介绍这个，就是自定义日志的action中，如果不填用户是不是默认是所有用户都能查看，如果指定用户又会怎样)，但我感觉已经大致框架还是很清晰的，下次有空再详细介绍跟日志相关的内容，上周ARTS总算补全了。

本周review来源于这篇[文章](https://www.digitalocean.com/community/tutorials/how-to-view-and-configure-linux-logs-on-ubuntu-and-centos)并加以整理，你可以跳转到原文查看详细的介绍
</details>

### Tips
中间由于种种原因，缺了很多周的ARTS，从这周开始接上，本周的Tips分享linux下如何在一堆字符串中过滤无用的，并取出想要的字段

比如当我们想kill掉某个进程的时候，都会通过ps命令然后查找其PID
你可以通过这种方式，然后手动复制其PID

```bash
bash-4.2# ps aux|grep top
root     33457  0.1  0.0 161972  2292 pts/3    S+   12:55   0:00 top
root     33463  0.0  0.0 112704   960 pts/2    S+   12:55   0:00 grep --color=auto top
```
但是作为一个程序员，有一种更优雅的方式，一条命令就能搞定

    kill -9 $(ps aux|grep top|grep -v color|awk -F ' ' '{print $2}')

如果你想kill掉其他程序，把top改成你想kill掉的程序即可，比如如果你想kill掉mysql就改为：

    kill -9 $(ps aux|grep mysql|grep -v color|awk -F ' ' '{print $2}')

> 即使开了多个同样的程序也会全都被killed掉




下面来一步步解释一下上述命令，先输入ps aux，然后出现以下结果
```bash
ps aux
root     34532  0.0  0.0 161972  2296 pts/3    S+   13:09   0:00 top
root     34535  0.0  0.0 161972  2292 pts/2    S+   13:09   0:00 top
root     34540  0.0  0.0 112704   960 pts/4    S+   13:09   0:00 grep --color=auto top
```

> 记住，我们的目的是要取出PID，及每一行的第二个字段。为了达到这个目的，我们需要做如下事情：

- 过滤掉无关的PID，比如第三行的PID就不是我们想要的
- 取出每行的第二个字段

第一步，过滤无关PID我们可以这么写

    ps aux|grep top|grep -v color

即通过grep -v color这条命令，然后你将会看到以下内容

```bash
root     34532  0.1  0.0 161972  2088 pts/3    S+   13:09   0:00 top
root     34535  0.1  0.0 161972  2084 pts/2    S+   13:09   0:00 top
```

第二步，取每一行的第二个字段，可以用过awk命令

    ps aux|grep mysql|grep -v color|awk -F ' ' '{print $2}' 

解释一下 -F <code>' '</code>为分隔符的意思,即以空格符为分隔符，然后'{print $2}' 输出第二个字段

这条命令会显示以下结果：
```
34532
34535
```
这就是我们要找的PID了

完整的命令合起来就是：

    kill -9 $(ps aux|grep top|grep -v color|awk -F ' ' '{print $2}')

$()这个符号在这里的意思就是命令置换，即将最后过滤剩下的PID作为kill命令的参数。

### Share

本周share分享一个golang中的小技巧，本来我以为golang中是无法删除slice中的元素的，但是google了一下，在stackoverflow里找到一个令我拍案叫绝的写法

比如一个slice [1,2,3,4,5,6],你想删除里面的一个元素，假设我要删除5把，可以这么写：

```golang
package main

import "fmt"

func main() {
    s := []int{1, 2, 3, 4, 5, 6}

    for i, v := range s {
        if v == 5 {
            s = append(s[:i], s[i+1:]...)
        }
        fmt.Println(s)
    }
}

```
代码是完整的，你可以复制粘贴去跑一下。

最核心的是这一句 s = append(s[:i], s[i+1:]...),第一眼看到我真的佩服得五体投地，我没想到切片的索引还能这么用，而且还用上s[i+1]... 这个语法，太牛逼了。反正对我这种新手来说，这完全颠覆我的之前对go的运用。读取来就跟读诗的感觉，amazing。

抱歉，搞得我就跟山里人进城的一样，原谅我毫不吝啬地表达对这群充满智慧的人的仰慕之情。


</details>


<details>

    
<summary>6st week</summary>

### <span id="6">Algorithm</span>

<details>
    
<summary>题目</summary>

>Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

>Given linked list -- head = [4,5,1,9], which looks like following:



 


>Example 1:

>Input: head = [4,5,1,9], node = 5
>Output: [4,1,9]
>Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.

>Example 2:

>Input: head = [4,5,1,9], node = 1
>Output: [4,5,9]
>Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.
 

>Note:

>The linked list will have at least two elements.
>All of the nodes' values will be unique.
>The given node will not be the tail and it will always be a valid node of the linked list.
>Do not return anything from your function. 
</details>

<details>
    
<summary>解法</summary>

题目的意思是给出链表中的一个节点，把这个节点删除，而且该节点都是都是不是尾节点，所以就很简单了，只需要把下一个的值取出来替换现在的值，然后再将下一个节点指向下下个节点即可，这样就相当于把当前节点给删了。

```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteNode(node *ListNode) {
    if node==nil||node.Next ==nil{
        return
    }
    
    next := node.Next
    node.Val = next.Val
    node.Next = next.Next 
}
```
</details>

### Review

本周review介绍如何在linux系统下设定一些定时任务，比如每天中午十二点打印"hello,world!",可以这么写

1. 打开终端，在命令行中输入以下命令
```
crontab -e
```
2. 把这条命令粘贴到文件末尾
```
0 12 * * * echo "hello,world!"
```
3. 退出保存即可

解释一下一条命令，其实很简单 0 12 * * * 分别代表： 分钟:小时:一个月的第几天:一年中的第几月:一个星期中的第几天,或者看下面这个表格会更清晰一些

```bash
* * * * *  Command_to_execute
- � � � -
| | | | |
| | | | +�� Day of week (0�6) (Sunday=0) or Sun, Mon, Tue,...
| | | +���- Month (1�12) or Jan, Feb,...
| | +����-� Day of month (1�31)
| +������� Hour (0�23)
+��������- Minute (0�59)
```

当然，如果你需要指定很多任务，一条命令无法满足，那么可以写在一个shell脚本里，然后执行这个shell脚本即可

比如每天0点执行test.sh脚本，可以这么写(需要附上这个脚本的路径),

```bash
0 0 * * * /root/test.sh
```
更详细的可以参考[这里](https://www.tutorialspoint.com/unix_commands/crontab.htm)


### Tips

本周tips介绍一个iptables使用技巧，iptables是linux下的一与个网络过滤相关的命令，根据你定义的规则过滤掉一些数据。

比如你在/var/log/secure 系统日志中看到某个ip多次ssh登陆失败，这很有可能是他想尝试暴力破解服务器的密码，你可以把他的ip给封了，这样他就无法破解了

    iptables -A INPUT -s 123.234.224.231 -j DROP

这样就可以了

iptables是一个很强大的工具，可以定义很多网络的规则，比如可以可以尝试以下命令：

    iptables -A INPUT -p icmp --icmp-type 8 -j DROP 

这条命令的的意思是禁止别人ping你，它的实现原理是这样的，ping这个程序本质上是通过icmp协议发送一个请求，然后等待目的主机的回复
。按照icmp报文格式，报文的第一个字段(type)为8的时候，代表的是请求报文。

知道这一点后就好办了，你直接自定义一条规则，判断所有进来的数据包，如果是icmp协议，且type字段为8的，直接丢弃，这样就可以实现禁止别人ping你了，^_^。


本周先粗略介绍以下iptables的使用，有空找个时间写个详细点的介绍。

### Share

本周share分享一个有意思的东西，其实也算是老掉牙的技术了，就是平时上班是用钉钉打卡，人事部的把打卡范围设置在公司100米以内，只有进入了这个区域点击打卡才算打卡成功。我一听到这个消息的时候，顿时笑出声来，同事还问我怎么了，我说没事。其实我心里在想，这还不简单吗，弄个GPS修改器，把定位调到公司范围内不就可以随时打卡了。

然后今天回来稍微弄了一下，去google paly下一个叫Fake GPS的软件，找到公司的地址，然后打开钉钉定位的时候，有偏差，我微调了一下，然后就可以实现在家里打卡了。说实话，弄这个东西也不是为了逃避迟到，只是单纯地觉得从技术层面来说这是一个很有意思的想法，然后就去实现罢了。

最后还用了百度和高德地图测了一下定位，发现可以修改高德地图的当前位置，而百度地址怎么修改GPS都没用，不知道用了什么技术能识别我真实的位置，牛批。

</details>


<details>
    
<summary>7st week</summary>

### <span id="7">Algorithm</span>

### Review

### Tips

### Share


本周阅读文章:

- [我是怎么招聘程序员的](https://coolshell.cn/articles/1870.html)
- [来信， 创业 和 移动互联网](https://coolshell.cn/articles/5815.html)

</details>


<!-- <details>
    
<summary>5st week</summary>

### <span id="5">Algorithm</span>

### Review

### Tips

### Share

</details>

<details>


<!-- 每周坚持ARTS 的目的，是为了刷完leetcode上大部分的题，和无障碍阅读英语文献。前者完成的标准很好判断，后者完成的标准，比如要阅读哪些英语文献、wiki上的还是技术博客、亦是英语原著，到时候再定，反正提高达到标准前期的道路是一样的，大量的翻译就可以了。


