# 导言与预备知识

## 导言

首先我们需要知道计算理论这门课应该讲的内容.一般而言，计算理论可以分为三部分内容：

* Automata Theory 自动机理论：介绍一些基本计算模型的形式化表达及它们对应的语言，即有限自动机与正则语言和下推自动机与上下文无关语言；
* Computability Theory 可计算性理论：定义图灵机，通过可计算性对问题进行分类；
* Complexity Theory 复杂度理论：通过解决一个问题的难度对问题进行分类.

本科阶段 2.0 学分的计算理论直到 2022 年都因为课时以及进度问题只涉及前两部分的内容，关于复杂度理论，我会在笔记中的计算理论进阶部分挑选几个专题做记录.

本笔记主要参考教材为《Elements of the Theory of Computation (2nd edition)》[美] Harry R. Lewis 等 著，辅助参考教材为《Introduction to the Theory of Computation (3rd edition)》[美] Michael Sipser 著。前者是本课程使用的教材，使用目的在于符号等与考试统一，后一本是经典的教程，由浅及深，通俗易懂.

## 预备知识

关于集合、关系与有限集无限集的基本定义此处不再阐述，我们从三种基本的证明方法开始.

### 三种基本的证明方法

**数学归纳法**

基本原理不再阐述，这是基于自然数集合的良序性定义的，接下来通过证明鸽巢原理进行应用.

**鸽巢原理（抽屉原理）**

**定理**：若 $A$ 和 $B$ 为有限集合且 $|A|>|B|$，则不存在从 $A$ 到 $B$ 的单射.

我们利用数学归纳法进行证明：

归纳奠基：若 $|B|=0$，表明不存在从 $A$ 到 $B$ 的映射，故也不存在单射，显然成立；

假设 $f:A\to B$，$|A|>|B|$，且 $|B|<n(n\ge 0)$，则 $f$ 不是单射.接下来考虑 $|A|>|B|=n+1$ 的情况. 我们首先选择一个 $A$ 中的元素 $a$，作如下分类讨论：

1. 若存在 $a'\in A$，使得 $f(a)=f(a')$，则 $f$ 不是单射；
2. 若不存在，则考虑剩余的集合上的函数 $g:A-{a}\to B-{f(a)}$，而此时 $|B-\{f(a)\}|=n$，由归纳假设可知 $g$ 不是单射，则存在 $b,b'\in A-{a}$ 使得 $g(b)=g(b')$，根据 $g$ 的定义可知也即存在 $b,b'\in A$ 使得 $f(b)=f(b')$，故 $f$ 不是单射，证毕.

鸽巢原理也可以推广到无限集合中，日后可以用来说明图灵机无法表达所有的语言.

**对角化（Diagonalization）方法**

对角化方法是一种重要的思想，我们给出一个基于关系的表述：

设 $R$ 是一个集合 $A$ 上的二元关系，我们称 $D=\{a:a\in A \land(a,a)\notin R\}$ 为对角集. 对于任意的 $a\in A$，令$R_a=\{b:b\in A \land (a,b)\in R\}$. 则 $D$ 一定与所有的 $R_a$ 都不一样.

证明是简单的，我们分两种情况讨论即可：

1. $\forall a \notin R_a$，有 $(a,a)\notin R$，则必有 $a \in D$；
2. $\forall a \in R_a$，有 $(a,a)\in R$，则必有 $a \notin D$.

由此我们可以知道，任意 $a$ 对应的 $R_a$ 都和 $D$ 不一样.

我们可以用对角化方法证明自然数的幂集 $2^\mathbf{N}$ 是不可数集，也可以证明 $(0,1)$ 上的实数不可数。一般方法是找到一种列举的方式，然后通过对角化构造出列举不出的元素即可.

**例**：证明自然数的幂集 $2^\mathbf{N}$ 是不可数集.

证：假设 $2^\mathbf{N}$ 可列，我们将其中的元素（即自然数集的子集）排列如下：

$$2^\mathbf{N}=\{R_0,R_1,R_2,\cdots\}$$

接下来我们构造对角集为 $D=\{n\in\mathbf{N}:n\notin R_n\}$，例如 $R_3=\{1,4,5\}$ 就属于这种情况. 接下来我们证明不存在 $n$ 使得 $D=R_n$，方法与对角化原理一致，分类讨论即可：

1. $\forall k \in R_k$，必有 $k \notin D$；
2. $\forall k \notin R_k$，必有 $k \in D$.

故不存在 $n$ 使得 $D=R_n$，故 $2^\mathbf{N}$ 是不可列的.

### 闭包 Closures

闭包也是离散中学习过的内容，即满足一定条件（例如自反性/对称性/传递性）的包含某一关系的最小集合，例如传递闭包我们可以形式化定义如下：

传递闭包 $R^+$ 是满足如下条件的关系：

1. $R\subseteq R^+$；
2. $R^+$ 是满足传递性的；
3. $\forall R',R\subseteq R'$ 且 $R'$ 是传递的，则有 $R^+\in R'$.

这三点综合表明传递闭包 $R^+$ 是包含 $R$ 的满足传递性的最小关系。传递闭包的计算方法实际上是经典的，我们画出关系矩阵然后计算 $R\cup R^2\cup\cdots\cup R^{|R|}$ 即可，原理可以思考一个图的连通性来理解.

本课程研究最多的称为自反传递闭包（考虑计算模型运行 0 步或任意步的情况），一般记为 $R^*$.

### 字母表与语言 Alphabet and Language

我们的计算模型中实际出现的是数据编码成的字符串，因此我们正式讨论前需先了解字符串.

**字母表**

* 字母表是一个有穷的符号集合；
* 例如 $\Sigma_1=\{0,1\}$ 为二进制字母表，$\Sigma_2=\{a,b,\cdots,x,y,z\}$ 是经典的英文字母表；
* 简单起见我们只把字母、数字和其他常用字符（\$、# 等）用做符号.

**字符串**

* 字符串是字母表中符号的有穷序列；
* 一个字符串中可以没有符号，称为空串，记为 $e$；
* 通常用 $u,v,w,x,y,z$ 和希腊字母表示字符串，例如用 $w$ 作为字符串 $abc$ 的名字，但是不能把一个字母既用做符号又用作名字；
* 字母表 $\Sigma$ 上的所有字符串（包括空串）记作 $\Sigma^*$；
* 字符串的长度：# of symbols（# 一般表示 number，即个数），记作 $|w|$.

**字符串上的运算**

连接运算 Concatenation

1. 字符串 $x$ 和字符串 $y$ 的连接是在字符串 $x$ 的后面接上 $y$，记作 $x\circ y$，或简记为 $xy$；
2. $\forall w,w\circ e=e\circ w=w$；
3. 结合律：$(wx)y=w(xy)$；
4. 字符串 $v$ 是 $w$ 的字串（substring）$\iff \exists x,y, w=xvy$；
5. 如果对于某个 $x$，有 $w=xv$，则称 $v$ 是 $w$ 的后缀（suffix）；如果对于某个 $y$，有 $w=vy$，则称 $v$ 是 $w$ 的前缀（prefix）.

乘方运算 Exponentiation

对于任意字符串 $w$ 和任意自然数 $i$，字符串 $w^i$ 归纳定义如下：

1. $w^0=e$；
2. $w^{i+1}=w^i\circ w,\ \forall i\ge 0$.

例如 $w^1=w$，$do^2=dodo$.

反转运算 Reversal

对于字符串 $w$，其反转 $w^R$ 归纳定义如下：

1. 如果 $w$ 是长度为 0 的字符串，则 $w^R=w=e$；
2. 如果 $w$ 是长度为 $n+1>0$ 的字符串，则对于某个 $a\in\Sigma,w=ua,w^R=au^R$.

定义第二点即每次取出字符串中最后一个字母进行操作. 通俗而言字符串反转即将字符串反过来.

**例**：利用归纳定义证明：对于任意两个字符串 $w$ 和 $x$，$(wx)^R=x^Rw^R$.

例如：$(dogcat)^R=(cat)^R(dog)^R=tacgod$.

证：对 $|x|$ 进行归纳. 

归纳奠基：$|x|=0$ 时，$(wx)^R=(we)^R=w^R=ew^R=e^Rw^R=x^Rw^R$；

假设 $|x|\le n(n>0)$ 时有 $(wx)^R=x^Rw^R$，则 $|x|=n+1$ 时有

$$(wx)^R=(w(ua))^R=((wu)a)^R=a(wu)^R=au^Rw^R=(ua)^Rw^R=x^Rw^R.$$

其中 $a(wu)^R=au^Rw^R$ 使用了归纳假设.

**语言**

我们研究的计算模型可以处理某一类的字符串，因此我们需要研究字符串构成的集合. 

* 任意字母表 $\Sigma$ 上的字符串集合（即 $\Sigma^*$ 的任一子集）称为语言，即 $L \subseteq \Sigma^*$. 例如 $\{wyh,jxg,myc,jsll,lljsjkxdy\}$ 是 $\{a,b,\cdots,z\}$ 上的语言；
* 由上述定义可知 $\phi,\Sigma,\Sigma^*$ 都是语言；
* 有穷语言可以列举表示，无穷语言可以通过 $L=\{w\in\Sigma^*:w\textup{ has property }P\}$ 表示. 例如 $L=\{ab,aabb,aaabbb\}=\{a^nb^n\ |\ n\ge 1\}$.

显然一个有限字母表上的字符串是无限的，但其可数性是值得讨论的，我们有如下定理：

**定理**：如果 $\Sigma$ 是一个有限字母表，那么 $\Sigma^*$ 是一个可数无限集合.

证：证明可数性需要构造一个双射 $f:\mathbf{N}\to\Sigma^*$. 为了达成这一目的，我们需要先固定字母表中符号的顺序，比如 $\Sigma=\{a_1,\cdots,a_n\}$，然后我们以如下方式枚举 $\Sigma^*$ 中的元素：

1. 对于每一个 $k\ge 0$，所有长度为 $k$ 的字符串都在所有长度为 $k+1$ 的字符串前枚举；
2. 所有 $n^k$ 个长度为 $k$ 的字符串按字典序枚举（lexicographically），即如果对于某个 $m(0\le m\le k-1)$，$i_l=j_l(l=1,\cdots,m)$ 且 $i_{m+1}<j_{m+1}$，则 $a_{i_1}\cdots a_{i_k}$ 在 $a_{j_1}\cdots a_{j_k}$ 前面.

例如，对于 $\Sigma=\{0,1\}$，排列如下：$e,0,1,00,01,10,11,000,001,010,011,100,\cdots$.

**语言的运算**

1. 语言是集合，因此可以定义并、交、差、补（$\overline{L}=\Sigma^*-L$）运算；
2. 连接运算 $L_1\circ L_2=\{w_1\circ w_2:w_1\in L_1\land w_2\in L_2\}$，可简记为 $L_1L_2$；
3. 基于连接定义乘方运算 $L^0=\{e\},L^{i+1}=LL^i(i\ge 0)$；
4. Kleene Star $L^*=\{w\in\Sigma:w=w_1\circ\cdots\circ w_k,k\ge 0,w_1,\cdots,w_k\in L\}=L^0\cup L^1\cup L^2\cup\cdots$，即连接 $L$ 中 0 个或多个字符串得到的字符串集合. 还有一个概念$L^+=L^1\cup L^2\cup L^3\cup\cdots$.

**例**：$L_1=\{w\in\{0,1\}^*:w \textup{ has an even number of 0's}\}$，$L_2=\{w\in\{0,1\}^*:w\textup{ starts with a 0, the rest symbol are 1's}\}$，$L_1L_2=\{w\in\{0,1\}^*:w\textup{ has an odd number of 0's}\}$.

**例**：$L=\{w\in\{0,1\}^*:w\textup{ has an unequal number of 0's and 1's}\}$，则 $L^*=\{0,1\}^*$.

证：证明两个集合相等只需证明二者互相包含. 根据定义，$L^*\subseteq\{0,1\}^*$ 成立，下面证明反方向. 实际上，我们有若 $L_1\subseteq L_2$，则 $L_1^*\subseteq L_2^*$，而根据 $L$ 的定义有 $\{0,1\}\subseteq L$，故 $\{0,1\}^*\subseteq L^*$ 成立. 故 $L^*=\{0,1\}^*$ 得证.

**Remark**

1. 事实上记号 $\Sigma^*$ 中的 $*$ 与 Kleene Star 含义一致，将 $\Sigma$ 视为单字母字符串组成的语言即可；
2. $\phi^*=\{e\}$，原因是只能选取 0 个字符串进行连接，故结果为 $\{e\}$；
3. $L^+=LL^*$（$L^+$ 连接一个或多个 $L$ 中的字符串），可以将 $L^+$ 看作 $L$ 在连接运算下的闭包，即 $L^+$ 是包括 $L$ 且在连接运算下封闭的最小语言；
4. 对任意的语言 $L$ 有 $(L^*)^*=L^*,L\phi=\phi L=\phi$，注意区别于第 2 点.

### 语言的有穷表示

计算理论的一个核心问题是如何用有穷的规定表示语言. 对于有穷语言我们列举即可，但无穷语言需要更好的表示方法. 有穷表示有以下两个原则；

1. 有穷表示本身是一个字符串；
2. 要求不同语言有不同的表示.

但我们发现，因为有穷表示是字符串，因此只有 $|\Sigma^*|$ 种表示，我们在之前已经证明 $\Sigma^*$ 是可数无限集，而所有语言构成 $\Sigma^*$ 的幂集（即全部子集构成的集合）$2^{\Sigma^*}$，我们可以证明这是不可数无限集（与 $2^\mathbf{N}$ 类似），故一定存在无法被有穷表示的语言，这是计算理论这门课的第一个重要结果.

我们先来看一个例子：

**例**：令 $L=\{w\in\{0,1\}^*:w$ 中 1 出现两次或三次，且第一、二次不相邻$\}$. 这个语言可以只用单元集和符号 $\cup$、$\circ$ 和 * 描述为

$$\{0\}^*\circ\{1\}\circ\{0\}^*\circ\{0\}\circ\{1\}\circ\{0\}^*((\{1\}\circ\{0\}^*)\cup \phi^*)$$

一定注意 $^*$ 表示 0 或多个元素连接的结果. 请务必理解最后一个连接的含义，注意回忆语言连接的定义，最后一个连接表明在 0 个或多个  0 后连接 100$\cdots$ **或者**空串都是这个新的语言中的元素.

以上表示可以简化为 $0^*10^*010^*(10^*\cup\odot^*)$，我们称这种类型的表达式为**正则表达式**. 我们接下来给出正则表达式的规范定义：

字母表 $\Sigma$ 上的正则表达式是字母表 $\Sigma\cup\{(,\ ),\ \odot,\ \cup,\ *\}$ 上可以如下获得的所有字符串：

1. $\odot$（空元，对应语言的空集）和 $\{x\}(\forall x\in\Sigma)$ 是正则表达式；
2. 若 $\alpha$ 和 $\beta$ 是正则表达式，则 $(\alpha\beta),(\alpha\cup\beta),\alpha^*$ 是正则表达式；
3. 其他表达式都不是正则表达式.

注意上述符号中括号是拆成左括号和右括号，中间逗号隔开，而不是 $(,)$ 代表一个符号.

上述符号 $\cup$ 我们可以解释为集合的并，$*$ 可以解释为 Kleene Star，表达式的并列视为连接. 因此，上述符号都可以赋予语言运算的含义，于是我们可以用函数 $L$ 建立起正则表达式和它们表示的语言的连接，如果 $\alpha$ 是正则表达式，则 $L(\alpha)$ 是 $\alpha$ 表示的语言. 

请注意不要将正则表达式和语言混淆，正则表达式根据定义只是一个字符串，但我们可以通过函数 $L$ 得到其表达的语言. 从字符串到语言的函数 $L$ 定义如下：

1. $L(\odot)=\phi$，$L(a)=\{a\},\forall a\in\Sigma$；
2. 若 $\alpha$ 和 $\beta$ 是正则表达式，则 $L(\alpha\beta)=L(\alpha)L(\beta),L(\alpha\cup\beta)=L(\alpha)\cup L(\beta),L(\alpha^*)=L(\alpha)^*$.

**例**：求 $L(((a\cup b)^*a))$ 表示的语言.

$\begin{align}L(((a\cup b)^*a)) &= L((a\cup b)^*)L(a)=L((a\cup b))^*\{a\}\\&=(L(a)\cup L(b))^*\{a\}=(\{a\}\cup\{b\})^*\{a\}=(\{a,b\})^*\{a\}\\&=\{w\in\{a,b\}^*:w\textup{ end with an }a\}\end{align}$

解决此类问题，重要的是首先要看出正则表达式的结构，即我们可以如何按照上面的规则进行化简，理解时可以**适当拆除括号**，尽量让表达式看起来不那么复杂.

**例**：$(c^*(a\cup (bc^*))^*)$ 表示 $\{a,b,c\}$ 上不含子串 $ac$ 的所有字符串的集合. 不含子串 $ac$ 是显然的，所有子串原因在于，这个正则表达式的开头第一个 $a$ 之前可以由任意的含 $b,c$ 的子串组成，我们删去这一部分，接下来任意个 $c$ 都只能接在任意个 $b$ 后，因此能表达全部的此类字符串.

正则表达式有以下性质，可以理解记忆（特别是第 7、8 条，要理解其含义）：

1. $SR\neq RS$；
2. $S\cup R=R\cup S$；
3. $R(ST)=(RS)T$；
4. $R(S\cup T)=RS\cup RT,(R\cup S)T=RT\cup ST$；
5. $\odot^*=\{e\}$；
6. $(R^*)^*=R^*$；
7. $(R^*S^*)^*=(R\cup S)^*$；
8. $(\{e\}\cup R)^*=R^*$.

**Remark**

1. 每一个可以用正则表达式表示的语言都可以用无穷多个正则表达式表示，例如 $\alpha$ 和 $(\alpha\cup\odot)$ 总是表示相同的语言，上述性质 2-7 也可以用来等价转换；
2. 定义字母表 $\Sigma$ 上的**正则语言**（regular language）类为所有可以写成 $L=L(\alpha)$ 的语言 $L$ 组成，其中 $\alpha$ 是 $\Sigma$ 上任意正则表达式. 即正则语言是所有能用正则表达式描述的语言. $\Sigma$ 上的正则语言类恰好是语言集合 $\{\{\sigma\}:\sigma\in\Sigma\}\cup\{\phi\}$ 关于并、连接和 Kleene Star 的闭包（即空集或单个字母通过这些操作得到的字符串的集合）；
3. 正则表达式无法描述所有语言，是一种不充分的规定说明方法，例如 $\{0^n1^n:n\ge 0\}$ 无法被表示；
4. 两种语言有穷表示的类型：语言识别装置（recognition device，回答一个字符串是否属于某个语言的问题），语言生成器（generator，描述如何生成语言中的实例），二者关系也是这门课的一个重要课题.

总结：我们介绍了字母表、字符串以及语言的基本定义以及运算，表明语言是字符串的集合，并且证明了不是所有的语言都能被有穷表示. 我们介绍了一种基本但不充分的表示形式，即正则表达式，正则表达式与语言之间通过符号与集合操作的函数关系联系起来.