

## Hallucination classification

Reference :  [A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions](https://arxiv.org/abs/2311.05232)

### Definition

Hallucinations in large language model are in some sense analogous to what they mean in the context of a human being, but there are some nuances. The oxford dictionary define them as "*an experience involving the apparent perception of something not present*", in the context of NLP, *hallucination* refers to a phenomena where the generated content appears to be non sensical or unfaithful to the provided context. This materialise in a way that is sort of similar to human hallucination but this, as if the model was dreaming.

We distinguish two kind of hallucination: intrinsic and extrinsic (unfaithful and untruthful).
#### Faithfulness hallucination 

Faithfulness hallucinations happen when the model fails to follow or conflict with the provided context when generate new token.

#### Truthfulness hallucination

Truthfulness hallucinations happen when the model statements cannot be verified in the context or a knowledge base.

![[Pasted image 20250124112926.png]]

## Detection

## Benchmarks
