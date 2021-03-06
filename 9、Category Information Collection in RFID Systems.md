## abstract
在启用RFID的应用中，当标签被投入使用并且与特定对象相关联时，关于该对象的类别相关信息（例如，服装的品牌）可以根据需要被预加载到标签的存储器中。由于这些信息反映了分类属性，所以同一分类中的所有标签都携带相同的分类信息。为了收集这些信息，我们不需要重复询问每个标签;一个标签在一个类别中的响应就足够了。在本文中，我们研究了多类RFID系统中类别信息收集的新问题，即信息抽样。我们提出了一个有效的两阶段采样协议（TPS）。通过快速放大到一个类别并从这个类别中分离出一个标签，TPS能够通过只广播7.5位轮询矢量（与96位标签ID相比非常有效）来对一个类别进行采样。我们从理论上分析协议性能，并讨论最小化总体执行时间的最佳参数设置。大量的仿真表明，TPS的性能优于基准测试，大大提高了采样性能。

## introduction
无线电频率识别（RFID）在各种应用中变得无处不在，包括图书馆库存[1]，[2]，仓库管理[3] - [5]，对象跟踪[6]，[7]等。这些应用中，RFID标签通常被附加到属于不同类别的对象，例如图书馆中的书籍主题，药房中的药品类别，或者服装店中的衣服品牌。当一个标签与一个特定的对象相关联时，关于这个object1的与分类有关的信息很可能被预加载到标签的内存中。由于该信息反映了类别属性，因此同一类别中的每个标签携带相同的类别相关信息。

为了在多类RFID系统中收集类别信息，我们不需要重复询问每个标签。一个标签在一个类别中的回应就足够了。例如，如果我们想知道储存在仓库里的天然有机牛奶的生产商，我们只需要查询一个牛奶盒而不是全部，因为这种牛奶是由同一个生产商生产的。在另一个例子中，考虑一个冷藏食物储藏室，其中每个食物被固定有配备有热传感器的传感器增强型RFID标签（例如，WISP [8]）。阅读器周期性地从标签采样温度读数来检查是否有任何区域超出正常温度。由于属于同一类别的标签对象通常被打包在一起或紧密放置，因此来自附近标签的温度报告导致高数据冗余。因此，在这种情况下收集来自所有标签的传感器信息是一种浪费。

在本文中，我们研究了多类RFID系统中的类别信息收集问题，即信息抽样。为了解决这个问题，现有的工作[9] - [11]需要收集所有标签的信息，或者每隔一个感兴趣的标签时都要考虑整个标签集，从而导致耗时的收集过程。主要原因是这些解决方案是针对某些特定的应用而设计的，而不是针对具有两个新特征的采样问题：（i）由于同一类别中的标签携带相同的类别相关信息，所以我们不需要查询每个单独的标签。每个类别的一个标签响应足以报告相应的信息。 （ii）我们不关心某个类别中的哪个标签对读者有反应;该类别中的任何人都可以成为报告的候选人

通过考虑上述两个特点，我们提出了一个由两个阶段组成的有效的两阶段采样协议（TPS）。在第一阶段，读者将一个类别与其他类别分开，这有助于我们快速放大一个类别，而不是整个标签集合。在第二阶段，读者通过使用标签的几何分布，从分离的类别中分离任意标签。有效的两个步骤使得TPS远远优于现有的解决方案。我们分析协议性能并讨论最小化总体执行时间的最佳参数设置。理论分析和仿真结果表明，TPC能够通过仅广播7.5比特轮询矢量来采样一个类别，与96比特标签ID相比非常有效。本文的其余部分安排如下。第二节阐述抽样问题。第三部分提出了一个双相抽样协议。第四节评估所提出的协议的性能。第五节介绍相关工作。最后，第六节总结本文。

## 阐述抽样问题
A. System Model
我们考虑一个由阅读器和一些标签组成的RFID系统。每个标签都有一个唯一的标签ID，它唯一代表与其关联的单个对象。标签ID包含两个组件：表示标签属于哪个类别的类别ID，以及标识该类别中的特定成员的成员ID。具有相同类别标识的标签共享一些共同的信息，可以是标签投入使用时预先加载的静态类别相关信息（例如标签产品的品牌），也可以是动态监测信息（如传感器数据）是由附近的标签实时测量的。我们将这些信息称为类别信息。注意类别ID和类别信息之间的差异。前者是标签ID的一个子集，后者是一类商品的共同属性。由于芯片上资源有限，标签之间不能互相通信，而是可以通过单跳传输与阅读器进行通信。
B. Problem Definition
假设N是RFID系统中预先设定的标签集，其中n = | N |。 根据类别ID，将N划分为一系列不相交集合C = {C1，C2，...，Cm}，满足m i = 1Ci = N。 为了简单起见，我们使用Ci来表示类别ID以及该类别中的标签集合。 有m = | C | 总共的类别; N中的每个标签都属于其中之一。 抽样问题是以一种有效的方式从每个类别中收集一个或多个比特类别信息。 由于类别中的信息是相同的，所以不需要从所有标签收集信息。 每个类别中的任意标签响应足以报告所需的信息。
## TPS
A. Baseline Protocol
基本轮询（BP）作为一种常见的防冲突协议，提供了一种请求 - 响应的方式来在RFID系统中每次选出一个标签。 在BP中，读卡器广播一个标签的ID; 所有的标签都在不停地收听，只有唯一匹配的标签才会回复给读者。 BP的显着优势在于询问请求和响应是一对一映射; 在开放的无线信道中不会发生碰撞。 然而，广播标签ID（EPCglobal Gen2标准中的96位长）用于轮询每个标签是耗时的。 我们引用广播位（来自读者）来选出一个标签作为轮询矢量。 显然，在BP中，每个标签的轮询矢量是96位标签ID。 我们把它作为比较的基准

B. Basic Idea
抽样问题的一个更复杂的解决方案是随机选择不同类别的m个标签（用M表示），然后使用高级工作ETOP [11]从子集M中收集信息。但是，这种设计必须采取 每次询问标签时都考虑到整个标签集N，降低了采样效率（大概意思是，每次随机选取即采样都在整个集合里取）。 例如，ETOP中排序向量的大小必须设置得足够长以保证M之外的标签不被选中。 我们不是这样做，而是深入研究抽样问题的两个特点，并提出了一个两阶段的解决方案：1）将一个类别与其他类别分开; 2）在这个类别中挑出一个标签。 第一步可以帮助我们快速放大一个类别，而不是整个标签集。 第二步只使用4位索引根据标签ID的几何分布轮询单个标签。 有效的两个步骤组成了我们的远远优于BP和ETOP的TPS协议。

C. Protocol Description
TPS通常由几个抽样轮次组成，每个抽样轮次使用两个阶段（排序阶段和轮询阶段）对近30％的类别进行抽样。 排序阶段告诉标签是否以及何时参与这一轮，轮询阶段使用4位索引来将标签从类别中提取出。 详情如下

1) Ordering Phase:
读卡器首先通过广播一个带有参数f，r的请求来初始化帧，其中f是帧大小，r是随机种子。该帧是一个虚拟的帧，永远不会实际发出来。它只是作为一个工具来找出在这个帧中有用的插槽;只有由有用的插槽组成的实际帧将在稍后执行。

在接收到这个请求后，每个标签在H（cid，r）mod f的位置随机选择一个槽位，其中cid是标签的类别ID，H（·）是所有标签共享的散列函数。由于每个标签都采用类别ID而不是标签ID作为散列种子，因此来自同一类别的标签必须位于同一个插槽中。没有标签选择的插槽，仅来自单个类别的标签，来自多个类别的标签分别被称为空插槽，同类插槽和异类插槽。以图1为例。共有三类;同一类别中的标签散列到同一个槽位。第二个插槽是同类的，因为它是由单个类别C1中的标签选取的。相反，第四个时隙是不同的，因为它的三个标签（t2，t5和t6）来自两个不同的类别。左边是空的插槽。注意，这个阶段的帧大小f只与类别的数目m有关，而与标签的数量n无关，大大降低了通信开销。例如，每个类别平均有20个标签，m = 0.05n。稍后我们将讨论参数设置和协议开销。

存在于同类槽中的类别称为同类类别，相应的标签称为同类标签。在以下协议执行中，只有同类插槽可能会有用。当且仅当它是同类且可解的（我们将在轮询阶段给出可解析的时隙的定义）时，一个时隙（即插槽，slots）才是有用的。不符合上述要求的左侧插槽被称为无用插槽。每个标签不知道它选择的槽是否有用，但是读卡器可以。利用标签ID（包含类别ID）信息，读卡器可以预测虚拟框架中的哪些插槽是有用的。在执行实际的帧之前，它将删除无用的插槽。为此，读者广播一个f位的排序向量V [11]。 V中的每个比特对应于虚拟帧中的一个时隙：“0”指示无用，“1”指示有用。如果V太长，读卡器可以把它分成许多96位段，并在一个长度为tid的时隙中传输每个段[9]。

从标签的角度来看，排序矢量V携带两条信息。首先，标签可以通过检查V中的相应位来了解是否选择了一个有用的插槽。只有这样，标签才会参与下面的轮询阶段。另一方面，V告诉实际帧中有用时隙的索引。如果一个标签发现在它的位之前有i个位为V（即1），标签就知道它选择了第（i + 1）个有用的时隙
D.Polling Phase
一个有用的插槽绝对是一个同质插槽。考虑到有用时隙中的同类Ci（1≤i≤m），我们的目的是有效地选出一个标签。在这个阶段，每个标签分别挑选一个索引R（H（id，r）mod 2^K），其中id是标签ID，r是在前一个排序阶段中使用的相同散列种子，K是常数，R ·）是输入的二进制表示中最右边位1的位置。例如，R（81_10）= R（01010001_2）= 1，R（104_10）= R（01101000_2）= 4。事实上，任意标签的散列结果（H（·）mod 2^K）是一个K位的二进制数。当我们从右到左检查它的每一位时，每一位的值可以看作伯努利轨迹：'1'成功，'0'失败。 R（·）运算符实际上是获得第一个“成功”所需的轨迹数，即R（·）遵循几何分布：P rob [R（·）= j] = 1 /2^j。几何分布特征使标签数量随着指标的增加而急剧下降。例如，大约一半的标签选择索引1，只有大约1 /2^10≈0.1％的标签选择索引10.如果一个索引恰好被一个标签选中，我们称之为单一索引。直观地说，围绕log2（| Ci |）的指标很有可能是单身人士，其中|·|是集合的基数和| Ci |表示Ci中标签的数量。由于读者知道所有标签的ID，它可以预测每个标签的散列结果。如果前一个虚拟框架中的一个槽有一个或多个单体索引，我们称之为可解析槽。如前所述，有用的时隙是同类的以及可解析的。读者然后播放一个由有用的插槽组成的实际帧。在每个时隙中，广播一个log2 K比特的单例索引以隔离相应的标签（单个标签），并且专用匹配的标签根据需要发送信息。一旦信息被成功采样，一个类别中的所有标签都将保持沉默。未被采样的左侧类别保持活跃状态，将参与下面的抽样轮次。这两个阶段一个接一个地重复，直到所有类别都被完全采样。请注意，有用的插槽可能包含多个单身索引。在这种情况下，我们可以随机选择其中的一个作为轮询矢量来询问相应的标签。
## 评估
A. Simulation Setting
我们的仿真设置遵循C1G2标准的规范[12]。从阅读器到标签的任何两个连续进行通信，或反之亦然，以时间间隔分隔。例如，在阅读器发送命令之后，所有的标签在回复阅读器之前都必须等待发送到接收的翻转时间T1。另一方面，在接收到来自标签的回复之后，阅读器需要在与标签交谈之前等待接收至发送周转时间T2。根据规范，T1是最大（RT cal，20Tpri），T2的范围是3Tpri到20Tpri，其中RT cal是读写器到标签的校准符号，等于数据0符号的长度加上数据1符号的长度，Tpri是后向散射链路脉冲重复间隔。在我们的仿真中，我们设定符合C1G2标准的T = T1 = T2 =200μs。根据物理实现和实际环境，阅读器和标签之间的传输速率不一定是对称的。标签 - 读取器传输速率随数据编码而变化：FM0为40kbps至640kbps，米勒调制子载波为5kbps至320kbps。我们得到的交集为40kbps到320kbps，并采用下限40kbps作为数据速率。换句话说，传送一位需要25μs的标签。从阅读器到标签的数据速率范围从26.7 kbps到128 kbps。同样，我们将数据速率设置为26.7 kbps的下限，读取器传输一位需要37.45μs。此外，在整个模拟过程中，类别ID的长度被设置为32位。请注意，其他参数设置可能会改变绝对度量，但仿真结论可以用类似的方式绘制。根据上述参数设置，来自标签的1位短响应的持续时间ts等于25 + T =225μs;通过标签传输类别ID的持续时间tcid为25×12 + T =500μs;阅读器广播类别ID的持续时间为37.45×32 + T =1398.4μs;阅读器广播标签ID的持续时间t r id为37.45×96 + T = 3795.2μs;为了通过标签发送w位分类信息，持续时间tinf等于25w +Tμs。所有结果都是100次模拟运行的平均结果。
B. Execution Time
在这个小节中，我们评估在知道标签ID的情况下我们的采样协议TPS的执行时间。如第三节所述，基本投票（BP）和ETOP可以修改为信息抽样。为了实现这个目标，读者首先从每个类别中随机挑选一个标签，这些标签组成一个标签集合S.然后，对于BP，读写器依次在S中广播每个标签的ID，然后在每次广播之后等待他们的回复。所有标签都保持收听，只有完全匹配的标签才会将所需的分类信息传输给读者。采样过程终止，直到S中的所有标签都被询问。另一方面，ETOP专门用于以有效的方式收集来自标签子集的标签信息。在我们的问题中，我们可以把S作为N的有用子集，并按照原样执行ETOP来按需要收集类别信息。在仿真中，我们保留了与[11]中相同的ETOP参数设置，即帧大小为24×| S |，段长为80位，每段由4个分区组成。图3比较了不同标签数量下BP，ETOP和TPS的执行时间。在模拟中，我们将每个类别中的标签数量固定为10，并将标签数量从10,000到100,000（类别数量从1000到10000）变化。如图3（a）和图3（b）所示，在不同的参数设置下对两种不同长度的类别信息（w = 10,20）进行采样。在这些图中，我们观察到TPS比其他两种协议优越，因为它通过平均只广播约7.5比特的轮询矢量来将每个类别的标签与其他标签隔离。例如，为了从50,000个标签（如图3（b）所示）中抽取10位的分类信息，BP需要最长的时间21.2s，因为它需要传输繁琐的标签ID。 ETOP将执行时间缩短了56.1％，达到9.3s，因为它使用分段Bloom过滤器来隔离和排序属于S的标签，从而避免大部分ID传输。 TPS性能最好，仅消耗3.7s，分别比BP和ETOP提高5.7倍和2.5倍性能。其他参数设置也可以得出类似的结论：TPS最好，ETOP跟随，BP最差。在图4中，我们比较了BP，ETOP和TPS在每个类别中不同标签数量下的执行时间。在模拟中，我们将标签数量固定为100,000，并将每个类别中的标签数量从5改变为50.一旦类别信息的长度w固定，我们观察到这三个协议的执行时间随着数量每个类别中的标签增加。这是因为类别数量随着每个类别中标签数量的增加而减少，从而减少了采样数量，节省了通信开销。与图3类似，也可以得出同样的结论：TPS是最有效的，ETOP更差，而BP是最耗时的。例如，当类别信息的长度为20位，每个类别有25个标签（如图4（b）所示）时，BP的执行时间为17.0s，这是三种协议中最长的。相比之下，TPS花费最少的执行时间。实现相同的采样任务只需不到3.0秒，产生大约6倍的性能增益。尽管ETOP远优于BP，但它比TPS需要更长的时间，即7.4s。请注意，三个协议的执行时间随着w的增加而增加。这是直观的，因为当w较大时，标签需要传输更多类别相关的数据。基于上述仿真结果，我们得出结论，通过传输几个比特来选择每个类别中的标签，我们的协议TPS优于BP和ETOP，大大提高了采样效率
## CONCLUSION
在本文中，我们研究了多类RFID系统中的类别信息收集问题。 考虑到这个问题的新特点，我们提出了一个两阶段抽样协议（TPS），首先快速放大到一个类别，然后使用标签的几何分布将一个任意标签从类别中分离出来。 我们从理论上分析协议性能，并讨论最小化总体执行时间的最佳参数设置。 大量的仿真表明，TPS能够将轮询矢量的长度缩短到只有7.5位，与96位标签ID相比，这是非常有效的。
