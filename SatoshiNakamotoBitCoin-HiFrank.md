# 比特币创世文 Bitcoin: A Peer-to-Peer Electronic Cash System 解读
--- 

>>>一夜之间，区块链再次火起来。网络上大量的讲解分析区块链比特币的文章蜂拥而出。我是一个考据癖者和学术原教旨主义者，刚好周末，跑完步回来，把区块链的始作俑者，中本聪的论文再拿出来，整理一篇中英文对照，分飨给大家。<br><br>
>>>我尽量意译而不直译，每部分后边再加一点我的解读。翻译不求严谨只求达意，要严谨请读原文。即便如此，作为一篇专业论文，无论如何无法在这里将其解读到通俗易懂，请见谅。<br><br>
论文链接：[Bitcoin: A Peer-to-Peer Electronic Cash System](https://www.bitcoincash.org/bitcoin.pdf)
>>><p align="right"> ——  HiFrank  冯立超</p>

<br>

# Bitcoin: A Peer-to-Peer Electronic Cash System
## 比特币：一种点对点的电子现金系统
<p align="center">Satoshi Nakamoto<br>
satoshin@gmx.com<br>
www.bitcoin.org<br><br>
作者：Satoshi Nakamoto  中本聪<br>
翻译：HiFrank 冯立超<br>
解读：HiFrank 冯立超<br>
Hiweb@Outlook.com
</p>

# Abstract. 
>> A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer network. The network timestamps transactions by hashing them into an ongoing chain of hash-based proof-of-work, forming a record that cannot be changed without redoing the proof-of-work. The longest chain not only serves as proof of the sequence of events witnessed, but proof that it came from the largest pool of CPU power. As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network, they'll generate the longest chain and outpace attackers. The network itself requires minimal structure. Messages are broadcast on a best effort basis, and nodes can leave and rejoin the network at will, accepting the longest proof-of-work chain as proof of what happened while they were gone.

## 概要
>> 一套纯粹的点对点电子现金系统，将允许交易双方直接进行在线支付，而不需要通过金融机构介入其中。**数字签名**解决了部分问题，但如果仍需要一个可信的第三方机构介入来防止**双重支付**问题，这就失去了点对点电子现金系统的意义。我们提出了一种用点对点网络解决双重支付问题的方案。点对点网络通过添加时间戳的方式为将交易的**哈希值**添加到一条不断增长的链中，该链采用基于哈希的工作量证明方式进行增长。这确保了记录不能被修改，除非重新进行工作量证明。最长的链不仅作为事件系列的见证，同时也证明它来自于最大 CPU 算力池。只要主要的算力在非恶意节点的控制之下，它们就会超过攻击者而产生最长的链。这种网络本身只需要最小的结构。信息以尽力而为的方式广播，节点可以离开后再重新加入，它接受最长的工作量证明链，证明其不在线时所发生的事务。

## HiFrank 解读
- **哈希算法**、**数字签名**等，是确保信息安全的最基本概念。简单解释一下：一段数据， 对其进行某种运算，得到一个值。这段数据如果有篡改，这个算出来的值就会变。拿到数据后再算一下这个值是不是正确，就知道数据是否被改过。这是**哈希算法**的通俗解释，后面我还会讲。**数字签名**说来话长，暂不赘述。它基于哈希算法和加密算法提供了数据的可信性。这篇论文以上述概念为基础，但不做解释。就好像讲微积分默认你懂算数一样。
- 中本聪的这篇论文，关键要解决的问题是 **双重支付** ，即 **double-spending** 问题。现实生活中，硬币/钞票不可能重复使用，你把那枚又大又圆的硬币或者那张又长又宽的钞票给我了，你就没了，你没办法把它再给另一个人（一女怎可二嫁）。但，电子现金，是一个数字信息，是可以复制的，有可能被恶意重复使用的。中本聪的这篇论文就是要解决这个双重支付问题。


---
# 1. Introduction
Commerce on the Internet has come to rely almost exclusively on financial institutions serving as trusted third parties to process electronic payments. While the system works well enough for most transactions, it still suffers from the inherent weaknesses of the trust based model. Completely non-reversible transactions are not really possible, since financial institutions cannot avoid mediating disputes. The cost of mediation increases transaction costs, limiting the minimum practical transaction size and cutting off the possibility for small casual transactions, and there is a broader cost in the loss of ability to make non-reversible payments for nonreversible services. With the possibility of reversal, the need for trust spreads. Merchants must be wary of their customers, hassling them for more information than they would otherwise need. A certain percentage of fraud is accepted as unavoidable. These costs and payment uncertainties can be avoided in person by using physical currency, but no mechanism exists to make payments over a communications channel without a trusted party.
What is needed is an electronic payment system based on cryptographic proof instead of trust, allowing any two willing parties to transact directly with each other without the need for a trusted third party. Transactions that are computationally impractical to reverse would protect sellers from fraud, and routine escrow mechanisms could easily be implemented to protect buyers. In this paper, we propose a solution to the double-spending problem using a peer-to-peer distributed timestamp server to generate computational proof of the chronological order of transactions. The system is secure as long as honest nodes collectively control more CPU power than any cooperating group of attacker nodes.
## 1. 简介
互联网上的商业活动，已经变得几乎完全依赖于可信任第三方金融机构来处理电子支付。虽然这套系统对于绝大多数交易都运行良好，但这种基于信任的模型仍有其固有缺陷。完全不可逆的交易是不现实的，因为金融机构不可避免地要参与调解纠纷。调解成本会增加交易成本，从而限制了最小实际交易规模，切断了小额随机交易的可能性。而且，为不可逆的服务所提供不可逆支付的能力的缺失将产生更大的成本。因为交易可能被逆转，对信任的需求进一步增加。商人为对其客户保持警惕，常获取其本不需要的额外信息。一定比例的诈骗是可接受和不可避免的。作为个人，可以通过使用真实货币来避免这种成本以及支付的不确定性。但不存在一种使用通信渠道进行支付，却不依赖可信任第三方的机制。<br>
我们需要的是一种基于加密证明的，而不是基于信任的电子支付系统，从而允许任意两方直接进行交易，而不需要通过信任的第三方。对交易进行可逆操作所需的不切实际的计算量，将保护卖家免受欺诈。常规的保管机制则很轻易实施以，保护买家。本论文中，我们提出一种运用分布式点对点时间戳化服务来生成交易信息和次序的充足计算证明来防止双重支出的解决方案。这套系统只要在主要的算力在非恶意节点的控制之下就是安全的。
## HiFrank 解读
- 在以物易物的年代，或者只使用硬币现钞进行交易的年代，交易双方可以随时自行交易，一手交钱一手交货，基本不会产生欺诈。而在互联网交易中，如果没有可信任的中间机构，很容易导致纠纷，出现可逆操作。想想如果没有支付宝，商家怎么保证发了货能收到钱？买家怎么保证给了钱能收到货？<br>
- 电子货币设计者所期望实现的，就是通过电子的方式，实现当年两个人用现金直接交易的场景。<br>
- 举个例子：现在我用手机微信给你转钱，并不是把我手机上的钱转到你手机上。咱俩手机上其实都没钱，钱在腾讯服务器上，我们只是给腾讯发信息让它分别改一下你我在腾讯服务器上的钱的数字。<br>
银行卡也是一样，卡里没钱。卡只是记录了你的银行账号。所以你用卡转账，和你到银行柜台给小美眉说出你的账号进行转账效果是一样的。<br>
真正的电子货币则完全不同，钱就在你的手机上，或就在你的卡上。上面记录的你的钱数，类似于你兜里的现金，是多少就是多少，不依赖于某个银行或机构。并且可以直接使用或划转。<br>
其实公交卡就是这样的，里边充的是真钱。由于其安全机制尚需完善，所以类似的卡只用于小额特定场景。而且它不能挂失，因为卡丢了没人知道里面原来还有多少钱。<br>
- 本论文，就是提供了一种方法，实现真正的点对点的电子货币，并解决双重支付、可逆交易问题。其主要方法是时间戳、哈希算法、以及交易证明加密计算的巨大计算量。

---
# 2. Transactions
We define an electronic coin as a chain of digital signatures. Each owner transfers the coin to the next by digitally signing a hash of the previous transaction and the public key of the next owner and adding these to the end of the coin. A payee can verify the signatures to verify the chain of ownership.<br>

![](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin1.png?raw=true)<br>

The problem of course is the payee can't verify that one of the owners did not double-spend the coin. A common solution is to introduce a trusted central authority, or mint, that checks every transaction for double spending. After each transaction, the coin must be returned to the mint to issue a new coin, and only coins issued directly from the mint are trusted not to be double-spent. The problem with this solution is that the fate of the entire money system depends on the company running the mint, with every transaction having to go through them, just like a bank.<br>
We need a way for the payee to know that the previous owners did not sign any earlier transactions. For our purposes, the earliest transaction is the one that counts, so we don't care about later attempts to double-spend. The only way to confirm the absence of a transaction is to be aware of all transactions. In the mint based model, the mint was aware of all transactions and decided which arrived first. To accomplish this without a trusted party, transactions must be publicly announced[1], and we need a system for participants to agree on a single history of the order in which they were received. The payee needs proof that at the time of each transaction, the majority of nodes agreed it was the first received.

## 2. 交易
我们将**电子币**定义为数字签名链。币的转移是通过持有者 *Owner* 对前一笔交易的哈希值 *Hash* 和下一个所有者的公匙 *Public Key* 进行数字签名 *Signature*，并将其放到币的末端来实现（如下图，时间有限，图暂不译）。收款人可以通过验证签名来验证链的持有者。
<br>

![](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin1.png?raw=true)<br>

然而，问题是收款者还是没办法确定付款者是否把他的电子币花了两次。一个通常的解决方案是引入一个可信的中间权威机构，或者造币厂，来检查每一笔交易是否存在双重支付。在每笔交易之后，必须将币退回造币厂来发行新的币，只有直接由造币厂发行的币才能确信没有双重支付。这种解决方案的问题是，整个货币体系的命运都依赖于运营造币厂的公司，每一笔交易都得通过他们，就像一个银行。<br>
我们需要一种方法，来让收款人知道之前的持有者没有签过任何更早的交易。为此目标，最早的交易就是算数的交易，所以我们不必关心此后的双重支付企图。确认缺失交易的唯一方法是知道所有交易。在基于造币厂的模型中，造币厂知道所有的交易，并确定哪笔交易是第一笔。为了在不引入可信方的前提下来实现同样的效果，交易必须被公开宣告。同时我们还需要一套系统，让参与者认同他们收到的是交易次序的唯一历史。在每次交易的时候，收款人需要证实大多数节点认同这是第一笔。
## HiFrank 解读
- 电子币，Electronic Coin，这个术语使用了硬币的币 *Coin*，想必就是要强调其独立性，其类似于传统钢镚儿一样的不依赖于第三方的可直接点对点交易的特性。这是电子货币的核心特性。
- 本段一开始几句话就讲清楚了 **区块链** 的结构。即每一笔交易/每一个区块中，写入上一笔交易/上一个区块的经签名的哈希值，哈希和签名确保了信息不可篡改，并一路链下去。<br>
但这不是本文的关键，本文的关键，是防止双重支付。是，我签了字了说这张钞票给你了。我又签了一遍说这张钞票给他了。怎么办？<br>
然后，作者又扯出一个办法，造币厂，就是每一笔钱都是直接来源于造币厂的，只能花一次。收款者想把钱再花到别处，得交回造币厂，造币厂重发个新的给你用，所有的人都只认造币厂发到新钱。
<br>但是，这又回到了需要可信任的第三方的老路上去了。<br>
- 之所以没完没了又扯造币厂这个中间商，啰嗦吗？非也。<br>是因为作者要用造币厂模型的一个特点：**只用新钱**。就是说，想办法只认这笔钱的第一次交易！想办法在没有可信中间机构介入的情况下，确认我是第一个用这笔钱的，后面重复用的，用了白用，不算。<br>
- 到现在还在铺陈问题呢，还没开始讲解决方案呢。怎么办？往后看。
---
# 3. Timestamp Server
The solution we propose begins with a timestamp server. A timestamp server works by taking a hash of a block of items to be timestamped and widely publishing the hash, such as in a newspaper or Usenet post [2-5]. The timestamp proves that the data must have existed at the time, obviously, in order to get into the hash. Each timestamp includes the previous timestamp in its hash, forming a chain, with each additional timestamp reinforcing the ones before it.<br>
![](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin2.png?raw=true)<br>
## 3.时间戳服务器
我们提供的解决方案由时间戳服务器开始。时间戳服务器对多项信息组成的每个区块打上时间戳并进行哈希，并广泛发布此哈希值，类似于报纸或 Usenet 分布式讨论系统那样。具体细节如本论文最后参考文献2~5文章所述。时间戳证明数据在那个时间点一定是存在的，明显的，为了得到哈希值，每个时间戳的哈希值中都包含之前的时间戳，从而形成一条链，每一个新增的时间戳都对其上一个进行增强。如下图所示。<br>
![](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin2.png?raw=true)<br>

## HiFrank 解读
- 如前所述，防止双重支付的办法是只认最早的那笔交易。那么，时间信息就尤为重要。所以，每笔交易中都要记录时间信息。<br>生活中，时间戳这个词很形象，就是那个邮戳，就是那个在给你买的面包包装袋上盖上生产日期的那个戳。就是你手机照相时印到角角上的那个日期。<br>但在计算机领域中，这个较复杂，但很成熟。故作者没展开，给了参考文献。有兴趣的请参阅本论文最后参考文献2~5文章所述。
---
# 4. Proof-of-Work
To implement a distributed timestamp server on a peer-to-peer basis, we will need to use a proof-of-work system similar to Adam Back's Hashcash [6], rather than newspaper or Usenet posts. The proof-of-work involves scanning for a value that when hashed, such as with SHA-256, the hash begins with a number of zero bits. The average work required is exponential in the number of zero bits required and can be verified by executing a single hash.<br>
For our timestamp network, we implement the proof-of-work by incrementing a nonce in the block until a value is found that gives the block's hash the required zero bits. Once the CPU effort has been expended to make it satisfy the proof-of-work, the block cannot be changed without redoing the work. As later blocks are chained after it, the work to change the block would include redoing all the blocks after it.<br>
![3](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin3.png?raw=true)<br>
The proof-of-work also solves the problem of determining representation in majority decision making. If the majority were based on one-IP-address-one-vote, it could be subverted by anyone able to allocate many IPs. Proof-of-work is essentially one-CPU-one-vote. The majority decision is represented by the longest chain, which has the greatest proof-of-work effort invested in it. If a majority of CPU power is controlled by honest nodes, the honest chain will grow the fastest and outpace any competing chains. To modify a past block, an attacker would have to redo the proof-of-work of the block and all blocks after it and then catch up with and surpass the work of the honest nodes. We will show later that the probability of a slower attacker catching up diminishes exponentially as subsequent blocks are added.<br>
To compensate for increasing hardware speed and varying interest in running nodes over time, the proof-of-work difficulty is determined by a moving average targeting an average number of blocks per hour. If they're generated too fast, the difficulty increases.
## 4.工作量证明
在基于点对点的系统上实现分布式时间戳服务器，我们需要使用一种 Adam Back's 的 Hashcash 类似的工作量证明系统（见参考文献 6），而不是前述报纸或 Usenet 系统。工作量证明涉及到在哈希运算后扫描一个值，以 SHA-256 为例，哈希值以一系列的零比特位开始。平均工作量与所要求的0位的数量呈指数级上升，且可以通过执行一次哈希就可以验证。<br>
对于时间戳网络来说，工作量证明机制这样实现：在区块中递增一个随机值，对于每次增加随机值后的区块，计算该区块的哈希值，直到找到满足所要求的零比特位数量的哈希值为止。一旦 CPU 算力被花费来使其满足工作量证明，除非重做工作量证明，否则这个区块就不能被修改。当之后的区块加入进来，修改区块导致的工作量将包含重算之后所有的区块的工作量。<br>
![3](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin3.png?raw=true)<br>
工作量证明机制还解决了在多数票决策中确定谁为代表的问题。如果多数票的确定是靠一个IP地址一票，它就会被可以配置多个IP的节点搅局。工作量证明本质上是一CPU一票的机制。多数票决策由最长的链代表，其中包含最大的工作量投入。如果大多数CPU算力由诚信节点所控制，那么诚信链就会比所有竞争的链都生长的更快并超过他们。为了修改一个之前的区块，攻击者必须重算这个区块以及这个区块之后所有区块的工作量证明，并要赶上并超过诚信节点。我们随后会展示落后的攻击者赶上的可能性随着区块的增加呈指数级减少。<br>
为了抵偿硬件速度的增加，以及随时间推移所投入运算节点的变化，工作量证明的难度随每小时平均生成区块数所决定，如果它们产生的过快，就会增加难度。
## HiFrank 解读
- 还是得说说哈希算法的特点，哈希算法特点是：一大串数据，经过一种称作哈希运算的操作，哈希是个人名，可以得到一个简短的值，如128位或者256位。如果前面那一大串数据有改动，后面算出来的哈希值就会变化。
- 哈希运算的另一个特点是：不可逆。就是可以由前面那一大串数据算出这个一小段数据，即哈希值。但不能由这个结果值反算回原来的值。想想上学的时候学的有些函数没有反函数。<br>
记得十几年前遇到一个哥们儿拉投资，他发明了一种压缩算法，不管多大的文件都可以压缩成一个很小的压缩包，只是还没有研究出来如何解压出原来的文件，需要投资继续研究并改变世界，想起来就开心。<br>
- 所以，哈希算法顺着算可以，倒着算不行。当对结果提出特定要求的时候，比如要求结果开头是几个0，只有一种办法，试算。把那个区块，就是那一大串数据，用哈希运算算一下，不符合要求，加个随机数，再算，不行，换个随机数，再算... 直到算出来为止。<br>
而一旦算出来了，把结果告诉大家，大家却很容易验证。比如猜谜语，你猜呀猜猜不着，我猜出来了告诉你，你马上会“噢——，对对对”。
- 这需要很大的算力，为了交易欺诈而重算，完全是得不偿失。我们必须假设好人比坏人多，而且坏人各怀私心。


---
# 5. Network
The steps to run the network are as follows:

1) New transactions are broadcast to all nodes.
2) Each node collects new transactions into a block.
3) Each node works on finding a difficult proof-of-work for its block.
4) When a node finds a proof-of-work, it broadcasts the block to all nodes.
5) Nodes accept the block only if all transactions in it are valid and not already spent.
6) Nodes express their acceptance of the block by working on creating the next block in the chain, using the hash of the accepted block as the previous hash.

Nodes always consider the longest chain to be the correct one and will keep working on extending it. If two nodes broadcast different versions of the next block simultaneously, some nodes may receive one or the other first. In that case, they work on the first one they received, but save the other branch in case it becomes longer. The tie will be broken when the next proof-of-work is found and one branch becomes longer; the nodes that were working on the other branch will then switch to the longer one.<br>
New transaction broadcasts do not necessarily need to reach all nodes. As long as they reach many nodes, they will get into a block before long. Block broadcasts are also tolerant of dropped messages. If a node does not receive a block, it will request it when it receives the next block and realizes it missed one.

## 5.网络
网络运行步骤如下：
1) 新交易广播给所有节点。
2) 每个节点都把新的交易收集到区块中。
3) 每个节点都试图找到其区块的工作量证明。
4) 当某个节点找到了工作量证明，它将其广播到所有节点。
5) 只有所有交易都有效且没有被重复支付，节点才接受该区块。
6) 节点通过在链上创建下一个块来表示他们接受了该块，并使用接受块的哈希作为新创建块的前导哈希。

节点总是将最长链视为正确链，并继续延长它。如果两个节点同时广播了不同版本的下一区块，则一些节点可能会先收到其中某一个。在该情况下，它们以其先收到的那个开始工作，但同时保存另一个分支，以防该分支变成了更长的链。当找到下一个工作量证明导致某个分支变得更长，这种情形就会被打破，在另一条分支工作的节点就会切换到更长的那条链上。<br>
新交易的广播没有必要到达所有的节点。只要到达很多节点，它们就会进入区块。区块的广播同样还容忍消息丢失，如果一个节点没有收到一个区块，它会在收到下一个区块时，意识到它丢失了一个区块，并去请求它。
## HiFrank 解读
- 本段概念清晰，WYSIWYG。
---
# 6. Incentive
By convention, the first transaction in a block is a special transaction that starts a new coin owned by the creator of the block. This adds an incentive for nodes to support the network, and provides a way to initially distribute coins into circulation, since there is no central authority to issue them. The steady addition of a constant of amount of new coins is analogous to gold miners expending resources to add gold to circulation. In our case, it is CPU time and electricity that is expended.<br>
The incentive can also be funded with transaction fees. If the output value of a transaction is less than its input value, the difference is a transaction fee that is added to the incentive value of the block containing the transaction. Once a predetermined number of coins have entered circulation, the incentive can transition entirely to transaction fees and be completely inflation free.<br>
The incentive may help encourage nodes to stay honest. If a greedy attacker is able to assemble more CPU power than all the honest nodes, he would have to choose between using it to defraud people by stealing back his payments, or using it to generate new coins. He ought to find it more profitable to play by the rules, such rules that favour him with more new coins than everyone else combined, than to undermine the system and the validity of his own wealth.
## 6.奖励机制
按照惯例，区块的第一笔交易是一笔特殊交易，它发起一个由该块的创建者所拥有的新币。这就激励了为网络提供支持的节点，且由于没有中央机构发行货币，这就提供了发行货币投入流通的机制。稳定增加一定数量的新币的过程，类似于黄金矿工花费资源增加黄金并投入到流通系统。在我们的方案中，花费的资源是CPU工作时间和电能。<br>
激励也可以是交易费用，如果交易的产出值小于投入值，差额就是交易费用，添加到包含这个交易的区块的奖励中。一旦预定数量的币进入流通，激励就完全转换为交易费用，且没有通货膨胀的风险。<br>
这样的激励机制也会促进节点保持可信。如果一个贪婪的攻击者可以组装比所有诚实节点更多的CPU算力，他就必须做出如下选择：利用这些算力偷回他的付款进行欺诈，或用其生成新币。他会发现按规则行事更划算，这样的规则对他来说将获得比其他人加起来还多的新币，而不是破坏系统和他的财富的有效性。
## HiFrank 解读
- 这里给出了电子货币的发行机制，该机制保证了不需要中央银行的货币发行机制和防止通胀的机制。
- 同时也看到了挖矿的原理。挖矿本质上是投入算力和电力进行前面所讲的工作量证明计算。奖励机制让投入诚信节点参与挖矿，而不是搞破坏是成为可能。
---
# 7. Reclaiming Disk Space
Once the latest transaction in a coin is buried under enough blocks, the spent transactions before it can be discarded to save disk space. To facilitate this without breaking the block's hash, transactions are hashed in a Merkle Tree [7][2][5], with only the root included in the block's hash. Old blocks can then be compacted by stubbing off branches of the tree. The interior hashes do not need to be stored.<br>
![4](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin4.png?raw=true)<br>
A block header with no transactions would be about 80 bytes. If we suppose blocks are generated every 10 minutes, 80 bytes * 6 * 24 * 365 = 4.2MB per year. With computer systems typically selling with 2GB of RAM as of 2008, and Moore's Law predicting current growth of 1.2GB per year, storage should not be a problem even if the block headers must be kept in memory.
## 7.回收磁盘空间
当新交易被放入链中，且在该链后扩展了足够多的区块之后，此前已支付过的交易可以被丢弃，以节省磁盘空间。为确保哈希不被破坏，交易在Merkle树中被哈希，只有根保存在区块的哈希中（参考文献7，2，5）。旧区块可以通过截断树分支的方式来压缩，其内部哈希不需要存储。<br>
![4](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin4.png?raw=true)<br>
没有交易记录的区块头大概占80字节。假设每10分钟产生一个区块，每年的大小为 80 bytes * 6 * 24 * 365 = 4.2MB。在2008年，在售的典型计算机系统为2GB的内存，摩尔定律预测当前的增长为每年1.2GB，就算区块头都导入内存，存储也应该不是问题。
## HiFrank 解读
- 通过丢弃已完成的早期交易信息只保留根哈希的方式，降低了区块链的大小。<br>
这些考虑都是在2008年发布论文时做的，对区块链在各领域中的发展不是当时可以预见的。
---
# 8. Simplified Payment Verification
It is possible to verify payments without running a full network node. A user only needs to keep a copy of the block headers of the longest proof-of-work chain, which he can get by querying network nodes until he's convinced he has the longest chain, and obtain the Merkle branch linking the transaction to the block it's timestamped in. He can't check the transaction for himself, but by linking it to a place in the chain, he can see that a network node has accepted it, and blocks added after it further confirm the network has accepted it.
<br>
![5](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin5.png?raw=true)<br>
As such, the verification is reliable as long as honest nodes control the network, but is more vulnerable if the network is overpowered by an attacker. While network nodes can verify transactions for themselves, the simplified method can be fooled by an attacker's fabricated transactions for as long as the attacker can continue to overpower the network. One strategy to protect against this would be to accept alerts from network nodes when they detect an invalid block, prompting the user's software to download the full block and alerted transactions to confirm the inconsistency. Businesses that receive frequent payments will probably still want to run their own nodes for more independent security and quicker verification.

## 8. 简化支付验证
可以在不运行完整网络节点的情况下验证付款。用户只需要保留最长工作量验证链的区块头副本，他可以不断发起查询，直到确信它拿到的是最长链。并且包含将交易连接到将其时间戳化的区块里的Merkle 分支。用户不能自行检查交易，但通过将其链接到链的某一个位置，他可以看见有网络节点已经接受它，并且在它之后又链接了新的区块，进而确认交易已被全网接受。<br>
![5](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin5.png?raw=true)<br>
由此，只要可信节点控制着网络，验证就是可靠的。但如果网络被攻击者控制，则更加脆弱。由于网络节点可以自我进行交易验证，只要攻击者还可以继续控制网络，这种简化方案就可能会被攻击者的虚假交易欺骗。一种抵御这种攻击的保护策略是，节点接收它们检测到非法区块的警报，提示用户软件下载完整的块，并警告交易以确认不一致性。收款业务频繁的企业可能仍需运行自己的节点以获得更加独立的安全性和更快的验证。
## HiFrank 解读
- 本部分给出了普通用户简化支付验证的方式。同时也指出了安全风险。
---
# 9. Combining and Splitting Value
Although it would be possible to handle coins individually, it would be unwieldy to make a separate transaction for every cent in a transfer. To allow value to be split and combined, transactions contain multiple inputs and outputs. Normally there will be either a single input from a larger previous transaction or multiple inputs combining smaller amounts, and at most two outputs: one for the payment, and one returning the change, if any, back to the sender.<br>
![6](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin6.png?raw=true)<br>
It should be noted that fan-out, where a transaction depends on several transactions, and those transactions depend on many more, is not a problem here. There is never the need to extract a complete standalone copy of a transaction's history.
## 9.值的结合与分离
尽管可以分别处理每一个币，但为每一分钱单独进行交易会很笨拙。为了允许值的分离与结合，交易包含多个输入和输出。通常会有一个来自较大的前导交易的单一输入或多个来自较小前导交易的汇总输入，而最主要的两项输出：一是付款，另一项是找零，如果有的话，就会退回到支付者。<br>
![6](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin6.png?raw=true)<br>
需要说明，当一笔交易建立在几笔交易的基础上，而那些交易又建立在更多的交易之上时，并不是问题。我们不需要展开所有的独立交易历史。
## HiFrank 解读
- 所见即所得，无需解读。
---
# 10. Privacy
The traditional banking model achieves a level of privacy by limiting access to information to the parties involved and the trusted third party. The necessity to announce all transactions publicly precludes this method, but privacy can still be maintained by breaking the flow of information in another place: by keeping public keys anonymous. The public can see that someone is sending an amount to someone else, but without information linking the transaction to anyone. This is similar to the level of information released by stock exchanges, where the time and size of individual trades, the "tape", is made public, but without telling who the parties were.<br>
![7](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin7.png?raw=true)<br>
As an additional firewall, a new key pair should be used for each transaction to keep them from being linked to a common owner. Some linking is still unavoidable with multi-input transactions, which necessarily reveal that their inputs were owned by the same owner. The risk is that if the owner of a key is revealed, linking could reveal other transactions that belonged to the same owner.
## 10.隐私
传统的银行模式通过限制对相关方和可信第三方信息的访问，实现了一定程度的隐私保护。本机制中，由于必须公开宣布所有交易，这种方法将不行了。但通过打破另一个地方的信息流，依然可以保证隐私：即保持公钥匿名。公众可以看到某人正在向某个其他人发送交易，却没有将交易链接到任何人的信息。这和证券交易所发布的信息类似，交易时间、交易规模是公开的，但不会告诉你当事人是谁。<br>
<br>
![7](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin7.png?raw=true)<br>
作为额外的防御措施，应对每个交易使用新的密钥对，以防止它们与公共所有者链接。在多输入交易中，一些连接依旧不可避免，这必然表明他们的输入来自同一所有者。这样的风险在于如果密钥的所有者被暴露，则可以暴露该同一所有者的其他交易。

## HiFrank 解读
- 通过公钥匿名的方法，实现了电子货币交易的隐私。<br>
这一点很关键，这个隐私是真隐私，因为没有第三方参与，所以隐私到不可监管。<br>
不同于银行模型，其隐私是基于规则，因为交易是通过可信的第三方的，其交易信息被该可信第三方掌握，只是不随意提供给其他方而已。<br>
所以，比特币被用到了许多灰色地带。例如勒索病毒。
---
# 11. Calculations
We consider the scenario of an attacker trying to generate an alternate chain faster than the honest chain. Even if this is accomplished, it does not throw the system open to arbitrary changes, such as creating value out of thin air or taking money that never belonged to the attacker. Nodes are not going to accept an invalid transaction as payment, and honest nodes will never accept a block containing them. An attacker can only try to change one of his own transactions to take back money he recently spent.<br>
The race between the honest chain and an attacker chain can be characterized as a Binomial Random Walk. The success event is the honest chain being extended by one block, increasing its lead by +1, and the failure event is the attacker's chain being extended by one block, reducing the gap by -1.<br>
The probability of an attacker catching up from a given deficit is analogous to a Gambler's Ruin problem. Suppose a gambler with unlimited credit starts at a deficit and plays potentially an infinite number of trials to try to reach breakeven. We can calculate the probability he ever reaches breakeven, or that an attacker ever catches up with the honest chain, as follows [8]:<br>
>*p* = probability an honest node finds the next block<br>
>*q* = probability the attacker finds the next block<br>
>*q<sub>z</sub>* = probability the attacker will ever catch up from z blocks behind
<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin81.png?raw=true)<br>
Given our assumption that *p* > *q*, the probability drops exponentially as the number of blocks the attacker has to catch up with increases. With the odds against him, if he doesn't make a lucky lunge forward early on, his chances become vanishingly small as he falls further behind.<br>
We now consider how long the recipient of a new transaction needs to wait before being
sufficiently certain the sender can't change the transaction. We assume the sender is an attacker who wants to make the recipient believe he paid him for a while, then switch it to pay back to himself after some time has passed. The receiver will be alerted when that happens, but the sender hopes it will be too late.<br>
The receiver generates a new key pair and gives the public key to the sender shortly before signing. This prevents the sender from preparing a chain of blocks ahead of time by working on it continuously until he is lucky enough to get far enough ahead, then executing the transaction at that moment. Once the transaction is sent, the dishonest sender starts working in secret on a parallel chain containing an alternate version of his transaction. <br>
The recipient waits until the transaction has been added to a block and z blocks have been linked after it. He doesn't know the exact amount of progress the attacker has made, but assuming the honest blocks took the average expected time per block, the attacker's potential progress will be a Poisson distribution with expected value:<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin82.png?raw=true)<br>
To get the probability the attacker could still catch up now, we multiply the Poisson density for each amount of progress he could have made by the probability he could catch up from that point:
<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin83.png?raw=true)<br>
Rearranging to avoid summing the infinite tail of the distribution...
<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin84.png?raw=true)<br>
Converting to C code...<br>

```
#include <math.h>
double AttackerSuccessProbability(double q, int z)
{
    double p = 1.0 - q;
    double lambda = z * (q / p);
    double sum = 1.0;
    int i, k;
    for (k = 0; k <= z; k++)
    {
        double poisson = exp(-lambda);
        for (i = 1; i <= k; i++)
        poisson *= lambda / i;
        sum -= poisson * (1 - pow(q / p, z - k));
    }
    return sum;
}
```
Running some results, we can see the probability drop off exponentially with z.
```
q=0.1
z=0   P=1.0000000
z=1   P=0.2045873
z=2   P=0.0509779
z=3   P=0.0131722
z=4   P=0.0034552
z=5   P=0.0009137
z=6   P=0.0002428
z=7   P=0.0000647
z=8   P=0.0000173
z=9   P=0.0000046
z=10  P=0.0000012
q=0.3
z=0   P=1.0000000
z=5   P=0.1773523
z=10  P=0.0416605
z=15  P=0.0101008
z=20  P=0.0024804
z=25  P=0.0006132
z=30  P=0.0001522
z=35  P=0.0000379
z=40  P=0.0000095
z=45  P=0.0000024
z=50  P=0.0000006
```
Solving for P less than 0.1%...
```
P < 0.001
q=0.10 z=5
q=0.15 z=8
q=0.20 z=11
q=0.25 z=15
q=0.30 z=24
q=0.35 z=41
q=0.40 z=89
q=0.45 z=340
```
## 11.计算
我们来考虑如下场景：攻击者试图比诚实链更快地生成替代链。即便能够完成这种操作，也不会导致系统开放到可被任意修改的地步。例如，凭空创造价值，或拿走从不属于攻击者的钱。节点不会接受一个不合法的交易支付，诚实的节点永远不会接受一个包含这种交易的区块。攻击者只能试图修改一个他自己的交易，拿回他此前刚花出去的钱。
诚实节点和攻击者之间的竞赛可以被特征化为二项随机游走。成功的事件是诚实的链被延伸一个区块，将领先优势 +1，而失败事件是攻击者的链被延长一个块，将差距减少 -1。
攻击者从特定亏损中赶超的概率类似于赌徒破产问题。假设一个拥有无限信用的赌徒开始出现赤字，并且可进行无数次尝试以试图达到盈亏平衡。我们可以计算其达到盈亏平衡的概率，或者说攻击者赶上诚实链的概率，计算如下：（具体可参见参考文献 8）<br>
>设：<br>
>*p*  = 诚实节点找到下一区块的概率。<br>
>*q*  = 攻击者找到下一区块的概率。<br>
>*q<sub>z</sub>* = 攻击者从落后 z 个区块追上诚实的链条的概率
<br>

![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin81.png?raw=true)<br>
假设 *p* > *q*， 随着攻击者必须追赶的区块的数量增加，概率 *q<sub>z</sub>* 呈指数下降。如果他没能在早期进行一次幸运的冲刺，那么随后成功的机会将越来越小，渐趋于零。<br>
我们现在考虑新交易收款者需要等待多长时间，才能充分确定付款者无法篡改交易。我们假设发起支付者是攻击者，他希望收款人在一定时间范围内相信他付了钱，而经过一段时间后，将交易款返还给自己。届时收款人会收到警告，但支付者希望到那时，为时已晚。<br>
收款者（被害人）生成一个新的密钥对，然后在签名前将公钥提供给付款人（攻击者）。我们假设即将签名前才提供公钥，以免付款人预先准备好一个区块链然后持续地对此区块进行运算，直到他的区块链幸运地超过诚实链条，然后立即执行交易。一旦交易发出，该不诚实的付款者（攻击方）就开始秘密地对一个包含其交易替代版本的并行链进行工作。<br>
收件人等待交易添加到块中，并且有 *z* 个块已经链接在它后面。他不知道攻击者取得的具体进展，但假设诚实的区块生成新快的时间为平均预期时间，攻击者的潜在进展将是具有预期值的泊松分布：<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin82.png?raw=true)<br>
为了得到攻击者依然可以赶上的概率，我们将他可以取得的每一个进展量的泊松密度乘以他从该点赶上的概率：<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin83.png?raw=true)<br>
重新排列以免对无限数列求和...<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin84.png?raw=true)<br>
转换为C代码......

```
#include <math.h>
double AttackerSuccessProbability(double q, int z)
{
    double p = 1.0 - q;
    double lambda = z * (q / p);
    double sum = 1.0;
    int i, k;
    for (k = 0; k <= z; k++)
    {
        double poisson = exp(-lambda);
        for (i = 1; i <= k; i++)
            poisson *= lambda / i;
        sum -= poisson * (1 - pow(q / p, z - k));
    }
    return sum;
}
```
从运行结果，我们可以看到概率随着 z 的增大呈指数下降。
```
q=0.1
z=0   P=1.0000000
z=1   P=0.2045873
z=2   P=0.0509779
z=3   P=0.0131722
z=4   P=0.0034552
z=5   P=0.0009137
z=6   P=0.0002428
z=7   P=0.0000647
z=8   P=0.0000173
z=9   P=0.0000046
z=10  P=0.0000012
q=0.3
z=0   P=1.0000000
z=5   P=0.1773523
z=10  P=0.0416605
z=15  P=0.0101008
z=20  P=0.0024804
z=25  P=0.0006132
z=30  P=0.0001522
z=35  P=0.0000379
z=40  P=0.0000095
z=45  P=0.0000024
z=50  P=0.0000006
```
求解 P 小于 0.1%...
```
P < 0.001
q=0.10 z=5
q=0.15 z=8
q=0.20 z=11
q=0.25 z=15
q=0.30 z=24
q=0.35 z=41
q=0.40 z=89
q=0.45 z=340
```

## HiFrank 解读
- 这一部分通过概率计算，演示了攻击者成功的概率极小的事实。
- 英文论文翻译为中文。C 代码翻译成什么呢？ 好吧，人生苦短，我来翻译成 Python, 并把它做完整，你可以拷贝粘贴直接运行出结果：<br>
```
import math
def atk_prb(q,z):
    p=1-q
    lmd=z*(q/p)
    s=1
    for k in range(0,z+1):
        pos=math.exp(-lmd)
        for i in range(1,k+1):
            pos*=lmd/i
        s-=pos*(1-math.pow(q/p,z-k))
    return s

print('假设 q=0.1,落后区块数z逐一递增，概率如下：')
for z in range(0,10):
    p=atk_prb(0.1,z)
    print('z=',z,'p=',p)
        
        
```
---
# 12. Conclusion
We have proposed a system for electronic transactions without relying on trust. We started with the usual framework of coins made from digital signatures, which provides strong control of ownership, but is incomplete without a way to prevent double-spending. To solve this, we proposed a peer-to-peer network using proof-of-work to record a public history of transactions that quickly becomes computationally impractical for an attacker to change if honest nodes control a majority of CPU power. The network is robust in its unstructured simplicity. Nodes work all at once with little coordination. They do not need to be identified, since messages are not routed to any particular place and only need to be delivered on a best effort basis. Nodes can leave and rejoin the network at will, accepting the proof-of-work chain as proof of what happened while they were gone. They vote with their CPU power, expressing their acceptance of valid blocks by working on extending them and rejecting invalid blocks by refusing to work on them. Any needed rules and incentives can be enforced with this consensus mechanism.
## 12.结论
本文提出了一种不依赖于信任的电子交易系统。我们从使用数字签名的常规货币框架开始，它提供了强有力的所有权控制，但其不能防止双重支付，所以并不完整。为解决此问题，我们提出了一种用**工作量证明**来记录交易的公开历史的点对点网络，如果诚实节点控制大部分CPU算力，从计算量上使得攻击者改变数据很快变得不切实际。该网络因其非结构化的简洁而非常健壮。节点只需极少的协调即可一起工作。由于消息不会路由到任何特定的地方，只需要尽力而为，所以它们不需要被识别。节点可以随时离开网络或重新加入，通过接受工作量证明链作为它们离开时产生的交易历史的证明。通过CPU算力投票，通过扩展它们来表达他们对有效块的接受，并通过拒用来拒绝无效块。任何所需的规则和激励措施都可以通过这种共识机制来实施。
## HiFrank 跋
>- 距离中本聪 2008 年发布此论文，已经过去十一年了。2009 年他发布了首个比特币软件 BitCoin，正式启动了比特币金融系统。此后，数字货币跌宕起伏，搅动着整个世界的金融体系，并使区块链技术扩展到各个领域。<br>
>- 在我们将“区块链作为核心技术自主创新的重要突破口”的新背景下，我们再次认真研读这篇论文，不忘初心砥砺前行，在底层技术基础上实现突破创新。<br>
>- 本译稿只求达意，不拘严谨，个人解读更是随意揣度，有失偏颇，仅供参考。建议充分阅读原文得其真趣。<br>
>- 本文同时在如下位置发布，以方便大家阅读：
>>- https://github.com/HiwebFrank/Blogs/blob/master/SatoshiNakamotoBitCoin-HiFrank.md
>>- https://blog.csdn.net/franklfeng/article/details/102791408
>>- https://mp.weixin.qq.com/s?__biz=MzU5NDgyODUyOQ==&mid=2247483757&idx=1&sn=244d68cc912ee2db3207bb7a18a40248
>- 码字不易，解读劳心。如果本文对您有所收获，欢迎转发，但请注明出处。也可以扫码请作者喝杯咖啡 :)
<div align="right">
<img src="https://github.com/HiwebFrank/Blogs/blob/master/imgs/HiRunner.jpg?raw=true" height="150",width="150"/>
</div>



# References  参考文献
1. W. Dai, "b-money," http://www.weidai.com/bmoney.txt, 1998.<br>
2. H. Massias, X.S. Avila, and J.-J. Quisquater, "Design of a secure timestamping service with minimal trust requirements," In *20th Symposium on Information Theory in the Benelux*, May 1999.<br>
3. S. Haber, W.S. Stornetta, "How to time-stamp a digital document," In *Journal of Cryptology*, vol 3, no 2, pages 99-111, 1991.<br>
4. D. Bayer, S. Haber, W.S. Stornetta, "Improving the efficiency and reliability of digital time-stamping," In *Sequences II: Methods in Communication, Security and Computer Science*, pages 329-334, 1993.<br>
5. S. Haber, W.S. Stornetta, "Secure names for bit-strings," In *Proceedings of the 4th ACM Conference on Computer and Communications Security*, pages 28-35, April 1997.<br>
6. A. Back, "Hashcash - a denial of service counter-measure," http://www.hashcash.org/papers/hashcash.pdf, 2002.<br>
7. R.C. Merkle, "Protocols for public key cryptosystems," In *Proc. 1980 Symposium on Security and Privacy*, IEEE Computer Society, pages 122-133, April 1980.<br>
8. W. Feller, "An introduction to probability theory and its applications," 1957.9
# 译者致谢
> 在本文翻译的过程中，参考了 *夜读春秋* 及 *柏拉图的双斜杠* 的博文，在此致谢。<br>
> 1. 中本聪比特币论文, 夜读春秋， https://blog.csdn.net/alading2009/article/details/78768309 <br>
> 2.  比特币创世论文《Bitcoin: A Peer-to-Peer Electronic Cash System》研读，柏拉图的双斜杠，https://blog.csdn.net/qq_27467365/article/details/81569962
