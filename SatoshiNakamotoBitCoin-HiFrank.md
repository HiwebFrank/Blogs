# 比特币创世文 Bitcoin: A Peer-to-Peer Electronic Cash System 解读
--- 

>>>一夜之间，区块链再次火起来。网络上大量的讲解分析区块链比特币的文章蜂拥而出。我是一个考据癖者，今晚不睡了，把区块链的始作俑者，中本聪的论文拿出来，做个中英文对照，分飨给大家。<br><br>
>>>我尽量意译而不直译，每部分后边再加一点我的解读。翻译不求严谨只求达意，要严谨请读原文。但即便如此，作为一篇专业论文，无论如何无法在这里将其解读到通俗易懂，请见谅。<br><br>
原文链接：[Bitcoin: A Peer-to-Peer Electronic Cash System](https://www.bitcoincash.org/bitcoin.pdf)
>>><p align="right"> ——  HiFrank  冯立超</p>

<br>

# Bitcoin: A Peer-to-Peer Electronic Cash System
## 比特币：一种点对点的电子现金系统
<p align="center">Satoshi Nakamoto  --- 中本聪<br>
satoshin@gmx.com<br>
www.bitcoin.org</p>

## Abstract. 
>> A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer network. The network timestamps transactions by hashing them into an ongoing chain of hash-based proof-of-work, forming a record that cannot be changed without redoing the proof-of-work. The longest chain not only serves as proof of the sequence of events witnessed, but proof that it came from the largest pool of CPU power. As long as a majority of CPU power is controlled by nodes that are not cooperating to attack the network, they'll generate the longest chain and outpace attackers. The network itself requires minimal structure. Messages are broadcast on a best effort basis, and nodes can leave and rejoin the network at will, accepting the longest proof-of-work chain as proof of what happened while they were gone.

### 概要
>> 纯点对点的电子现金系统，将允许从一方到另一方直接进行在线支付，而不需要通过一个金融机构。**数字签名**解决了部分问题，但如果仍需要一个可信的第三方机构来防止**双重支付**问题，这就是去了意义。我们提出了一种针对点对点网络解决双重支付问题的方案。这种网络通过将交易信息的**哈希值**添加到一条基于哈希的工作量证明来生长的链的方式将交易时间戳化。从而生成一种除非重新进行工作量证明，否则就不可修改的记录。最长的链不仅作为一系列事件的见证，同时也证明它来自于最大算力池。只要主要的算力在非恶意节点的控制之下，它们就会产生最长的链同时将攻击者拒之门外。这种网络本身只需要最小的结构。信息以尽力而为的方式广播，节点可以自由的离开或加入，接受以最长的工作量证明链作为这些节点下线时的记录。

### HiFrank 解读
- **哈希算法**、**数字签名**等，是确保信息安全的最基本概念。简单解释一下：一段数据， 对其进行某种运算，得到一个值。这段数据如果有篡改，这个算出来的值就会变。拿到数据后再算一下这个值是不是正确，就知道数据是否被改过。这是**哈希算法**的通俗解释。**数字签名**说来话长，暂不赘述。它基于哈希算法和加密算法提供了数据的可信性。这篇论文以上述概念为基础，但不做解释。就好像讲微积分默认你懂算数一样。
- 中本聪的这篇论文，关键要解决的问题是 **双重支付** ，即 **double-spending** 问题。现实生活中，硬币/钞票不可能重复使用，你把那张钱给我了，你就没了，你没办法再给另一个人（一女怎可二嫁）。但，电子现金，是一个数字信息，是可以复制的，可能重复使用的。这篇论文是要解决这个问题。
- 他的基本思路是：
