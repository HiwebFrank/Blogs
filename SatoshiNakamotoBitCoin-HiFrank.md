# 比特币创世文 Bitcoin: A Peer-to-Peer Electronic Cash System 解读
--- 

>>>一夜之间，区块链再次火起来。网络上大量的讲解分析区块链比特币的文章蜂拥而出。我是一个考据癖者，刚好周末，跑完步回来，把区块链的始作俑者，中本聪的论文拿出来，整理一篇中英文对照，分飨给大家。<br><br>
>>>我尽量意译而不直译，每部分后边再加一点我的解读。翻译不求严谨只求达意，要严谨请读原文。即便如此，作为一篇专业论文，无论如何无法在这里将其解读到通俗易懂，请见谅。<br><br>
论文链接：[Bitcoin: A Peer-to-Peer Electronic Cash System](https://www.bitcoincash.org/bitcoin.pdf)
>>><p align="right"> ——  HiFrank  冯立超</p>

<br>

# Bitcoin: A Peer-to-Peer Electronic Cash System
## 比特币：一种点对点的电子现金系统
<p align="center">Satoshi Nakamoto  --- 中本聪<br>
satoshin@gmx.com<br>
www.bitcoin.org</p>

# Abstract. 
>> A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer network. The network timestamps transactions by hashing them into an ongoing chain of hash-based proof-of-work, forming a record that cannot be changed without redoing the proof-of-work. The longest chain not only serves as proof of the sequence of events witnessed, but proof that it came from the largest pool of CPU power. As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network, they'll generate the longest chain and outpace attackers. The network itself requires minimal structure. Messages are broadcast on a best effort basis, and nodes can leave and rejoin the network at will, accepting the longest proof-of-work chain as proof of what happened while they were gone.

## 概要
>> 纯点对点的电子现金系统，将允许从一方到另一方直接进行在线支付，而不需要通过一个金融机构。**数字签名**解决了部分问题，但如果仍需要一个可信的第三方机构来防止**双重支付**问题，这就是去了意义。我们提出了一种针对点对点网络解决双重支付问题的方案。这种网络通过将交易信息的**哈希值**添加到一条基于哈希的工作量证明来生长的链的方式将交易时间戳化。从而生成一种除非重新进行工作量证明，否则就不可修改的记录。最长的链不仅作为一系列事件的见证，同时也证明它来自于最大算力池。只要主要的算力在非恶意节点的控制之下，它们就会产生最长的链同时将攻击者拒之门外。这种网络本身只需要最小的结构。信息以尽力而为的方式广播，节点可以自由的离开或加入，接受以最长的工作量证明链作为这些节点下线时的记录。

## HiFrank 解读
- **哈希算法**、**数字签名**等，是确保信息安全的最基本概念。简单解释一下：一段数据， 对其进行某种运算，得到一个值。这段数据如果有篡改，这个算出来的值就会变。拿到数据后再算一下这个值是不是正确，就知道数据是否被改过。这是**哈希算法**的通俗解释。**数字签名**说来话长，暂不赘述。它基于哈希算法和加密算法提供了数据的可信性。这篇论文以上述概念为基础，但不做解释。就好像讲微积分默认你懂算数一样。
- 中本聪的这篇论文，关键要解决的问题是 **双重支付** ，即 **double-spending** 问题。现实生活中，硬币/钞票不可能重复使用，你把那张钱给我了，你就没了，你没办法再给另一个人（一女怎可二嫁）。但，电子现金，是一个数字信息，是可以复制的，可能重复使用的。这篇论文是要解决这个问题。
- 他的基本思路是：



# 1. Introduction
Commerce on the Internet has come to rely almost exclusively on financial institutions serving as trusted third parties to process electronic payments. While the system works well enough for most transactions, it still suffers from the inherent weaknesses of the trust based model. Completely non-reversible transactions are not really possible, since financial institutions cannot avoid mediating disputes. The cost of mediation increases transaction costs, limiting the minimum practical transaction size and cutting off the possibility for small casual transactions, and there is a broader cost in the loss of ability to make non-reversible payments for nonreversible services. With the possibility of reversal, the need for trust spreads. Merchants must be wary of their customers, hassling them for more information than they would otherwise need. A certain percentage of fraud is accepted as unavoidable. These costs and payment uncertainties can be avoided in person by using physical currency, but no mechanism exists to make payments over a communications channel without a trusted party.
What is needed is an electronic payment system based on cryptographic proof instead of trust, allowing any two willing parties to transact directly with each other without the need for a trusted third party. Transactions that are computationally impractical to reverse would protect sellers from fraud, and routine escrow mechanisms could easily be implemented to protect buyers. In this paper, we propose a solution to the double-spending problem using a peer-to-peer distributed timestamp server to generate computational proof of the chronological order of transactions. The system is secure as long as honest nodes collectively control more CPU power than any cooperating group of attacker nodes.
## 1. 简介
互联网上的商业活动已经变得几乎完全仰仗于作为可信任的第三方金融机构来处理电子支付。虽然这套系统对于绝大多数交易都是足够的，但它依旧面临这种基于信任模型的固有缺陷。完全不可逆的交易是不可能实现的，因为金融机构不可避免的调解纠纷。调解过程所产生的成本会增加交易成本，限制了最小实际交易规模，隔断小额随机交易的可能性，更广泛的成本会因为缺失为不可逆服务而提供不可逆交易的能力而产生。因为（交易被）逆转的可能性，对信任的需求进一步增加。商人必须对它的客户保持警惕，麻烦他们获取更多在另一种情况（客户不履约）下会需要的信息。一个确定的比例的诈骗是在可接受氛围以内的并被认为是无可避免的。这些成本和支付的不确定性可以通过使用物理现金而被避免，但没有一个不存在可信方又能让支付通过交流渠道进行的机制存在。

我们需要的是一个基于加密证明的而不信任的数字支付系统，允许任意两方不通过可信第三方机构直接进行交易。交易是在计算上不可逆的，这会保护销售者免遭诈骗。常规的第三方保管机制就可以轻易实施来保护买家。在这篇论文里，我们提出一种运用分布式点对点时间戳化服务来生成交易信息和次序的充足计算证明来防止双重支出的解决方案。这套系统只要在主要的算力在非恶意节点的控制之下就是安全的。
## HiFrank 解读

# 2. Transactions
We define an electronic coin as a chain of digital signatures. Each owner transfers the coin to the next by digitally signing a hash of the previous transaction and the public key of the next owner and adding these to the end of the coin. A payee can verify the signatures to verify the chain of ownership.<br>

![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin1.png?raw=true)<br>

The problem of course is the payee can't verify that one of the owners did not double-spend the coin. A common solution is to introduce a trusted central authority, or mint, that checks every transaction for double spending. After each transaction, the coin must be returned to the mint to issue a new coin, and only coins issued directly from the mint are trusted not to be double-spent. The problem with this solution is that the fate of the entire money system depends on the company running the mint, with every transaction having to go through them, just like a bank.<br>
We need a way for the payee to know that the previous owners did not sign any earlier transactions. For our purposes, the earliest transaction is the one that counts, so we don't care about later attempts to double-spend. The only way to confirm the absence of a transaction is to be aware of all transactions. In the mint based model, the mint was aware of all transactions and decided which arrived first. To accomplish this without a trusted party, transactions must be publicly announced[1], and we need a system for participants to agree on a single history of the order in which they were received. The payee needs proof that at the time of each transaction, the majority of nodes agreed it was the first received.

## 2. 交易
我们定义了一种由数字签名组成的链为基础的数字货币。每一个币的拥有者可以通过对上一笔交易和下一个人的公匙进行签名并把这两个签名添加到币的末端的方式来把币支付给下一个人。收款人可以通过验证签名来验证对付款者的所有权进行验证。

当然，问题是收款者还是没办法确定付款者是否把他的比特币花了两次。一个通常的解决方案是引入一个可信的中心权威机构，或者铸币厂，来检查每一笔交易是否存在双重支付问题。在每笔交易之后，必须将币退回铸币厂来产生新的币，只有直接由铸币厂发行的币才被认为不存在双重支付问题。这种解决方案的问题是这整个货币体系的命运都取决于负责运营铸币厂的公司，每一笔交易都得通过他们，就像一个银行。

我们需要一种方法来让收款人知道之前的持有者没有签过任何更早的交易。为了这个目标，最早的交易就是算数的那个，所以我们不关心晚一点的试图双重支付的企图。只有一个办法来确认缺失的交易那就是知道所有的交易。在一个基于铸币厂的模型里，铸币厂知道所有的交易它决定哪些更早发生。为了在不引入可信方的前提下来实现相同的效果，交易必须被公开广播，同时我们还需要一套系统让参与者认同他们收到的是同一个交易次序的历史收款人需要证明在每次交易的时候，大多数节点认同它是首先到达的。
## HiFrank 解读

# 3. Timestamp Server
The solution we propose begins with a timestamp server. A timestamp server works by taking a hash of a block of items to be timestamped and widely publishing the hash, such as in a newspaper or Usenet post [2-5]. The timestamp proves that the data must have existed at the time, obviously, in order to get into the hash. Each timestamp includes the previous timestamp in its hash, forming a chain, with each additional timestamp reinforcing the ones before it.<br>
![2](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin2.png?raw=true)<br>
## 3.时间戳服务器
我们提供的解决方案从时间戳服务器说起。一个时间戳服务器是靠对多个项目组成的区块进行哈希来得到时间戳，并在如报纸或Usenet网广泛发布该哈希值而得以工作，时间戳证明数据在那个时间点一定是存在的，明显的，为了得到哈希值，每个时间戳的哈希值中都包含之前的时间戳，从而形成一条链，每一个新增的时间戳都对其上一个进行加强。
## HiFrank 解读

# 4. Proof-of-Work
To implement a distributed timestamp server on a peer-to-peer basis, we will need to use a proofof-work system similar to Adam Back's Hashcash [6], rather than newspaper or Usenet posts. The proof-of-work involves scanning for a value that when hashed, such as with SHA-256, the hash begins with a number of zero bits. The average work required is exponential in the number of zero bits required and can be verified by executing a single hash.<br>
For our timestamp network, we implement the proof-of-work by incrementing a nonce in the block until a value is found that gives the block's hash the required zero bits. Once the CPU effort has been expended to make it satisfy the proof-of-work, the block cannot be changed without redoing the work. As later blocks are chained after it, the work to change the block would include redoing all the blocks after it.<br>
![3](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin3.png?raw=true)<br>
The proof-of-work also solves the problem of determining representation in majority decision making. If the majority were based on one-IP-address-one-vote, it could be subverted by anyone able to allocate many IPs. Proof-of-work is essentially one-CPU-one-vote. The majority decision is represented by the longest chain, which has the greatest proof-of-work effort invested in it. If a majority of CPU power is controlled by honest nodes, the honest chain will grow the fastest and outpace any competing chains. To modify a past block, an attacker would have to redo the proof-of-work of the block and all blocks after it and then catch up with and surpass the work of the honest nodes. We will show later that the probability of a slower attacker catching up diminishes exponentially as subsequent blocks are added.<br>
To compensate for increasing hardware speed and varying interest in running nodes over time, the proof-of-work difficulty is determined by a moving average targeting an average number of blocks per hour. If they're generated too fast, the difficulty increases.
## 4.工作量证明
为了实现基于点对点网络的分布式时间戳服务器，我们需要使用一种和Adam Back's Hashcash相似的工作量证明系统，比起报纸或Usenet网。工作量证明牵扯到在生成哈希时扫描一个值，以SHA-256为例，这个哈希值以一系列的零比特开始。平均工作量随着开头零的个数要求的增加而越来越大并可以通过执行一次hash来验证。<br>
对于我们的时间戳网络来说，我们通过在区块中增加一个临时值直到一个可以让这个区块的哈希值达到要求的值被找到来实现工作量证明。一旦CPU算力被花费来使其满足工作量证明，除非重做工作量证明这个区块就不能被修改。当之后的区块加入进来，修改区块的工作量将包含重算之后所有的工作量证明。<br>
工作量证明也解决了在主要决策中确定代表性的问题如果多数派的确定是靠IP地址一IP一票，它就会被可以产生多个IP的节点操纵。工作量证明是必要的一CPU一票的机制。最长链就是主要决策的代表，其中包含最多的工作量，如果大多数CPU算力被非恶意节点所控制，那么诚实的链就会比所有与它竞争的链都生长的更快并超过他们。为了修改一个之前的区块，攻击者必须重算这个区块以及这个区块之后所有区块的工作量证明，然后赶上并超过非恶意节点。我们待会儿会落后的攻击者赶上的可能性随着区块的增加呈指数级减少。<br>
为了匹配硬件速度的增加，和运算节点的变化，工作量证明的难度被变化的每小时平均生成区块数所决定，如果它们产生的过快，就会增加难度。
## HiFrank 解读

# 5. Network
The steps to run the network are as follows:

1) New transactions are broadcast to all nodes.
2) Each node collects new transactions into a block.
3) Each node works on finding a difficult proof-of-work for its block.
4) When a node finds a proof-of-work, it broadcasts the block to all nodes.
5) Nodes accept the block only if all transactions in it are valid and not already spent.
6) Nodes express their acceptance of the block by working on creating the next block in the chain, using the hash of the accepted block as the previous hash.

Nodes always consider the longest chain to be the correct one and will keep working on extending it. If two nodes broadcast different versions of the next block simultaneously, some nodes may receive one or the other first. In that case, they work on the first one they received, but save the other branch in case it becomes longer. The tie will be broken when the next proofof- work is found and one branch becomes longer; the nodes that were working on the other branch will then switch to the longer one.<br>
New transaction broadcasts do not necessarily need to reach all nodes. As long as they reach many nodes, they will get into a block before long. Block broadcasts are also tolerant of dropped messages. If a node does not receive a block, it will request it when it receives the next block and realizes it missed one.

## 4.网络
运行网络的步骤如下：
1) 新的交易被广播到所有节点。
2) 每一个节点都把新的交易收集到区块链中。
3) 每个节点都试图找到适用于其区块的工作量证明。
4) 一个节点找到了适用于其区块的工作量证明，它将其广播到所有节点。
5) 节点当且仅当该被广播的区块中所有交易都合法以及没有双重支付的前提下才接受该区块。
6) 节点通过使用该被广播的区块中的哈希值开始计算下一个区块来表示其接受该被广播的区块。

节点永远将最长链视为正确的链并基于它开始工作尝试延长它。如果两个节点同时广播了不同版本的下一区块，一些节点可能会先收到一个或先收到另一个。在这种情况下，它们基于它们先收到的那个开始工作，但同时保存另一个以防万一它变成了更长的链。当下一个区块被找到其中一个分支就会变得更长，这种情形就会被打破；哪些基于另一条分支工作的节点就会切换到更长的那条链上。<br>
新交易的广播没有必要到达所有的节点。只要到达很多节点，它们很快就会进入区块。区块的广播同样也可以容忍消息丢失，如果一个节点没有收到一个区块，当它收到下一个区块时，就会意识到它丢失了一个区块，并会去请求它。
## HiFrank 解读

# 6. Incentive
By convention, the first transaction in a block is a special transaction that starts a new coin owned by the creator of the block. This adds an incentive for nodes to support the network, and provides a way to initially distribute coins into circulation, since there is no central authority to issue them. The steady addition of a constant of amount of new coins is analogous to gold miners expending resources to add gold to circulation. In our case, it is CPU time and electricity that is expended.<br>
The incentive can also be funded with transaction fees. If the output value of a transaction is less than its input value, the difference is a transaction fee that is added to the incentive value of the block containing the transaction. Once a predetermined number of coins have entered circulation, the incentive can transition entirely to transaction fees and be completely inflation free.<br>
The incentive may help encourage nodes to stay honest. If a greedy attacker is able to assemble more CPU power than all the honest nodes, he would have to choose between using it to defraud people by stealing back his payments, or using it to generate new coins. He ought to find it more profitable to play by the rules, such rules that favour him with more new coins than everyone else combined, than to undermine the system and the validity of his own wealth.
## 6.奖励机制
按照惯例，每个区块的第一笔交易都很特殊因为区块的创建者会在区块中的第一个交易里为自己创造新的比特币。这就激励了为网络提供支持的节点，鉴于没有中心来监管这件事，也提供了一条把币分发到流通系统的路径。稳定增加一定数量的新比特币的过程类似于黄金矿工花费资源为流通系统增加黄金。在我们的例子里，花费的资源是CPU工作时间和电能。<br>
激励也可以通过交易费用得到，如果交易收入的值小于支出的值，差额就被添加到包含这个交易的区块的奖励值中。一旦预定数量的比特币进入流通，激励就可以完全转换为交易费用并且这样做完全没有通货膨胀的风险。<br>
这样的激励机制也会促进节点保持可靠。如果一个贪婪的攻击者可以收集比所有诚实节点更多的CPU算力，他就会被迫在通过偷回他的付款骗人和用其生成新的货币之间作出选择。他会发现根据规则行事他会受益更多，这样的规则对他更有利，因为比起破坏系统和他自己的财富的有效性，他会获得比所有其他人加起来还要多的新比特币。
## HiFrank 解读

# 7. Reclaiming Disk Space
Once the latest transaction in a coin is buried under enough blocks, the spent transactions before it can be discarded to save disk space. To facilitate this without breaking the block's hash,
transactions are hashed in a Merkle Tree [7][2][5], with only the root included in the block's hash. Old blocks can then be compacted by stubbing off branches of the tree. The interior hashes do not need to be stored.<br>
![4](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin4.png?raw=true)<br>
A block header with no transactions would be about 80 bytes. If we suppose blocks are generated every 10 minutes, 80 bytes * 6 * 24 * 365 = 4.2MB per year. With computer systems typically selling with 2GB of RAM as of 2008, and Moore's Law predicting current growth of 1.2GB per year, storage should not be a problem even if the block headers must be kept in memory.
## 7.回收磁盘空间
一旦最新的交易被埋在足够的区块底下，它之前的交易可以被丢以节约磁盘空间。为了在不损毁区块哈希的情况下使这个过程更便捷，交易在Merkle树被哈希化，只有根保存着区块的哈希。旧区块可以通过截断树的分支的方式来压缩。 内部哈希不需要存储。<br>
一个没有交易信息的区块头大概80 bytes大小。如果我们假设区块每10分钟产生，80 bytes * 6 * 24 * 365 = 4.2MB 每年.在2008年在售的典型计算机系统拥有2GB的内存，摩尔定律预测当前的增长为1.2GB每年，就算区块头你存着交易信息，存储也应该不是问题。
## HiFrank 解读

# 8. Simplified Payment Verification
It is possible to verify payments without running a full network node. A user only needs to keep a copy of the block headers of the longest proof-of-work chain, which he can get by querying network nodes until he's convinced he has the longest chain, and obtain the Merkle branch linking the transaction to the block it's timestamped in. He can't check the transaction for himself, but by linking it to a place in the chain, he can see that a network node has accepted it, and blocks added after it further confirm the network has accepted it.
<br>
![5](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin5.png?raw=true)<br>
As such, the verification is reliable as long as honest nodes control the network, but is more vulnerable if the network is overpowered by an attacker. While network nodes can verify transactions for themselves, the simplified method can be fooled by an attacker's fabricated transactions for as long as the attacker can continue to overpower the network. One strategy to protect against this would be to accept alerts from network nodes when they detect an invalid block, prompting the user's software to download the full block and alerted transactions to confirm the inconsistency. Businesses that receive frequent payments will probably still want to run their own nodes for more independent security and quicker verification.

## 8. 简化支付验证
不通过运行一个全节点来验证支付是可能的，一个用户只需要保存一份最长工作量证明链区块头的拷贝，通过不断向网络节点请求就可以获得该拷贝，直到它确信它拿到的是最长链。并且包含将交易连接到将其时间戳化的区块里的Merkle 分支。他不能自行检查交易，但通过将他连接到链的某一个位置，他可以看见一个节点已经接受它了，添加在其之后的区块确认节点已经接受了它。<br>
如此一来，这样的验证过程只要可信节点控制着网络就是可靠的，但如果网络被攻击者控制，他就更加脆弱。当网络节点可以为自己确认交易，只要攻击者还可以继续控制网络，这种简单的方案就可能会被攻击者的虚假交易欺骗。一种抵抗这种攻击的保护策略是从节点接收它们检测到非法区块的警报提示用户的软件下载完整的块并警告交易以确认不一致。为了更多的独立安全性和更快的验证，经常收到支付的生意可能依旧希望运行它们自己的全节点。
## HiFrank 解读

# 9. Combining and Splitting Value
Although it would be possible to handle coins individually, it would be unwieldy to make a separate transaction for every cent in a transfer. To allow value to be split and combined, transactions contain multiple inputs and outputs. Normally there will be either a single input from a larger previous transaction or multiple inputs combining smaller amounts, and at most two outputs: one for the payment, and one returning the change, if any, back to the sender.<br>
![6](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin6.png?raw=true)<br>
It should be noted that fan-out, where a transaction depends on several transactions, and those transactions depend on many more, is not a problem here. There is never the need to extract a complete standalone copy of a transaction's history.
## 9.值的结合与分离
虽然单独的处理每一笔币（交易）是可能的，但为每一笔钱单独进行交易的话就会让转账变得很笨拙。为了允许值的结合与分离，交易包含多个收入和支出。通常来说，这里既会有有来自于之前的大笔交易对资金也会有多个小笔资金汇集的组合作为输入，而最主要的两项输出：一是支付的资金，另一项是找零，如果有的话，就会退回到支付账户。<br>
当一笔交易建立在几笔交易的基础上，而那些交易又建立在更多的交易之上时应该注意输出，这不会成为问题。永远不需要提取事务历史的完整独立副本。
## HiFrank 解读

# 10. Privacy
The traditional banking model achieves a level of privacy by limiting access to information to the parties involved and the trusted third party. The necessity to announce all transactions publicly precludes this method, but privacy can still be maintained by breaking the flow of information in another place: by keeping public keys anonymous. The public can see that someone is sending an amount to someone else, but without information linking the transaction to anyone. This is similar to the level of information released by stock exchanges, where the time and size of individual trades, the "tape", is made public, but without telling who the parties were.<br>
![7](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin7.png?raw=true)<br>
As an additional firewall, a new key pair should be used for each transaction to keep them from being linked to a common owner. Some linking is still unavoidable with multi-input transactions, which necessarily reveal that their inputs were owned by the same owner. The risk is that if the owner of a key is revealed, linking could reveal other transactions that belonged to the same owner.
## 10.隐私
传统的银行模型通过对相关方和可行第三方限制信息实现了一定程度的隐私保护。公开宣布所有交易的方式排除了这种方法的必要性，但通过打破另一个地方的信息流依然可以保证一定程度的隐私。通过保持公钥匿名，公众可以看到某人正在向某个其他人发送交易，却没有将交易链接到任何人的信息。这和证券交易所发布的信息水平很类似，储存时间和每个交易的大小的“磁带”，是公开的，但不会告诉你这些相关方是谁。<br>
作为外加的防火墙，应对每个交易使用新的密钥对以防止它们与公共所有者链接。在有多个输入的情况下，一些链接依旧是不可避免的，这必然表明他们的投入归同一所有者所有。这样做的风险在于如果钥匙的所有者被揭露，链接可以揭示属于同一所有者的其他交易。
## HiFrank 解读

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
我们考虑攻击者试图比诚实链更快地生成替代链的情景。就算攻击者完成了这一步骤，也不会讲系统至于可随意修改的境地，例如凭空创造价值或拿走从不属于攻击者的钱。节点不会接受一个不合法的支付，诚实的节点永远不会接受一个包含这样交易的区块。攻击者只能试图改变一个他自己的交易拿回他不久前花出去的钱。
诚实节点和攻击者之间的竞争可以被特征化为Binomial Random Walk。成功的事件是诚实的链被延伸一个区块，将领先优势提高+1，并且失败事件是攻击者的链被延长一个块，将间隙减少-1。
攻击者从特定亏损中赶超的可能性类似于赌徒的破产问题。假设一个拥有无限信用的赌徒开始出现赤字并且可能会进行无数次试验以试图达到盈亏平衡。我们可以计算他达到盈亏平衡的概率，或者说攻击者赶上诚实的链条的概率，就像接下来这样。
>*p*= 诚实节点找到下一区块的可能性。<br>
>*q*= 攻击者找到下一区块的可能性。<br>
>*q<sub>z</sub>*= 攻击者从落后z个区块赶上诚实的链条的概率
<br>

![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin81.png?raw=true)<br>
就算我们假设 *p* > *q*, 随着攻击者必须赶上的数量增加，概率呈指数下降。如果他没有能够在早期进行一次幸运的冲刺，那么随着他会落后更远，他的机会就会变得越来越小。
我们现在考虑新交易的收款者需要等待多长时间才能充分确定付款者无法篡改交易。我们假设支付者是一个希望收款人在一定时间范围内相信他付了钱的攻击者，他会经过一段时间后，将交易返还给自己。收款人那时会收到警告，但支付者希望那时就太晚了。
收款者生成一个新的钥匙对，然后并在签署前不久将公钥提供给付款人。这防止了付款人通过持续工作，直到他足够幸运，远远超前准备一连串的区块，再在这个时候执行交易。一旦交易发出，这个不诚实的付款者就对一个包含其交易的备用版本的并行链进行工作。
收件人开始等待，直到交易已添加到块并且有z块已经被添加在它后面。他不知道攻击者取得的确切进展，但假设诚实的区块花费了每块的平均预期时间，攻击者的潜在进展将是具有预期值的泊松分布：<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin82.png?raw=true)<br>
为了得到攻击者依然可以赶上对可能性，我们将他本可以取得的每一个进展量的泊松密度乘以他从那一点赶上来的概率：<br>
![ ](https://github.com/HiwebFrank/Blogs/blob/master/imgs/BitCoin83.png?raw=true)<br>
重新排列，以避免总结分布的无限尾...<br>
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
跑出一些结果，我们可以看到可能性随着z的增大呈指数下降。
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
解出P小于 0.1%...
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

# 12. Conclusion
We have proposed a system for electronic transactions without relying on trust. We started with the usual framework of coins made from digital signatures, which provides strong control of ownership, but is incomplete without a way to prevent double-spending. To solve this, we proposed a peer-to-peer network using proof-of-work to record a public history of transactions that quickly becomes computationally impractical for an attacker to change if honest nodes control a majority of CPU power. The network is robust in its unstructured simplicity. Nodes work all at once with little coordination. They do not need to be identified, since messages are not routed to any particular place and only need to be delivered on a best effort basis. Nodes can leave and rejoin the network at will, accepting the proof-of-work chain as proof of what happened while they were gone. They vote with their CPU power, expressing their acceptance of valid blocks by working on extending them and rejecting invalid blocks by refusing to work on them. Any needed rules and incentives can be enforced with this consensus mechanism.
## 12.结论
我们提出了一种不依赖信任的电子交易系统。我们从常规的通过数字签名的货币框架开始，它提供了强有力的所有权控制，但如果没有办法防止双重支出，这将是不完整的。为了解决这个问题我们提出了一种用工作量证明来记录公开的历史交易记录的点对点网络。如果诚实的节点控制大部分CPU算力，从计算的角度上讲让攻击者改变数据很快就会变得不切实际。这个网络虽然结构简单却非常健壮。节点一起工作却几乎不需要协调。因为消息不会路由到任何特定的地方，只需要尽力而为，所以它们不需要身份认证，节点可以自由的加入或离开网络，以工作量证明链作为它们离开时产生的交易历史的证明。它们通过CPU算力投票，通过扩展它们来表达他们对有效块的接受，并通过拒绝对它们进行处理来拒绝无效块。任何所需的规则和激励措施都可以通过这种共识机制来实施。
## HiFrank 解读

# References  参考文献
1. W. Dai, "b-money," http://www.weidai.com/bmoney.txt, 1998.<br>
2. H. Massias, X.S. Avila, and J.-J. Quisquater, "Design of a secure timestamping service with minimal trust requirements," In *20th Symposium on Information Theory in the Benelux*, May 1999.<br>
3. S. Haber, W.S. Stornetta, "How to time-stamp a digital document," In *Journal of Cryptology*, vol 3, no 2, pages 99-111, 1991.<br>
4. D. Bayer, S. Haber, W.S. Stornetta, "Improving the efficiency and reliability of digital time-stamping," In *Sequences II: Methods in Communication, Security and Computer Science*, pages 329-334, 1993.<br>
5. S. Haber, W.S. Stornetta, "Secure names for bit-strings," In *Proceedings of the 4th ACM Conference on Computer and Communications Security*, pages 28-35, April 1997.<br>
6. A. Back, "Hashcash - a denial of service counter-measure," http://www.hashcash.org/papers/hashcash.pdf, 2002.<br>
7. R.C. Merkle, "Protocols for public key cryptosystems," In *Proc. 1980 Symposium on Security and Privacy*, IEEE Computer Society, pages 122-133, April 1980.<br>
8. W. Feller, "An introduction to probability theory and its applications," 1957.9