# ipse-core

## IPSE挖矿设计

> 总体逻辑是PoS和PoC的结合，有Staking生息的部分，更有抵押PoC挖矿的部分。

- 出块逻辑Babe + Grandpa组合，出块节点110个，认证节点不限制。
	- 出块奖励每年为总发行量的10%。
	- 出块节点要求：线上服务器+工网ip。
	- 出块节点选举规则：获得选票前110名。
	- 持币者投票支持出块节点，获得出块奖励分红，相当于持币生息。
- 挖矿逻辑用module来实现。
	- 每3个区块时间（9秒）挖一次矿。让节点有充分时间来计算所提交的答案是否为最优。
	- 挖矿软件提交答案花费高昂手续费，只有确信自己的答案更优，然后提交答案并且挖到才能获益，否则亏损手续费。
	- 出块节点接收到答案后，在9秒内进行计算，最接近准确答案的挖到矿。
	- 挖矿成功后，查看抵押情况，进行结算。然后给出下一次挖矿时的初始条件。
	- 矿工每次成功挖矿后，其对应的P盘id就会发生一次随机变化，矿工需要获取新的id进行重新P盘来参与挖矿。当然挖矿软件都会自动完成这些工作。
- 挖矿结算逻辑。
	- 首先矿工有两组地址，一个是矿工地址，一个是抵押币地址列表（不能超过10个），也就是说矿工最多能让十个持币者来出币做抵押。
	- 矿主可以自定义分配比例，比如矿主分币70%，持币人分币30%。矿主可以选择接受10个持币者的抵押币，其他则退回给持币者。当然矿主也可以自己出币抵押。
	- 剩下就是抵押足够与否，共轭双挖条件是否达成。

## IPSE存储数据挖矿设计

IPSE中存储数据部分是业务层，引入的是稳定币机制，比如TUSD（1TUSD=1USD），存储空间定价，稳定币进行结算，完成数据存储，验证节点进行数据持有的挑战，存储矿工节点要完成PDP数据持有型证明。

- 存储矿工定价，同时根据空间大小进行抵押。
- 客户存储数据，获得收益，收益和抵押不能释放。
- 在存储合约期，可以逐步释放，但每次释放都需要完成PDP数据持有证明。


Proof-of-capacity blockchain built on
[Substrate](https://github.com/paritytech/substrate).

## Overview

Ipse-core is the underlying consensus layer of IPSE project, which is the basic version of the whole application chain. The function modules to be added in the future are all extended based on this core version.

Ipse-core is developed based on Substrate and will try some new consensus algorithms at the consensus layer, and is a consensus algorithm that can be combined with the storage disk. For example, the PoC consensus algorithm has been proved successful so far.
