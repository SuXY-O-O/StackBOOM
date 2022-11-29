<br/>
<br/>
<br/>
<center><font size=15><b>StackBOOM·语言设计说明书</b></font></center>
<br/>
<br/>
<br/>
<center><font size=6><b>程序设计语言原理大作业</b></font></center>
<br/>

<center><font color=grey>苏星熠（ZY2206148） HHC（？？？？？）</font></center>
<center><font color=grey>2022年12月</font></center>

---

[toc]

---

# 1. 设计背景和目标
## 1.1 设计背景
经过一学期的学习，我们学习得到了许许多多不同设计思想的语言，包括过程式、对象式、函数式、逻辑式等等。事实上，在使用了多年面向对象的语言以后，再度上手其他思想的语言让我们感到十分“不快”——或许从语言排行的名单来看也能够发现问题，毕竟使用最多的语言，几乎都是面向对象的语言。

而面对这些发展已然成熟、各个几乎都比我们年龄还大的语言，我们实际上很难提出哪些对于已有语言的改进，如果非要提出来一个，那就是我非常想给Python语言的语句结尾增加分号。所以，在大作业的逼迫下，我们思考了一个问题：是否存在其他的语言设计思想。

实际上思考一下，程序语言无非有两个部分：数据和操作。对象式语言把这两个打了个包叫对象，所以能够万物皆对象；函数式语言把操作看得比数据重要，所以“函数成为了一等公民”；过程式语言相当于数据和操作平权。那么，存不存在一种语言，将数据的权重进行放大呢？

然后我们想到了计算器上的堆栈式表示法，或者说逆波兰表示法。在计算时，对于数据会存入栈中，先入后出，对于操作会直接进行计算，用后即弃，在某种意义上，也算是提高了数据的地位。


查阅相关资料后，本来欣喜若狂以为自己进行了创新，结果发现，果然是读书太少了。上述设计思想指导的语言已经经历了发展、鼎盛再到衰落的过程。典型的语言有RPL、PostScript语言。前者应用于惠普公司制造的计算器产品，后者则由Adoby公司制作，首先应用于打印机产品，后用于文档页面的描述，并对后续PDF的诞生有很大影响。

上述两个语言有一些共同：专用性很强，并且大部分情况下，语言的代码经由计算机自己生成。这导致语言本身还是有不少改进空间的。虽然对于是否会有人乐意使用“反常识”的逆波兰表达式写程序存有疑问，不过，改进上述语言，并应用于对于“堆栈”这种数据结构的教学，或许也不失为堆栈式语言一种存在价值。

综上，本次大作业，我们将参考PostScript，构建一种堆栈式语言。该语言命名为StackBOOM。


## 1.2 设计目标
本次大作业设计目标为仿照PostScript语言，设计一种“幼儿态”的堆栈式语言StackBOOM，其包含简单的数据处理能力，能够实现条件判断以及循环操作，并能够让用户定义自己的操作。

之所以称之为“幼儿态”，原因在于受限于作业时间限制以及个人能力等，本次设计的语言并没有实现所有目前主流语言的全部功能，例如生成字典这种数据结构。

对于期望StackBOOM实现的功能列举如下：

1. 能够实现数据的直接输入
2. 能够进行运算操作
3. 能够组合各种操作
4. 能够实现判断和循环
5. 能够部分实现并行

# 2. 词法设计
## 2.1 关键字
```
NEWSTACK = "news";
ENDSTACK = "ends";
WHILE = "while";
BREAK = "break";
CONTINUE = "continue";
IF = "if";
IFELSE = "ifelse";
DEF = "def";
POP = "pop";
KEYWORD = NEWSTACK | ENDSTACK | WHILE | BREAK | FOR | CONTINUE | IF | IFELSE | DEF | POP;
```
## 2.2 运算符
```
OPERATOR = "+" | "add" | "-" | "sub" | "*" | "mul" | "\" | "div" | "e" | "**" | "pow" | "sqrt";
JUDGE = "==" | "!=" | ">" | "<" | ">=" | "<=";
LOGIC = "&" | "and" | "|" | "or";
POPMARK = ",";
SAVEMARK = ";";
MARK = POPMARK | SAVEMARK;
```

## 2.3 标识符
```
LETTER = a | b | c | ... | z;
ALPHA = A | B | C | ... | Z;
NUM = 0 | 1 | 2 | .. | 9;
NAME = (LETTER | ALPHA | "_"), {LETTER | ALPHA | "_" | NUMBER};
IDENTIFIER = "/", NAME;
```

## 2.4 数字与字符
```
STRING = """, {所有字符}, """;
SPACE = " ";
EOL = "/n";
INTEGER = ["-"], NUM, {NUM};
FLOAT = INTEGER, ".", NUM, {NUM};
NUMBER = INTEGER | FLOAT;
```

# 3. 语法设计
```
OP = NAME | KEYWORD | OPERATOR;
ITEM = IDENTIFITER | NUMBER;
BLOCK = "{", [ITEM | OP | BLOCK], {SPACE, [ITEM | OP | BLOCK]}, "}", EOL;
OPLINE = [BLOCK | ITEM], {SPACE, BLOCK | ITEM}, SPACE, OP, EOL;
JUDGEBLOCK = "{", (BLOCK | ITEM), SPACE, (BLOCK | ITEM), SPACE, JUDGE, "}", EO:;
JUDGELINE = JUDGEBLOCK | ("{", JUDGEBLOCK, [SPACE, LOGIC, SPACE, JUDGEBLOCK], "}"), EOL;
IFLINE = OPLINE, JUDGELINE, IF, EOL;
IFELSELINE = OPLINE, OPLINE, JUDGELINE, IFELSE, EOL;
WHILELINE = OPLINE, 
```