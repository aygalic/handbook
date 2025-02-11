
Based on the following meta study [@huangSurveyHallucinationLarge2024], what are the methods for detecting hallucinations for a model.

## Detection

### Detection strategies taxonomy  

- Fact checking
	- External retrieval
	- Internal retrieval
- Incertitude quantification
	- LLM's internal states
	- LLM's behaviour


#### External retrieval

*Using an external knowledge base to compare with the model facts*

Example with **FActScore** [@minFActScoreFinegrainedAtomic2023]

![[Pasted image 20250125220307.png|452x367]]

- Step 1: Model generates biographies of public figures
- Step 2: Leverage InstructGPT to turn biographies into atomic facts
- Step 3: Use instruct Llama to label the atomic fact (“irrelevant”, ”supported”, “not supported”) based on a retrieved (Generalizable T5-based Retrievers) knowledge base (wikipedia)
- Step 4: Evaluate the proportion of fact that are supported

#### Internal retrieval

*Use the model to check its own replies*

Example: **Chain-of-Verification (CoVe)** [@dhuliawalaChainofVerificationReducesHallucination2023]

Prompt the model to:
1. Generate a draft
2. Plan verification questions (not templated)
3. Answer verification questions
4. Generate a final response

![[Pasted image 20250127160439.png|523x370]]

#### Internal states

Leveraging LLM's internal states such as the entropy of a given token or the perplexity of a sentence to infer on the hallucination state.

Example of an end to end implementation :  *A Stitch in Time Saves Nine*: Detecting and Mitigating Hallucinations of LLMs by Validating Low-Confidence Generation [@varshneyStitchTimeSaves2023]

1. Generating sentense tokens
2. Key concept identification (using AI)
3. Uncertainty quantification (Using the lowest probability of any token in a given concept)
4. Generating validation questions (using AI)
5. Knowledge retrieval (external)
6. Question answering
7. Sentence fix

Authors achieved a recall of 88% and a mitigation rate of 56%
The false positive didn't have any impact on performances.


![[Pasted image 20250127161356.png|453x302]]

#### Behavioural approach

Use behavioural tools to implement fact verification approaches. This set of tools is usually used when no other technique is applicable (for instance, when the API doesn't give access to token probability).

SELFCHECKGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models [@manakulSelfCheckGPTZeroResourceBlackBox2023]

Uses multi sampling techniques.

Relies on the idea that if an LLM has been trained on a concept, the sampled ideas should be similar and contain consistent facts.

1. multi sampling
2. Use LLMs to check the consistency between prompts
3. Use the results to assert the probability of the sequence to be hallucinated

![[Pasted image 20250127164920.png|450x366]]



