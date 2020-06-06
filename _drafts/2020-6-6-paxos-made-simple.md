---
layout: post
title: "外文翻译- Paxos Made Simple"
categories: [分布式]
description: ""
permalink: /paxos-made-simple/
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

此文是我毕业论文的外文翻译部分，翻译的是大名鼎鼎的Leslie Lamport于2001年写的论文《Paxos Made Simple》

{% raw %}
***          
{% endraw %}

# 摘要
用英语描述Paxos算法是非常简单的。
# 1．引言
用来实现容错分布式系统的Paxos算法被很多人认为是难以理解的，也许是因为原始讲稿对于大部分读者来说太Greek了。事实上，Paxos算法是最简单并且最容易实现的分布式算法。Paxos算法的核心是共识算法-“synod”算法。下一章节会说明该共识算法基本上满足了我们的需求。最后一个章节会展示完整的Paxos算法，通过直接应用状态机来构建分布式系统---这种方法应该是众所周知的，在分布式系统中被最多引用的文章主题。
# 2．共识算法 
## 2.1 问题
假设一些进程可以提出value(Paxos协议达成一致的过程：propose value)。共识算法保证了在所有提出的values中，最终只有一个value会被选中。如果没有提出任何value，那么一个都不会被选中。当一个value被选中后，其他进程可以学到所选的value。安全达成共识要求如下：

- 只能选择提出的value           
- 只有一个value被选中                
- 一个进程只会学习已经被选中的value                

我们不会尝试指定具体的要求。但目标是保证某一个提出的value被最终选中，而且如果这个value被选中，其他进程能够学习到这个value.我们把共识算法中的agent分为3类：proposers, acceptors和learners。在实际实现过程中，一个进程会扮演不止一个agent，但具体的agent到进程的映射在这里我们并不关心。

假设agent之间可以通过传输信息交流。我们通常使用异步的非拜占庭模型，其中：

- Agents以任意速度运行，可能失败停止，可能重启。因为选中一个value后，所有的agents都可能因为失败而重启，所以agents需要能够记住信息，不然无法解决这个问题。         
- 传递的消息可以花费很长时间传递，可能重复，也可能丢失，但不会损坏。
## 2.2 选择一个Value
最简单的选择一个value的方法是只有一个acceptor agent。Proposer agent发起提案到acceptor，acceptor选择最早收到的一个value。虽然这种方法很简单，但这个解决方案不让人满意，因为acceptor的故障会导致后续所有操作都变成不可能。

因此我们需要尝试另外一个选择value的方法。我们使用多个acceptor agents替代单个acceptor agent。一个proposer发送value到多个acceptor agents。其中一个acceptor也许会接受这个value，当足够多数量的acceptor agents接受这个value后，这个value才被最终选定。为了保证只有一个value被选中，我们让多数的agents为超过半数的agents。因为任意2个多数的agents都会有一个公共的agent，当一个agent只选中一个value时，这个方法可行。

在没有故障或者消息丢失的情况下，我们想要只有一个value被选中，即使只有一个value被一个proposer提出。需要满足以下需求：

P1. 一个acceptor必须接受收到的第一个提案

但这个要求会产生其他问题。不同的proposers可能会同时提出不同的values，导致每个acceptor都接受了一个value，但没有一个Value是被多数接受的。即使只有2个提案的values，如果他们各自被一半的acceptors接受，那么任意一个acceptor的故障都会让我们无法学习到被选中的value。

P1和value只有被多数acceptor接受才算选中，暗示我们的acceptor必须允许接受多于一个提案。我们给每个提案一个提案号，因此每个提案由一个提案号和value组成。为了防止冲突，我们要求不同的提案拥有不同的提案号。我们仅仅在假设实现，具体实现可能有所不同。一个value被选中，当一个提案被多数acceptors接受。在这种情况下，我们说这个提案被选中。

我们允许选中多个提案，但必须保证所有选中的提案必须有一样的value。通过归纳提案号，可以保证：

P2. 如果一个value为v的提案被选中，那么被选中的提案号更高的提案也有value v。

因为提案号是有序的，条件P2保证了只有单一value被选中的特性。

为了被选中，一个提案必须被至少一个acceptor接受，所以我们可以通过满足以下条件满足P2:

P2a 如果一个value为v的提案被选中，那么被任意一个acceptor选中的具有更高提案号的提案都有value v。

我们仍然需要满足P1来保证提案被选中。因为消息传递是异步的，一个提案可能被一些没有接受过任意提案的acceptor c选中。假设一个proposer 恢复正常，并发送了一个带有不同value的具有更高提案号的提案

P1要求acceptor c接受这个提案，但这却违背了P2a。为了同时维持P1和P2a，我们需要加强P2a：

P2b. 如果一个value为v的提案被选中，那么每个proposer发出的具有更高提案号的提案必须拥有value v。 

因为一个提案由proposer发出，因此满足P2b也就满足了P2a,也就满足了P2.

为了明白如何满足P2b，我们先对此进行证明。假设一个value为v，提案号为m的提案已经被选中，那么任何提案号n>m的提案也有value v。我们可以使用归纳法，归纳到n。假设提案号从m..n-1的提案都有value v，那么必然有大多数acceptor接受了这些提案，这些acceptor都接受了提案号在m..n-1，并拥有value v的提案。

因此我们可以通过满足一下条件来确保提案号为n的提案拥有value v：

P2c. 对任意的v和n，如果一个提案号为n并且value为v的提案被发出，那么存在一个由多数acceptor组成的集合S。那么S中没有acceptor接受过提案号小于n的提案，要么v是S中acceptor接受过的提案号小于n的最大提案的value。


因此我们可以通过满足P2c来实现P2b.

为了P2c的不变性，如果proposer想要发布一个提案号为n的提案，那么这个proposer必须获得提案号小于n中最大的提案，如果有这样的提案被多数选中了。学习已经被选中的提案是非常简单的，预测未来的接受情况是困难的。为了不用预测未来，proposer发出承诺不会有这样的案例。换而言之，proposer要求acceptors不接受任何提案号小于n的提案。这就导出了如下用于发布提案的算法：

1.一个proposer选择一个提案号n，并发送请求到大部分acceptors，要求它们回复：    
           
(a) 承诺不再接收提案号小于n的提案

(b) 如果已经接收过了提案，返回小于n的最大提案号的提案。

我称这样的请求为提案号为n的prepare request。

2.如果proposer收到大部分acceptors的回复，那么它可以发送提案号为n，value为v—其中v要么是返回的提案号最高的提案的value，要么是proposer选中的value。

Proposer将提案发送给一些acceptors，并请求acceptors接收(这些acceptors并不一定和初始的acceptors是一样的)。我们称这样的请求为accept request。

这里描述的是proposer 的算法，那么acceptor呢？acceptor可以接收从proposers发出的2种请求：prepare requests 和 accept requests。Acceptor可以忽略任何请求而不影响安全性。所以，我们只讨论acceptor可以回复请求的情况。Acceptor永远可以回复prepare request。有时可以回复accept request，这代表接受提案，除非它没有承诺不这么做。换而言之：

P1a. 一个acceptor可以接受提案号为n的提案，如果它之前没有回复过提案号大于n的prepare request。

可以发现P1a包含P1。

现在我们有一个完整的算法来选择一个满足如下条件的value—假设提案号唯一。最终的算法是改算法的优化版本。

假设一个acceptor收到一个提案号为n的提案，但它已经回复了提案号大于n的提案的prepare request，那么它不会接受任何提案号为n的提案。Acceptor不会回复这个prepare request，因为它肯定不会接受这个提案。因此我们让acceptor忽视类似的请求。同样我们也让它忽视已经接受过的提案的prepare request。

通过这样的优化，一个acceptor需要记住它接受的提案的最高提案号以及它回复过的最高提案号的prepare request。

因为P2c要求即使发生故障，一个acceptor必须记住这些信息，即使发生了故障或者重启。需要注意的是proposer可以随时抛弃自己的提案—只要它不发生具有相同提案号的提案。

将proposer 和acceptor的行为放在一起，我们可以看到算法的执行分为2个阶段。

Phase 1. (a) 一个proposer选择一个提案号n，并向大部分acceptors发送提案号为n的prepare request

(b) 如果acceptor收到一个prepare request，并且提案号n大于任何它已经回复的prepare request的提案号，那么这个acceptor回复这个请求，并保证不再接收提案号小于n的提案。

Phase 2. (a) 如果proposer收到了大部分的acceptors 对prepare requests的回复，随后它将发送accept request到这些acceptors，带上提案号n和value v，其中v是收到的回复中提案号最大的提案的value，如果没有收到提案，这个value是自己选定的任意值。

(b) 如果 acceptor收到accept request，其中的提案号为n，并且它并没有回复过提案号大于n的提案，那么它应该接受这个提案。

一个 proposer 可以生成多个提案，只要它能满足算法的要求。Proposer可以随时在协议过程中抛弃提案。当其他proposer发出提案号更高的提案后，比较明智的做法是放弃当前提案。因此，如果一个acceptor忽视prepare request 或者accept request，因为它已经接受了提案号更高的提案，它应该通知对应的proposer放弃该提案。这是针对性能优化，并不影响正确性。

## 2.3 学习选择的Value
为了学习已经被选中的value，learner必须先确定一个被大多数acceptor接受的提案。

一个显而易见的算法是每个acceptor接受一个提案后，向所有的learner发送这个提案。这样允许learner尽快发现被选中的value，但这样要求每个acceptor对所有的learner都进行回复-相当于回复acceptor数量与learner数量的乘积。非拜占庭式的失败假设允许一个learner从另一个learner那里学到已经被接受的value。

我们可以让acceptors把自己的接受情况发给一个主learner，让其他learner从主learner中学习哪些value被选中。这个方法需要所有的learner进行额外的一轮学习。同时这个方法也是不可靠的，因为主learner可能会发生故障。需要发生的回复数跟之前的一样多。

更一般些，acceptors可以发送接收情况给一个主learners的集合，其中任意一个都能够在value被选中时通知所有的learners。随着集合的变大，在通信的过程中拥有更高的可靠性。

因为存在消息丢失的情况，learner可能不知道一个value已经被选中。Learner可以直接询问acceptors接受了哪个提案，但acceptors的故障可能让我们无法知道一个提案是否被多数acceptors接受。在这种情况下，learner只有在新的提案被接受后才能知道value的值。如果一个learner想要知道哪个value被确认了，可以让proposer发起新的提案。

## 2.4 进展
很容易构建这样的场景：两个proposers持续发送比对方提案号高的提案，最终所有提案都没有被接受。Proposer p使用提案号n1完成阶段1，另一个proposer q使用提案号n2>n1完成阶段1. Proposer p在阶段2的accept request会被拒绝，因为acceptor承诺不再接受提案号小于n2的提案。所以Proposer p重新使用新的提案号n3>n2完成阶段1，导致proposer q的阶段2被终止。

为了流程的顺利进行，我们必须选出一个主proposer，只有它才能发送提案。如果主proposer可以跟大部分acceptors通信，然后选出一个最大的提案号，就能成功发送提案并被接收。通过放弃现存的提案，使用更高的提案号，主proposer会最终选择一个足够大的提案号。

如果系统足够多的部分可以正常运行，那么选举单一的主proposer就可以使系统正常运行。Fischer, Lynch, and Patterson有个注明的结论：实现一个选举proposer的可靠算法必须使用随机或者实时性-例如，超时。然而，无论选举是否成功，安全性都足以保证。

## 2.5 实现
Paxos算法假设一个进程网络。在共识算法中，每个进程扮演角色：proposer, acceptor和 learner。算法会选择一个leader担任主proposer和主learner。Paxos 共识算法如上面所述，请求和回复都是普通信息。我们需要持久性的存储来保证故障时acceptor能记住信息。Acceptor会在发送回复前记下信息。

接下来的内容是描述如何确保机器不会产生相同的提案号-对于不同的提案。不同的proposers会根据自己的集合选择不同的数字，所以不同的proposers永远不会产生相同的提案号。每个proposers会记住自己提过的最大的提案号，然后在阶段1时选择一个更大的提案号。

# 3．实现状态机
实现分布式系统最简单的方法是让clients向一个中央主服务器发送请求。主服务器可以被看作一个以一定顺序执行client命令的的确定性状态机。状态机拥有现在的状态，并根据输入产生输出，并移动到下一个状态。比如说分布式银行系统的客户端可能是柜员，并且状态机由所有用户的账户余额组成。取钱的操作通过执行状态机命令：当且仅当余额大于取的钱时，减少余额，输出新旧余额。

一种实现方法是使用单一的中央主服务器，如果主服务器发生故障，所有服务失败。因此，我们采用servers的集合，每个服务器单独实现自己的状态机。因为状态机是确定的，所以在执行完相同顺序的命令后，会产生相同的输出的状态结果。Client可以使用任何一个服务器返回的结果。

为了保证所有的服务器都执行同样顺序的状态机命令，我们事先一个序列的Paxos共识算法，第i 个实例选择的value作为第i个状态机的状态。每个server实例都运行所有role: proposer, acceptor和 learner。现在我们假设servers的集合是固定的，此共识算法的所有实例都是用同样的agents。

在平时的操作中，一个server被选举成为leader，并成为唯一的主proposer，在所有的共识算法实例中。客户端发送命令给leader，leader觉得这个命令应该放在序列中的什么位置。假设leader决定这个命令应该成为第135个命令，那么它会尝试把该命令变成第135个实例选择的value。这通常都会成功。但有可能失败，当发生故障时，或者另一个server以为自己是leader，然后对第135个实例进行另外的操作。但共识算法保证了只会有一个第135条命令。

Paxos共识算法的关键在于value的值在阶段2时才被确认。回想一下，在阶段1完成后，要么value的值已经被决定了，要么value的值可以自己随便选。

我现在要描述的是在正常运行时，Paxos的状态机如何运行。以及什么情况下会出现问题。比如说旧的leader刚刚发生故障，新的leader还没选出。

新的leader，也就是共识算法中的learner，应该知道大多数已经被选中的value。假设它知道命令1-134,138,139(接下来我们会知道空隙是如何产生的)。然后它执行实例135-137的阶段1,和所有大于129的实例。假设执行结果确定了实例135和140的value，但其他实例仍然未知。然后leader对实例135和140执行阶段2，从而选中了135和140的命令。

Leader和其他获得了leader知道的命令的服务器可以执行命令1-135.但仍然不能执行命令1138-140，因为命令136和137仍未被选中。Leader可以将接下来的2个命令填充到命令136和137中。但我们选择填充空指令到136和137中，这样状态机的状态不会改变。当空指令被选中后，命令138-140可以立即执行。

命令1-140都被选中。Leader完成了对于所有大于140的实例的阶段1，现在可以在阶段2中选择任意的value。然后分配命令号131给客户端发出的请求，并把值作为命令的值。随后把客户端的下一个请求作为命令142，如此以往。

Leader可以提出命令142在学习到命令141的value之前。可能命令141的数据发生丢失，命令142在其他服务器学习到命令141前被选中。当leader没有收到实例141的阶段2的回复后，它会重复发送消息。如果运行正常，发送的信息会被选中。但也有可能在一开始失败，留下一个空白的间隙。通常来说，假设leader可以提前获得a个命令—即在1-i已经被选中的情况下，提出i+1-i+a个命令。一个最多为a-1的空隙是有可能出现的。

一个新的leader可以执行无限多实例的阶段1-在上述场景中，实例135-137以及大于139的实例。对所有实例使用同一个提案号，这样一次消息传递即可完成。在阶段1中，如果acceptor收到阶段2的消息，那么acceptor会回复不仅仅是一个简单的OK。因此，server作为acceptor可以用一个简单的消息回复所有的实例。在无数个实例的阶段1中，这是没有问题的。

因为leader的故障和新leader的选举都是小概率事件，因此执行状态机有效花费—即，在命令/value上达成共识的开销—也就是阶段2的执行。可以看出Paxos共识算法在阶段2的过程中具有所有能达成共识的允许故障的算法中的最小开销。因此Paxos算法本质上是最优的。

对于系统正常运行的假设是单一leader，除了旧leader发生故障以及新leader选举的过程中。在异常情况下，leader的选举可能失败。如果没有服务器当leader，那么新的命令自然不能被执行。如果多个服务器任务它们是leader，那么它们可以在共识算法的同一实例中推出不同value，这会导致没有value被选中。但安全性是保留的—2个不同的服务器不会在第i个状态机命令上达成一致。选举单一leader只是为了确保流程顺利。

如果server的集合可以改变，那么必须有一种方法确定哪些服务器确定了共识算法中的哪些实例。最简单的方法是通过状态机本身。当前的server的集合可以作为状态的一部分，并被普通的状态机命令改变。我们允许leader提前获得a个命令，然后执行i+a的实例，在执行完第i个实例的情况下。这就是一种简单的实现对复杂的重新配置的算法。

# 4．参考文献
[1] Michael J. Fischer, Nancy Lynch, and Michael S. Paterson. Impossibility
of distributed consensus with one faulty process. Journal of the ACM,
32(2):374–382, April 1985.              
[2] Idit Keidar and Sergio Rajsbaum. On the cost of fault-tolerant consensus
when there are no faults—a tutorial. TechnicalReport MIT-LCS-TR-821,
Laboratory for Computer Science, Massachusetts Institute Technology,
Cambridge, MA, 02139, May 2001. also published in SIGACT News
32(2) (June 2001).              
[3] Leslie Lamport. The implementation of reliable distributed multiprocess
systems. Computer Networks, 2:95–114, 1978.              
[4] Leslie Lamport. Time, clocks, and the ordering of events in a distributed
system. Communications of the ACM, 21(7):558–565, July 1978.
[5] Leslie Lamport. The part-time parliament. ACM Transactions on Com-
puter Systems, 16(2):133–169, May 1998.              

