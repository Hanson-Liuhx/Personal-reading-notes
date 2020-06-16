learning+idm

第一个问题是，之前的idm是只找到一个IC机制，那么怎么找到所有的IC机制？这一问题已被解决，需要阅读ICDA: Incentive Compatible Diffusion Auction.

第二个问题是，之前对买家的valuation分布没有相关考虑，现在要给买家引入一个与他们的valuation相关的先验假设 ，最终求出一个optimal max E[$v_i$]. 我们希望用神经网络来算出网络里的参数，但神经网络很难描述IC机制，用GNN来解决这一问题。所以还要看一下GNN相关的知识。

