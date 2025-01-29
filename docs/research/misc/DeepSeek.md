

# What makes DeepSeek V3 / R1 so ✨ special ✨

From the V3 paper : 

Architecture / Infra: 

![[Pasted image 20250128161241.png]]

1. Multi head latent attention
	Low-rank joint compression for attention keys and values to reduce Key-Value (KV) cache during inference.
	Queries are also low rank compressed ???
1. MoE
	Cross node MoE training - optimisation to significantly enhance training efficiency
3. Auxiliary-loss-free strategy for load balancing
	 Goal: minimising performance degradation due to load balancing 
4. Multi-token prediction training objective
	Beneficial to model performance + can be used for speculative decoding and inference acceleration
5. FP8 mixed precision
6. "DualPipe Algorithm" ???


Training process:

1. Simple and **stable** pre-training on 14.8T token 
2. (2 stages) Context extension : original -> 32K -> 128K
3. Supervised fine tuning + Reinforcement Learning 

Post training:

Distillation from DeepSeek R1 (chain of thought model)


Takeways: cheaper to train than a 70B param model
On par with 400B param (or better)


### Architecture

There are two important architecture changes in the transformer block

![[Pasted image 20250128161241.png]]

One on the attention layer, one in the feed-forward Network.

#### Multi head latent attention

While Llama3 uses Grouped-querry, attention

![[Pasted image 20250128170832.png]]

This leads to a compressed space that caches into 5% of the original size (20x saving in memory)

In the attention layer, MLHA is implemented, where they project key, values but ALSO queries (as NOT shown in the following graph).

Prior work : 
- Flash Attention - 2~4x speedup on GPT-2 
- Flash Attention 2 - 5~9 (2 time speedup over Flash Attention)

![[Pasted image 20250129110228.png]]


#### DeepSeekMoE

Then you have the DeepSeekMoE architecture, with use fine(er)-grained experts and isolate/group them.

![[Pasted image 20250128175719.png]]

The innovation here seems to be the fact that we have some shared expert (which are all activated at every token) and routed experts, which are selected at every token by the router.



### R1 on a potato ?


https://ollama.com/library/deepseek-r1

![[Pasted image 20250129120314.png]]

