## 博弈论、理性和智能性
从本讲起我们进入博弈论（game theory）的讨论。博弈论可以被定义为对**智能**的**理性**决策者之间冲突与合作的数学模型研究。博弈论为分析那些涉及两个或多个参与者且其决策会影响相互间福利水平的情况提供了一般性的数学方法。接下来的内容我们主要参考如下教材：

- [1]《博弈论：矛盾冲突分析》[美] 罗杰·迈尔森
- [2]《博弈论》[以] 迈克尔·马希勒，埃隆·索兰，什穆埃尔·扎米尔

当然我们也会参考一些其它的论文或者讲义，届时我们会在文中列出参考的材料。下面我们回到博弈论的基本概念。前面我们给出了博弈论的一个定义，当然事实上这一定义并非数学性质的严格定义，因此不同的人可能会有不同的描述方式，但基本的博弈定义都会包含如下要素：

1. **参与人**（player）：博弈的参与主体；
2. **策略**（strategy）：参与人的行动选择；
3. **效用**（utility）：也就是我们前面所说的“福利水平”。

注意到以上只是定义博弈的几个基本要素（之后可能还会见到其它的要素，目前最简单的版本就是这些要素），形成博弈我们还有人如下几个要求需要满足：

1. 多个参与者之间的策略选择是**互相影响**的。首先是必须有多个参与者，这一点无需过多解释。其次是参与者之间的行动选择是影响相互的福利的，例如下象棋、打扑克都是符合这一点的，但是很多人一起在考场上完成试卷虽然也是多人参与，但是他们的行动选择并不会影响到他人的福利，因此这种情况并不是博弈；
2. 参与者的行动选择是**理性**（rational）的。理性的含义是，参与人做出的决策与其所追求的目标是一致的；在我们接下来即将介绍的贝叶斯决策理论中，每个人的目标是效用最大化，因此理性人的决策是使得自己的效用最大化的行动；
3. 参与者是**智能**（intelligent）的，因此参与人需要对博弈有一个全局的判断。这一点可以用前面介绍的微观经济学中的价格理论来进行对比，在一般均衡模型中，每个人都是追求个体效用最大化的理性决策者，但每个人都不需要知道市场的完整结构就能做出决策；但在象棋或者扑克游戏中，我们需要对整个博弈的情况有一个全局的认识，并基于此做出决策。

一个自然的批评是，这些要求看起来有些苛刻：我们要求博弈中的参与者都是理性和智能的，这显然在现实中无法实现。这的确是一个问题，但我们以后介绍的一些例子将会体现出近似现实的特点，并且很多博弈理论也在现实中广泛应用；另一方面，我们也应该怀疑那些不符合这些要求的博弈模型的有效性，如果一个理论预测，某些人将经常被愚弄或者做出代价极高的错误行动，埃米尔这些人应当对整个博弈有更好的理解，因此前面这些理论也就会逐渐失效。博弈论在社会科学中的重要性很大程度上就来源于这样一个事实。

## 决策理论的基本概念
博弈论的逻辑根源在于贝叶斯决策理论(Bayesian decision theory)。事实上，博弈论可以被看作是决策理论（对两个或两个以上决策者情形）的一种推广，或者作为决策理论在本质上的逻辑完备。因此，要理解博弈论的根本思想，我们应该从研究决策理论开始。本章余下部分将集中介绍贝叶斯决策理论基本思想，这里从期望效用最大化定理的一般推导及其相关结论开始。

我们的目标是将博弈中一个人的行为做简化，事实上在之前的微观经济学部分我们已经研究过类似的问题，我们的手段是通过公理刻画人的偏好，然后证明满足某些性质的偏好可以被表示为效用函数，这样每个理性人的行为就被简化为了效用函数的最大化。在博弈论中，我们也会采用类似的方法，只是这里我们需要考虑不确定性下的决策问题，因此还需要一些新的模型来描述。

不确定性条件下的决策通常是用下述两个模型之一描述的：**概率模型**（probability model）和**状态变量模型**（state-variable model）。在每一种模型中，我们所说的决策者都是在彩票（lotteries）中进行选择的人，两者的区别在于其对彩票的定义不同：

- 概率模型适用于描述报酬依赖于具有明显客观概率的事件这一类赌博，我们称这样的事件为客观未知(objective unknowns)事件。这类赌博就是安斯库姆和奥曼(1963)的“轮盘彩票”(roulette lotteries)或奈特(Knight,1921)的“风险”(risks)。例如，掷一枚匀质的硬币、轮盘的自旋，或者从装有同样大小而颜色不同的球的瓮中随机地抽取一个球(各色球的总体已知)之类的赌博都可以用概率模型来加以描述。这里用到的一个重要假定是，就决策的目的而言，具有相同概率分布的两个客观未知事件是完全等价的。譬如说，如果用“获得100美元或0美元的彩金的概率均为1/2”来描述一个彩票，则我们在假定彩金
- 状态


## 贝叶斯条件概率系

我们从一系列的效用公理推导出了期望效用最大化定理。

!!! example "悖论"
    1. **完全性**：对于任意两个偏好的状态，我们可以比较它们的效用。
    2. **传递性**：如果我们更喜欢A而不是B，更喜欢B而不是C，那么我们更喜欢A而不是C。
    3. **连续性**：如果我们更喜欢A而不是B，那么存在一个足够小的$\epsilon$，使得我们在$[0, \epsilon]$的范围内更喜欢B而不是A。
    4. **凹性**：如果我们更喜欢A而不是B，那么对于任意的$\lambda \in [0, 1]$，我们都更喜欢$\lambda A + (1 - \lambda)B$而不是B。