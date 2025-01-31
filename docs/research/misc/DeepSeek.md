

# What makes DeepSeek V3 / R1 so ✨ special ✨

The breakthroughs are taking place over the year 2024, through the release of the V2, V3 and R1 papers.

## V2

The V2 papers introduce us to the architectural changes. In particular they leverage 2 powerful changes : MoE (deepSeekMoE as it is their own implementation) and Multi Head Latent Attention (MHA) (DeepSeek developed as well).

Those two changes intervene on two distinct layers of the transformer block:
- MoE takes place in the FFN
- MHA takes place in the attention block

Architecture : 

![[Pasted image 20250128161241.png]]

1. Multi head latent attention
	Low-rank joint compression for attention keys and values to reduce Key-Value (KV) cache during inference.
	Queries are also cached on the v3 paper
2. MoE
	Cross node MoE training - optimisation to significantly enhance training efficiency.
	Basically there are a set of **shared** experts that are active at all stages, as well as **routed** experts that can be routed on any given tokens.


## V3 

In the V3 paper, here are the new changes

1. Auxiliary-loss-free strategy for load balancing
	 Goal: minimising performance degradation due to load balancing 
4. Multi-token prediction training objective
	Beneficial to model performance + can be used for speculative decoding and inference acceleration
5. FP8 mixed precision
6. "DualPipe Algorithm" 


Training process:

1. Simple and **stable** pre-training on 14.8T token 
2. (2 stages) Context extension : original -> 32K -> 128K
3. Supervised fine tuning over 1.5M examples

The paper also goes deeper into the architectural as well as pipeline changes.
## R1-zero

R1-zero is their RL only approach to CoT reasoning emergence.

Basically they use a lot of complexe reasoning task that are close problems (which have one correct output such as the code compiling etc.) and they just reward the model when it produce an output between the \<think> \</think> markups, as well as the result being correct.

This improved the capabilities quite impressively, reaching performance levels close to o1 

## R1

R1-zero 

Post training:

Distillation from DeepSeek R1 (chain of thought model)


Takeways: cheaper to train than a 70B param model
On par with 400B param (or better)




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

These changes, when implemented together, provide a significan bump in performance, as is shown by deepseek v2 paper:

![[Pasted image 20250130173714.png]]

### V3 

V3 takes the ideas from v2, add some new pipeline tweaks and scales the model to get new performance.

- 236B to 671B parameters (21 to 37B active at every token)
- 14.8T token for pretraining
- 1.5M samples for fine tuning

- FP8
- Multi-token prediction training objective

Impressively cheap cost:

![[Pasted image 20250130174008.png]]

Though this doesn't account for ablation studies, data generation cost etc.
Also note that this table is for V3, not R1, CoT reasoning training comes at further cost.

### R1 on a potato ?

No. R1 is the size of V3, ~670B parameters. But [they offer distilled version](https://ollama.com/library/deepseek-r1) on Llama and Qwen.



![[Pasted image 20250129120314.png]]

### R1-Zero

RL Only approach which is very impressive
![[Pasted image 20250130173029.png]]

R1-zero has some kinks such as poor readability and language mixing.
### R1

Adding SFT as well as another step of RL in the pipeline to get better behaviour and performance

Training is done in 4 steps:

1. SFT to improve stability instead of cold start
2. R1-Zero RL pipeline
3. Data generation through rejection sampling -> SFT
4. RL for Helpfulness and harmlessness

R1 ends up with very solid performance:

![[Pasted image 20250130173405.png]]


And the distillations are great too:

![[Pasted image 20250130173423.png]]

Another impressive step is that the distillation process enable levels of performance that can't be attained on the RL pipeline of R1-zero only.
![[Pasted image 20250130173520.png]]

The R1 base model end up being 27x cheaper than contemporary o1 pricing per million token output