Reading notes about DQN

Reading notes in Playing Atari with Deep Reinforcement Learning



在background里介绍reinforcement learning, CNN 和 deep learning， 可以把slide上的公式搬上去



Abstract:

The model is a convolutional neural network, trained with a variant of Q-learning, whose input is raw pixels and whose output is a value function estimating future rewards.



Introduction:

This paper demonstrates that a convolutional neural network can overcome these challenges to learn successful control policies from raw video data in complex RL environments.



**experience replay**

we store the agent’s experiences at each time-step, $e_t = (s_t, a_t, r_t, s_{t+1})$ in a data-set $D = e_1, ...,e_N$, pooled over many episodes into a **replay memory**.

在内层循环里，执行Q-learning的学习，就是从replay memory中随机采样.





发现了一个作者妥协的地方：Since using histories of arbitrary length as inputs to a neural network can be difficult, our Q-function instead works on fixed length representation of histories produced by a function $\phi$.



另一个：In practice, our algorithm only stores the last N experience tuples in the replay memory, and samples uniformly at random from D when performing updates. 

This approach is in some respects limited since the memory buffer does not differentiate important transitions and always overwrites with recent transitions due to the finite memory size N.

他文章中提到的方法是设置一个固定的值N，每次都会把transition放进去并且挤掉第一个，没有什么判断条件，不能区分出transition的重要程度，并且会覆盖掉之前的信息。直观上，我们可以：

不设置固定的N,而是动态的调整。或者我们对transition的重要程度进行量化，每次都只保留重要程度最大的N个transition。

再一个：

Similarly, the uniform sampling gives equal importance to all transitions in the replay memory. A more sophisticated sampling strategy might emphasize transitions from which we can learn the most, similar to prioritized sweeping.

文章里提到的从经验池里选取样本的方法是等概率随机抽样，这样就会忽视样本之间重要性程度的区别。直观上，我们可以让样本选取的概率与它的重要性成正比，建立一个概率分布。



**深度神经网络：提取复杂特征**

DL与RL结合存在以下问题 ：

DL是监督学习需要学习训练集，强化学习不需要训练集只通过环境进行返回奖励值reward，同时也存在着噪声和延迟的问题，所以存在很多状态state的reward值都是0也就是样本稀疏
DL每个样本之间互相独立，而RL当前状态的状态值是依赖后面的状态返回值的。
当我们使用非线性网络来表示值函数的时候可能出现不稳定的问题