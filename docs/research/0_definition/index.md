# Hallucination Definition

### Definition origin

Some important papers [@farquharDetectingHallucinationsLarge2024] [@filippovaControlledHallucinationsLearning2020]  and meta studies [@jiSurveyHallucinationNatural2023] [@dongMultiFactCorrectionAbstractive2020]   seems to point to the definition from a summarisation paper from Google [@maynezFaithfulnessFactualityAbstractive2020] (~900 citations)
### Definition

**Hallucinations** can be defined in the following way: "hallucination is typically referred to as a phenomenon in which the generated content appears nonsensical *(=! untruthful)* or unfaithful to the provided source content" [@maynezFaithfulnessFactualityAbstractive2020] 

### Faithfulness and Factuality

**faithfulness**: "a faithful model will generate a summary that only has information that is supported by its document [@maynezFaithfulnessFactualityAbstractive2020] ."

**Factuality**: "A summary S of a document D contains a factual hallucination if it contains information not found in D that is factually correct. Factual hallucinations may be composed of intrinsic hallucinations or extrinsic hallucinations" [@maynezFaithfulnessFactualityAbstractive2020] 

Faithfulness and Factuality are independent from one another.

==What about Truthfulness==
#### Examples :
Doc: "In the latest Microsoft keynote, windows 98 has just been released" 
The summary "windows 11 has just been released" is unfaithful but factual

==The summary "windows 98 has been released by Microsoft" is unfaithful but factual== adding external implicit knowledge leads to unfaithful content, here we dont know that microsoft is the company realising windows as it is not clear by the context. The statement is nonetheless factual

The summary "windows 98 has just been released" is faithful but untruthful (outdated)
### Intrinsic vs Extrinsic

in the context of summarisation, **intrinsic** hallucination are miss-representation of the source document while **extrinsic** hallucinations, are the generation of facts that are not present in the input document

These two concept are mutually exclusive.

Doc: "French prime minister just used the 49.3 to pass the budget" 
The summary "Dutch prime minister just used the 49.3 to pass the budget" is an intrinsic hallucination
The summary "The prime minister is very unpopular due to its use of article 49.3"  is an extrinsic hallucination

### The point for hallucinations:

Hallucination != bad : 

“hallucinations in summarisation are acceptable if they lead to better summaries that are factual with respect to the document and the associated background knowledge."

### Problem specific definition

Add task specific definition

### Interrogations

At which point do we consider that two things are the same ? 

"Earth is a Round" => "Earth is a Sphere" ? this implication is considered an extrinsic hallucination by my definition

How do we define factuality ?

an alternative definition of the problem from Dong et al. [@dongMultiFactCorrectionAbstractive2020] (where facts are interpreted as the input context, making faithfulness == factuality)

