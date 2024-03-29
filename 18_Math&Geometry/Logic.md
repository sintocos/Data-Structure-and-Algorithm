## 逻辑推理

1.对于逻辑推理类问题，最重要的就是抽象和数学建模。

2.有时候考虑计算机科学的一些数据结构来对问题进行转化，状态树什么的比较常用。

3.信息论和概率论的应用也非常常见。

4.有时候需要逆转问题，从结果开始，

5.计算机里面的算法也比较常见，动态规划、树剪枝什么的。



### 1.高楼扔鸡蛋

有100层楼，如果一个鸡蛋从N层以上(包含N层)扔下来就会破，但是从N层以下扔下来就不会破。现在提供**两个**鸡蛋，需要找出N的取值。要求在最差情况下需要猜的次数即扔的次数最少。

**参考**：注意不能直接二分，求得是最差的情况下的次数最少。方法是求x(x+1)/2=N，x向上取整即为答案。扩展性的题目可以**参考**LeetCode 887. Super Egg Drop。具体的解答可以**参考**[这个链接](https://www.zhihu.com/question/19690210/answer/514119129)。



### 2.用天平找不同

给一架天平和12个球，12个球里面有一个球的重量和其他不一样。

**参考**：编号，然后分成3堆，用其中两堆放在天平上称，然后分情况讨论。一共要称三次。有的题目还要求确定那个球是偏轻还是偏重，方法也是类似的。这种题目非常普遍，网上有更多解法。此外，可从信息论的角度去思考。



### 3.特定体积的水

假设有一个池塘，里面有无穷多的水。现有2个空水壶，容积分别为5升和6升。问题是如何只用这2个水壶从池塘里取得3升的水。

**参考**：先设定状态为00，然后可以据此得到三种后续状态50，06，56。然后据此演化出一棵状态数，注意不能出现前面已经出现的状态，要剪枝。从而可以得到状态53，其路径是00->06->51->01->10->16->52->02->20->26->53。其对应的操作是：由满6向空5倒，剩1升，把这1升倒5里，然后6盛满，倒5里面，由于5里面有1升水，因此6只能向5倒4升水，然后将6剩余的2升，倒入空的5里面，再灌满6向5里倒3升，剩余3升。



### 4.移动水杯

周雯的妈妈是豫林水泥厂的化验员。一天，周雯来到化验室做作业。做完后想出去玩。"等等，妈妈还要考你一个题目，"她接着说，"你看这6只做化验用的玻璃杯，前面3只盛满了水，后面3只是空的。你能只移动1只玻璃杯，就便盛满水的杯子和空杯子间隔起来吗?"爱动脑筋的周雯，是学校里有名的"小机灵"，她只想了一会儿就做到了。请你想想看，"小机灵"是怎样做的?
**参考**：设杯子编号为ABCDEF，ABC为满，DEF为空，把B中的水倒进E中即可。



### 5.三人决斗

三个小伙子同时爱上了一个姑娘，为了决定他们谁能娶这个姑娘，他们决定用手枪进行一次决斗。小李的命中率是30％，小黄比他好些，命中率是50％，最出色的枪手是小林，他从不失误，命中率是100％。由于这个显而易见的事实，为公平起见，他们决定按这样的顺序：小李先开枪，小黄第二，小林最后。然后这样循环，直到他们只剩下一个人。那么这三个人中谁活下来的机会最大呢？他们都应该采取什么样的策略？

**参考**：基本思路是构建一个状态树，分析计算各种情况下的概率，选择最优的。算出来的结果是小李的第一枪朝天开，后面的则是谁还活着就朝谁开。而小黄第一枪是朝小林开，后续则是谁活着朝谁开。小林第一枪朝小黄开，后续则是谁或者朝谁开。最后李、黄、林存活率约38:27:35。



### 6.三人分配

一间囚房里关押着两个犯人。每天监狱都会为这间囚房提供一罐汤，让这两个犯人自己来分。起初，这两个人经常会发生争执，因为他们总是有人认为对方的汤比自己的多。后来他们找到了一个两全其美的办法：一个人分汤，让另一个人先选。于是争端就这么解决了。可是，现在这间囚房里又加进来一个新犯人，现在是三个人来分汤。必须寻找一个新的方法来维持他们之间的和平，该怎么办呢？

**参考**：让甲分汤，分好后由乙和丙按任意顺序给自己挑汤，剩余一碗留给甲。这样乙和丙两人的总和肯定是他们两人可拿到的最大，然后将乙丙挑的汤混合，再按两人的方法再次分汤。这更偏心理问题而不仅仅是逻辑问题。



### 7.两人猜牌

S先生、P先生、Q先生他们知道桌子的抽屉里有16张扑克牌：红桃A、Q、4；黑桃J、8、4、2、7、3；草花K、Q、5、4、6；方块A、5。约翰教授从这16张牌中挑出一张牌来，并把这张牌的点数告诉P先生，把这张牌的花色告诉Q先生。这时，约翰教授问P先生和Q先生：你们能从已知的点数或花色中推知这张牌是什么牌吗？

P先生：我不知道这张牌。Q先生：我知道你不知道这张牌。

P先生：现在我知道这张牌了。Q先生：我也知道了。请问：这张牌是什么牌？

**参考**：先分析第一句话，P说不知道这张牌，说明该点数对应至少两张牌，从而可以排除那些点数只出现了一次的牌。从而候选集是红桃AQ4、黑桃4、草花Q54、方块A5。

再来分析第二句话，Q说他知道P无法确认这张牌，说明花色对应的那一堆牌里面没有那种点数只出现一次的牌，即花色要么是红桃要么是方块，从而候选集为红桃AQ4、方块A5。

再来看第三句话，P说确定了这张牌，说明在第二句话的信息的前提下得到的候选集里面，P所持有的点数只出现一次，从而候选集为红桃Q4、方块5。在这里面P结合自己手上的点数就知道了这张牌。

再来看第四句话，Q在第三句话的信息下也知道了，说明在第三句话的候选集下面，他手上的花色只对应一张牌，从而只能是方块5，其他情况全部排除。所以这张牌就是方块5。



### 8.三人猜数

一个逻辑学的教授有三个学生，而且三个学生均非常聪明。一天教授给他们出了一个题，教授在每个人脑门上贴了一张纸条并告诉他们，每个人的纸条上都写了一个正整数，且某两个数的和等于第三个！（每个人可以看见另两个数，但看不见自己的）教授问第一个学生：你能猜出自己的数吗？回答：不能，问第二个，不能，第三个，不能，再问第一个，不能，第二个，不能，第三个：我猜出来了，是144！教授很满意的笑了。请问您能猜出另外两个人的数吗？

**参考**：这种题一般意味着信息被人完全获取，接下来就是不断利用新的信息来进行排除。设三个人分别是A、B、C，对应的数字是X、Y、Z。在发生对话之前，A所能知道的是Y和Z，B所能知道的是X和Z，C能知道的是X和Y，此外就是X、Y、Z均为正整数。现在一共有三种情况X+Y=Z、X+Z=Y、Y+Z=X。对于每个人来说，他们都只知道自己的数是另外两个数之差或者之和，即每个人心中有两个候选数字。下面来分析这些回答。

第一轮，A首先说不知道，说明A无法确定是哪一种情况，说明Y和Z不相等，因为如果Y=Z，则结合三者均为正整数，能推出X=Y+Z。接下来B也不知道，类似于A，B提供了信息：X和Z不等。同样的，C的回答提供了X与Y不等。

经过第一轮回答，现在的情形是三人都知道X、Y、Z三个数都不相同。

第二轮，A说不知道，说明Y不是Z的两倍，Z也不是Y的两倍，如果是的话，结合三个数不同，A就知道X是Y和Z的和。类似的，B的回答也提供了信息：X不是Z的两倍，Z也不是X的两倍。结合三数不同，可知X、Y、Z两两之间并非2倍关系。

现在就有三个信息：1，每个数均为正整数；2，三个数都不同；3，三个数两两之间并非2倍关系。

C接下来猜出了144，而C心中只有两个候选：要么是X和Y的和，要么是X和Y的差。说明他根据上面的三个信息排除了一种候选情况。

假设C排除的是第一种情况，即C猜到了是两者之差，从而X-Y=144。这时上面的三个信息前面的两个都可以成立，说明C利用了第三个信息使得Z=X+Y不成立。这说明他看到的X和Y满足X+Y = 2*Y，即X=Y，而这不成立，因为如果成立的话，第一轮就可以得出了。

假设C排除的是第二种情况，即C猜到了Z=X+Y。那么要排除第二种情况，就要使得信息3不成立，即看到的X和Y满足X-Y=2*Y。从而有X+Y=144, X-Y=2 *Y。故X=108,Y=36,Z=144。

现在来回顾一下，假设我们是C，我们是如何猜出144的。C看到的是A的108和B的36，自己心中的候选答案是144或者72。

假设自己是72的话，那么B在第二回合就可以看出：B看到的是A的36和C的72，那么B就可以猜自己是36或者是108。如果假设B是36，那么C在第一回合的时候就可以看出来。

下面是如果B是36，C的思路：C看到的就是A的36和B的36，那么他就可以猜自己，是72或者是0。如果假设C是0，那么，A在第一回合的时候就可以看出来。

下面是如果C是0，A的思路：这种情况下，A看到的就是B的36和C的0，那么他就可以猜自己是36，那他可以一口报出自己头上的36。

接下来不断逆推，A在第一回合没报出自己的36，C(在B的想象中)就可以知道自己不是0，如果其他和B的想法一样(指B头上是36)，那么C在第一回合就可以报出自己的72。现在C在第一回合没报出自己的72，B(在C的想象中)就可以知道自己不是36，如果其他和C的想法一样(指C头上是72)，那么B在第二回合就可以报出自己的108。现在B在第二回合没报出自己的108，C就可以知道自己头上不是72，那么C头上的唯一可能就是144了。



### 9.肇事概率

某城市发生了一起汽车撞人逃跑事件，该城市只有两种颜色的车,蓝15%绿85%，事发时有一个人在现场看见了，他指证是蓝车，但是根据专家在现场分析,当时那种条件能看正确的可能性是80%。那么,肇事的车是蓝车的概率到底是多少?

**参考**：贝叶斯概率和全概率公式的应用，设X表示肇事的车的颜色，Y表示指证的颜色，那么所求的是：

P(X=blue|Y=blue) = P(X=blue, Y=blue) / P(Y=blue) = P(X=blue)* P(Y=blue|X=blue) / P(Y=blue)，

而用全概率公式可以得到 P(Y=blue) = P(Y=blue|X=green)* P(X=green) + P(Y=blue|X=blue)* P(X=blue)

代入，可以得到所求的概率P(X=blue|Y=blue) = 0.15 * 0.8 / (0.2 * 0.85 + 0.8 * 0.15) = 12/29



### 10.沙漠卖水

有一人有240公斤水，他想运往干旱地区赚钱。他每次最多携带60公斤，并且每前进一公里须耗水1公斤（均匀耗水）。假设水的价格在出发地为0，之后与运输路程成正比，即在10公里处为10元/公斤，在20公里处为20元/公斤，依次下去，又假设他必须安全返回，请问，他最多可赚多少钱？
**参考**：假设水是分散卖的，设最后一个卖水的地点是x公里，整个行程耗水量固定是2 * x公斤，如果在x公里前售卖，还不如直接在x公里处售卖。所以水是直接在终点卖出去，然后返回。又因为消耗水只和距离有关，从而尽量带多点水，故每次带60公斤。从而可售卖的水的量乘以水的价格，有f(x)=(60-2 * x) * x，可以得到当x=15时，f(x)有最大值450元。又因为一共有240公斤水，故最多可赚1800元。



### 11.排队看电影

有2n个人排队进电影院，票价是50美分。在这2n个人当中，其中n个人只有50美分，另外n个人有1美元（纸票子）。愚蠢的电影院开始卖票时1分钱也没有。问：有多少种排队方法使得每当一个拥有1美元买票时，电影院都有50美分找钱。
注：1美元=100美分拥有1美元的人，拥有的是纸币，没法破成2个50美分。
**参考**：从题目中可以得出，手持1美元的人才需要找50美分，即，当手持1美元的人到达售票处时，售票处至少需要一张50美分的钱。即，从队首开始往后数，任何时候，只要手持1美元的人比手持50美分的人少，肯定可以。

这个问题可以联想到括号匹配的问题。假设50美分的人是左括号，1美元的人是右括号，要求任意一个左括号都有一个右括号和其对应。故可行的排队方法就意味着合法的括号排列。

可以考虑用栈来解决，假设存在一个栈，遍历由n个左括号和n个右括号排成的队列，每次如果是左括号，则将其压入栈中，如果是右括号，则让栈尾的左括号出栈。如果始终能保证栈中有足够的左括号，那么该排列就是一个有效的排列。因此，可以做这样的分析，第一个括号一定是左括号，否则该队列肯定出错。假设第一个左括号和第k个符号匹配。那么从第2个符号到第k-1个符号、第k+1个符号到第2n个符号也都是一个合法的括号序列。从而，k是奇数，否则，第2个符号到第k-1个符号就只有奇数个符号，不合法。从而，可以设k=2 * i + 1，i=0,1,...,n-1。假设2n个符号中合法的括号序列的个数是f(2 * n)，若第一个左括号和第k=2 * i +1 个右括号匹配，则剩余括号的合法序列是f(2 * i) * f(2n - 2i -2)个。从而有下面的递推式：f(2n)=sum_(i=0到n-1) (  f(2i) * f(2n-2i-2) ) 。其中f(0)=1。这样，可以用O(n* n)的时间求出答案。甚至，进一步的，可以得到通项公式，为f(2n) = (2n)!/[n!(n+1)!]。具体可参考《编程之美》4.3章。



### 12.身份分析

5个人来自不同地方，住不同房子，养不同动物，吸不同牌子香烟，喝不同饮料，喜欢不同食物。根据以下线索确定谁是养猫的人。
1． 红房子在蓝房子的右边，白房子的左边（不一定紧邻）
2． 黄房子的主人来自香港，而且他的房子不在最左边。
3． 爱吃比萨的人住在爱喝矿泉水的人的隔壁。
4． 来自北京的人爱喝茅台，住在来自上海的人的隔壁。
5． 吸希尔顿香烟的人住在养马人的右边隔壁。
6． 爱喝啤酒的人也爱吃鸡。
7． 绿房子的人养狗。
8． 爱吃面条的人住在养蛇人的隔壁。
9． 来自天津的人的邻居（紧邻）一个爱吃牛肉，另一个来自成都。
10．养鱼的人住在最右边的房子里。
11．吸万宝路香烟的人住在吸希尔顿香烟的人和吸“555”香烟的人的中间（紧邻）
12．红房子的人爱喝茶。
13．爱喝葡萄酒的人住在爱吃豆腐的人的右边隔壁。
14．吸红塔山香烟的人既不住在吸健牌香烟的人的隔壁，也不与来自上海的人相邻。
15．来自上海的人住在左数第二间房子里。
16．爱喝矿泉水的人住在最中间的房子里。
17．爱吃面条的人也爱喝葡萄酒。
18．吸“555”香烟的人比吸希尔顿香烟的人住的靠右





### 13.过桥

U2合唱团在17分钟内得赶到演唱会场，途中必需跨过一座桥，四个人从桥的同一端出发，你得帮助他们到达另一端。天色很暗，而他们只有一只手电筒。一次同时最多可以有两人一起过桥，而过桥的时候必须持有手电筒，所以就得有人把手电筒带来带去，来回桥的两端。手电筒是不能用丢的方式来传递的。

四个人的步行速度各不同，若两人同行则以较慢者的速度为准。Bono需花1分钟过桥，Edge需花2分钟过桥，Adam需花5分钟过桥，Larry需花10分钟过桥。他们要如何在17分钟内过 桥呢？
**参考**：考虑用状态图表示，设初始时，四个人的状态都是0000，全都过桥后是1111，然后用t表示时间。故最开始是一个五元组(0,0,0,0,0)，最终目标是(1,1,1,1,x)，x不大于17。可以依次画一个状态数，如果t超过17就剪枝。

直接来分析的话，考虑10和5最大，他们两个最好一起过，用10来覆盖5的时间花费。然后要人来送手电筒，或者要人来取手电筒，故可用以下方案：一、2＋1先过，消耗时间2，二、1回来送手电筒，消耗时间1，三、5＋10再过，消耗时间10，四、2回来送手电筒，消耗时间2，五、2＋1过去，消耗时间2，总2＋1＋10＋2＋2＝17分钟。





### 14.芯片测试

有2k块芯片，已知好芯片比坏芯片多．请设计算法从其中找出一片好芯片，说明你所用的比较次数上限。其中好芯片和其它芯片比较时，能正确给出另一块芯片是好还是坏，坏芯片和其它芯片比较时，会随机的给出好或是坏。
**参考**：把第一块芯片与其它逐一对比，看看其它芯片对第一块芯片给出的是好是坏，如果给出是好的过半，那么说明这是好芯片，完毕。如果给出的是坏的过半，说明第一块芯片是坏的，那么就要在那些在给出第一块芯片是坏的芯片中，重复上述步骤，直到找到好的芯片为止。



### 16.考试答题

100个人回答五道试题，有81人答对第一题，91人答对第二题，85人答对第三题，79人答对第四题，74人答对第五题，答对三道题或三道题以上的人算及格， 那么在这100人中，至少有多少人及格。
**参考**：每道题的答错人数为（次序不重要）：26，21，19，15，9
第3分布层：答错3道题的最多人数为：（26+21+19+15+9）/3=30
第2分布层：答错2道题的最多人数为：（21+19+15+9）/2=32
第1分布层：答错1道题的最多人数为：（19+15+9）/1=43
Max_3=Min(30, 32, 43)=30。因此答案为：100-30=70。
其实，因为26小于30，所以在求出第一分布层后，就可以判断答案为70了。
要让及格的人数最少，就要做到两点：

1. 不及格的人答对的题目尽量多，这样就减少了及格的人需要答对的题目的数量，也就只需要更少的及格的人
2. 每个及格的人答对的题目数尽量多，这样也能减少及格的人数
   由1得每个人都至少做对两道题目
   由2得要把剩余的210道题目分给其中的70人： 210/3 = 70，让这70人全部题目都做对，而其它30人只做对了两道题
   也很容易给出一个具体的实现方案：
   让70人答对全部五道题，11人仅答对第一、二道题，10人仅答对第二、三道题，5人答对第三、四道题，4人仅答对第四、五道题
   显然稍有变动都会使及格的人数上升。所以最少及格人数就是70人！
   
   
   
   

### 17.猫捉老鼠

一个巨大的圆形水池，周围布满了老鼠洞。猫追老鼠到水池边，老鼠未来得及进洞就掉入水池里。猫继续沿水池边缘企图捉住老鼠（猫不入水）。已知V猫=4V鼠。问老鼠是否有办法摆脱猫的追逐？
**参考**：第一步：游到水池中心。
第二步：从水池中心游到距中心R/4处，并始终保持鼠、水池中心、猫在一直线上。
第三步：沿与中心相反方向的直线游3R/4就可以到达水池边，而猫沿圆周到达那里需要3.14R，所以捉不到老鼠。

类似的问题可参考https://www.zhihu.com/question/21216628 以及 https://www.zhihu.com/question/54727413。





### 18.钟表时间

从前有一位老钟表匠，为一个教堂装一只大钟。他年老眼花，把长短针装配错了，短针走的速度反而是长针的12倍。装配的时候是上午6点，他把短针指在“6 ”上，长针指在“12”上。老钟表匠装好就回家去了。人们看这钟一会儿7点，过了不一会儿就8点了，都很奇怪，立刻去找老钟表匠。等老钟表匠赶到，已经是下午7点多钟。他掏出怀表来一对，钟准确无误，疑心人们有意捉弄他，一生气就回去了。这钟还是8点、9点地跑，人们再去找钟表匠。老钟表匠第二天早晨8点多赶来用表一对，仍旧准确无误。请你想一想，老钟表匠第一次对表的时候是7点几分？第二次对表又是8点几分？
**参考**：7点x分：(7+x/60)/12=x/60 x=7 * 60=420/11=38.2
第一次是7点38分，第二次是8点44分



### 19.分硬币

有23枚硬币在桌上，10枚正面朝上。假设别人蒙住你的眼睛，而你的手又摸不出硬币的反正面。让你用最好的方法把这些硬币分成两堆，每堆正面朝上的硬币个数相同。
**参考**：分成10＋13两堆， 然后翻转10的那堆。



### 20.海盗分配

5名海盗抢得了窖藏的100块金子，并打算瓜分这些战利品。这是一些讲民主的海盗（当然是他们自己特有的民主），他们的习惯是按下面的方式进行分配：最厉害的一名海盗提出分配方案，然后所有的海盗（包括提出方案者本人）就此方案进行表决。如果50%或更多的海盗赞同此方案，此方案就获得通过并据此分配战利品。否则提出方案的海盗将被扔到海里，然后下一名最厉害的海盗又重复上述过程。所有的海盗都乐于看到他们的一位同伙被扔进海里，不过，如果让他们选择的话，他们还是宁可得一笔现金。他们当然也不愿意自己被扔到海里。所有的海盗都是有理性的，而且知道其他的海盗也是有理性的。此外，没有两名海盗是同等厉害的——这些海盗按照完全由上到下的等级排好了座次，并且每个人都清楚自己和其他所有人的等级。这些金块不能再分，也不允许几名海盗共有金块，因为任何海盗都不相信他的同伙会遵守关于共享金块的安排。这是一伙每人都只为自己打算的海盗。最凶的一名海盗应当提出什么样的分配方案才能使他获得最多的金子呢？
**参考**：如果轮到第四个海盗分配：100，0
轮到第三个：99，0，1
轮到第二个：98，0，1，0
轮到第一个：97，0，1，0，2，这就是第一个海盗的最佳方案。



### 21.囚犯分配

他们中谁的存活机率最大？
5个囚犯，分别按1-5号在装有100颗绿豆的麻袋抓绿豆，规定每人至少抓一颗，而抓得最多和最少的人将被处死，而且，他们之间不能交流，但在抓的时候，可以摸出剩下的豆子数。问他们中谁的存活几率最大？提示：　　　　　　
1，他们都是很聪明的人　　　　　　
2，他们的原则是先求保命，再去多杀人　　　　　　
3，100颗不必都分完　　　　　　
4，若有重复的情况，则也算最大或最小，一并处死
**参考**：1号选择17时是最优的，有先动优势。他确实有可能被逼死，后面的2、3、4号也想把1号逼死，但做不到（起码确定性逼死做不到）。可以看一下，如果1号选择21，他的信息是暴露给2号的，那么1号就将自己暴露在一个非常不利的环境下，2-4号就会选择20，5号就会被迫在1-19中选择，则1、5号处死。所以1号不会这样做，会选择一个更小的数。下面讨论的就是1号会选择一个什么数，他仍然不会选择一个太大或太小的数，因为那样仍然是自己处于不利的地位（2-4号肯定不会留情面的），100/6=16.7（为什么除以6？因为5号会随机选择一个数，对1号来说要尽可能的靠近中央，2-4号也是如此，而且正因为2-4号如此，1号才如此），最终必然是在16、17中选择的问题。
对16、17进行概率的计算之后，就得出了3个人选择17，第四个人选择16时，为均衡的状态，第4号虽然选择16不及前三个人选择17生存的机会大，但是若选择17则整个游戏的人必死（包括他自己）！第3号没有动力选择16，因为计算概率可知生存机会不如17。所以选择为17、17、17、16、X（1-33随机），1-3号生存机会最大。



### 22.卖萝卜

一个商人骑一头驴要穿越1000公里长的沙漠，去卖3000根胡萝卜。已知驴一次性可驮1000根胡萝卜，但每走一公里又要吃掉一根胡萝卜。问：商人共可卖出多少胡萝卜？
**参考**：商人带驴驮1000根胡萝卜，先走250公里，这时，驴已吃250根，放下500根，原地返回，又吃掉250根。商人再带驴驮1000根胡萝卜，走到250公里处，这时，驴已吃250根，再驮上原先放的500根中的250根，继续前行至500公里处，这时，驴又吃250根，放下500根，剩250根返回250公里处，在驮上250公里处剩下的250根返回原地，这时驴又吃250根。商人再带驴驮1000根胡萝卜，走到500公里处，这时，驴已吃500根，再驮上原先放的500根，走出沙漠，驴吃掉500根，还剩500根。



### 23.黄金查缺

10箱黄金，每箱100块，每块一两。有贪官，把某一箱的每块都磨去一钱。请称一次找到不足量的那个箱子。

**参考**：第一箱子拿1块，第二箱子拿2块， 第n箱子拿n块，然后放在一起称，看看缺了几钱，缺了n钱就说明是第n个箱子。



### 24.药物查缺

有十瓶药，每瓶里都装有100片药，其中有八瓶里的药每片重10克，另有两瓶里的药每片重9克。用一个蛮精确的小秤，只称一次，如何找出份量较轻的那两个药瓶？
**参考**：与众不同的瓶子有两个，只称一次的话，只能得到两个瓶子所缺的克数的总和，我们必须保证能从总和中唯一地得出两个瓶子的所缺数。第一个瓶可拿出1片，第二个拿2片，第三个拿3片，但第四个不能拿4片，因为如果结果缺了5克的话，你就不知道是缺了2+3还是1+4。所以第四个应拿5片，第五个应拿8片，第n个应拿a(n-1)+a(n-2)片。



### 25.黄金分割

你让工人为你工作７天，给工人的回报是一根金条。金条平分成相连的７段，你必须在每天结束时都付费，如果只许你两次把金条弄断，你如何给你的工人付费？
**参考**：把金条分成1，2，4三段。第一天给工人1，第二天给工人2，工人找回1，第三天再给工人1，第四天给工人4，工人找零钱找回1和2，第五天再给工人1，第六天给工人2，工人找回1，第七天给工人1。



### 26.年龄分析

一个经理有三个女儿， 三个女儿的年龄加起来等于13，三个女儿的年龄乘起来等于经理自己的年龄，有一个下属已知道经理的年龄，但仍不能确定经理三个女儿的年龄，这时经理说只有，一个女儿的头发是黑的，然后这个下属就知道了经理三个女儿的年龄。请问三个女儿的年龄分别是多少？为什么？
**参考**：显然3个女儿的年龄都不为0，要不爸爸就为0岁了，因此女儿的年龄都大于等于1岁。这样可以得下面的情况：1* 1* 11=11，1* 2* 10=20，1* 3* 9=27，1* 4* 8=32，1* 5* 7=35，1* 6* 6=36，2* 2* 9=36，2* 3* 8=48，2* 4* 7=56，2* 5* 6=60，3* 3* 7=63，3* 4* 6=72，3* 5*  5=75，4* 4* 5=80。因为下属已知道经理的年龄，但仍不能确定经理三个女儿的年龄，说明经理是36岁(因为有两种情况)，所以3个女儿的年龄只有2种情况，经理又说只有一个女儿的头发是黑的，说明只有一个女儿是比较大的，其他的都比较小，头发还没有长成黑色的，所以3个女儿的年龄分别为2、2、9。



### 27.圆环转圈

两个圆环，半径分别是1和2，小圆在大圆内部绕大圆圆周一周，问小圆自身转了几周？如果在大圆的外部，小圆自身转几周呢？
**参考**：直观的想法是把大圆剪断拉直。小圆绕大圆圆周一周，就变成从直线的一头滚至另一头。因为直线长就是大圆的周长，是小圆周长的2倍，所以小圆要滚动2圈。但是现在小圆不是沿直线而是沿大圆滚动，小圆因此还同时作自转，当小圆沿大圆滚动1周回到原出发点时，小圆同时自转1周。当小圆在大圆内部滚动时自转的方向与滚动的转向相反，所以小圆自身转了1周。当小圆在大圆外部滚动时自转的方向与滚动的转向相同，所以小圆自身转了3周。

一种简单的方法是使用这个公式：两圆圆心距/转动者半径=转动者切另一圆时的自转数。

简而言之，小圆所转动的路程便是小圆圆心走过的距离。所以在大圆外部转动，小圆圆心饶半径为3的大圆心转动，走了三圈。而在大圆内部转动，小圆圆心饶半径为1的大圆心转动，走了一圈。



### 28.多喝汽水

 1元钱一瓶汽水，喝完后两个空瓶换一瓶汽水，问：你有20元钱，最多可以喝到几瓶汽水？
**参考**：40瓶，20+10+5+2+1+1=39， 这时还有一个空瓶子，先向店主借一个空瓶，换来一瓶汽水喝完后把空瓶还给店主。



### 29.拿球游戏

假设排列着100个乒乓球，由两个人轮流拿球装入口袋，能拿到第100个乒乓球的人为胜利者。条件是：每次拿球者至少要拿1个，但最多不能超过5个，问：如果你是最先拿球的人，你该拿几个？以后怎么拿就能保证你能得到第100个乒乓球？
**参考**：首先拿4个，别人拿n个，你就拿6－n个。



### 30.飞机加油

每个飞机只有一个油箱， 飞机之间可以相互加油（注意是相互，没有加油机） 一箱油可供一架飞机绕地球飞半圈，问题：为使至少一架飞机绕地球一圈回到起飞时的飞机场，至少需要出动几架飞机？（所有飞机从同一机场起飞，而且必须安全返回机场，不允许中途降落，中间没有飞机场）



### 31.时钟的重合

在一天的24小时之中，时钟的时针、分针和秒针完全重合在一起的时候有几次？都分别是什么时间？你怎样算出来的？



### 32.盲人分牌

一个盲人面对桌面上七张纸牌，三张正面朝上，四张背面朝上，在没有人帮助的情况下，怎样把这堆牌分成两叠，每叠正面朝上的牌数相同？
**参考**： 设1表示正面朝上，0表示背面朝上。一开始的牌面状况为1110000。那么分牌策略为：
第一步，盲人将牌任意分为**三张一堆和四张一堆**，会有以下四种情况：
1) 111/0000
2) 110/1000
3) 100/1100
4) 000/1110
第二步，**将三张的牌堆里面的牌全部翻面**，则对应上面四种情况的结果为：
1) 000/0000(两堆0张正面朝上)
2) 001/1000(两堆各有1张正面朝上)
3) 011/1100(两堆各有2张正面朝上)
4) 111/1110(两堆各有3张正面朝上)
实现目标。


