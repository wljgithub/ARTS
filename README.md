# ARTS

ARTS which is 

- Algorithm
- Review
- Tips
- Share

Do the above things every week

# Punch record

- [1st Week](#1)
- [2st Week](#2)
 

<details>
	<summary>1st week</summary>
	
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
	
### <span id="1">Algorithm</span>

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

用vim保存退出的是记得带上！符号，即: wq!  (因为文件权限只允许管理员写入)

好了，然后你上一下百度，发现上不了，^_^.

### Share

本周的Share打算分享一位网友的博客，他把Leetcode上大部分题都刷完了，而且很多题还给了几种解法并详细地分析思路，然后做一个汇总。我其实真的很佩服这些人，上千道算法题，即使每天刷一道，而且得连续不断地刷，这样也要两年多的时间。这还没完，还把解过的题都整理起来，附上解题思路，这个过程想想都觉得十分不容易。

我特别的佩服这些人的毅力，他们一直都是我学习的榜样，而且我也打算要这么干，md,想想都刺激。

好了，链接在这，[点就好](https://www.cnblogs.com/grandyang/p/4606334.html)
</details>