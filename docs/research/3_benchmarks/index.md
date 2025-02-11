## Benchmarks

Most of the existing benchmark relies on extensive dataset and simple metrics.

One recent approach  from Hugging Face [@huangSurveyHallucinationLarge2024] has been to aggregate those benchmark into a more [unified leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard#/).

We will discuss individual benchmarks and their functioning in this page. 
### Truthfulness Benchmarks

#### Closed book QA:

- **NQ open**, Google, 2019: derived from Natural Questions [@kwiatkowskiNaturalQuestionsBenchmark2019] the first large publicly available data set to pair real (google) user queries with high-quality annotations of answers in (wikipedia) documents.
  The dataset is composed of quadruples question-evidence-longAnswer-shortAnswer. Scores on the benchmark at the time present a high upper bound, with good human performance.
  End up being a leaderboard where RAG model perform best, but most haven't been tested.
  
- **TriviaQA** [@joshiTriviaQALargeScale2017], University of Washington (+ a non profit founded by a Microsoft co-founder), 2017: Triples question-excerpt-answer with rich syntax  and complexe phrasing of non important/relatively funny facts (taken in part from online Trivia Quizz).
  The issue with this dataset compared to NQ is that those facts are generally niche/useless and the metrics on this benchmark are less relevant to everyday tasks.
  
- **PopQA** [@mallenWhenNotTrust2023], University of Washington (+ a non profit founded by a Microsoft co-founder), ACL 2023.
  The goal of the study was to highlight the need for RAG when using long tail knowledge (rarely occurring information within the training data) in comparison with popular knowledge (Pop in PopQA).
  PopQA is supposed to cover such long tail knowledge. Wikipedia views are used to determine how popular is a question.
  The study highlights 3 main findings: 
	  - With those dataset, scaling up models does not significantly improve the performance.
	  - Retrieval-augmented LMs are particularly competitive when subject entities are not popular. Surprisingly, retrieval augmentation can hurt the performance of  LLMs on questions about popular entities as the retrieved context can be misleading."
	  - We can chose wether or not to use RAG based on their provided popularity metric, increasing performances on PopQA by up to 10%.

- **TruthfulQA** [@linTruthfulQAMeasuringHow2022], University of Oxford/Open AI 2022 : benchmark composed of question that some human would answer falsely due to false beliefs or misconceptions. Judges the ability of a model to avoid replicating human misconceptions. 
  The performance difference between audited human and best model is 94% vs 58% respectively.
  Interesting findings: **the largest model were generally the least truthful** and fine tuning is a better approach to enhance performances rather than learning more text coming from internet.

Missing: SimpleQA (openAI)
#### Fact-Checking

- FEVER [@thorneFEVERLargescaleDataset2018], University of Sheffield, Amazon, 2018: Fact Extraction and VERification. Taking claims from wikipedia passages and verifying wether or not the claims are supported by the document (using human annotation).  Challenging test as the model successfully achieved only 32%.
  The reliance on human annotation make it so it is hard to really evaluate the inherent quality of the dataset (most humans agree on 70% of the labels).

#### Hallucination detection

- True-False [@azariaInternalStateLLM2023] ACL 2023: based on a set of sentences and the LLM internal states as it predicts the sentence, train a classifier that output the probability of a statements truthfulness. Classifier achieve 71~83% accuracy based on the model.

### Faithfulness Benchmarks

#### Summarisation

- XSum [@narayanDontGiveMe2018] University of Edinburgh, 2018: Requires the model to resonate on the document instead of simply extracting facts.
  Since it was written in 2018, it propose a CNN to work on the task associated with the Dataset.
  The dataset is composed of article from the BBC and single sentence summaries. The summary sentence is typically written by the author of the article.
  This benchmark can be used to evaluate how well a model perform on long context comprehension. [@gaoEmpowerYourModel2023]
  
- CNN/Daily Mail [@hermannTeachingMachinesRead2015] [@chenThoroughExaminationCNN2016] , Google, NeurIPS 2015, Stanford, 2016 :
  Proposing a supervised learning dataset for reading comprehension. The dataset is structured in context-query-answer triples.
  1M corpus & queries.
  The author tackle the issue with an  attention/LSTM model, unsuccessfully so.
  The next paper ups the accuracy from. ~7% to ~70%. It is a nice complement to the first paper as they dive much deeper into the dataset.
  (metric: ROUGE-L, which assesses n-gram overlap with reference summaries)
  ==SHOULDN'T THIS BE IN READING COMPREHENSION== ?
  
- NQ-Swap (derived from Natural Questions, exact match)

#### Reading comprehension

- RACE (accuracy)
- SQuAD 2.0 (Exact Match)

#### Instruction following

- MemoTrap
- IFEval

#### Hallucination Detection

- FaithDial
- HaluEval
- HotpotQA

Models are evaluated by accuracy



Insights:
- LLM size has beneficial impact on reducing faithfulness and truthfulness hallucination. It has to be underlined that truthfulness benefits from a better uplift.

Fallback:
- these benchmark don't optimise the prompt for each individual LLM, meaning that the comparison might be unfair and benefits some LLMs better than others
- Benchmarks come and go in terms of popularity and it is hard to truly assess the progress of LLMs over time as they are rarely compared to the same metrics (MMLU aside)
- some benchmarks are classified as closed book when they are developed as open book. others are classified as summarisation benchmark when they are reading comprehension, indicating a blurry line between these subjects
- Some problematic about benchmark leakage should be taken in consideration [@xuBenchmarkingBenchmarkLeakage2024]



[1]

L. Huang _et al._, “A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions,” _ACM Trans. Inf. Syst._, p. 3703155, Nov. 2024, doi: [10.1145/3703155](https://doi.org/10.1145/3703155). Available: [http://arxiv.org/abs/2311.05232](http://arxiv.org/abs/2311.05232). [Accessed: Jan. 24, 2025]

[2]

T. Kwiatkowski _et al._, “Natural Questions: A Benchmark for Question Answering Research,” _Transactions of the Association for Computational Linguistics_, vol. 7, pp. 453–466, Aug. 2019, doi: [10.1162/tacl_a_00276](https://doi.org/10.1162/tacl_a_00276). Available: [https://doi.org/10.1162/tacl_a_00276](https://doi.org/10.1162/tacl_a_00276). [Accessed: Feb. 10, 2025]

[3]

M. Joshi, E. Choi, D. S. Weld, and L. Zettlemoyer, “TriviaQA: A Large Scale Distantly Supervised Challenge Dataset for Reading Comprehension.” arXiv, May 13, 2017. doi: [10.48550/arXiv.1705.03551](https://doi.org/10.48550/arXiv.1705.03551). Available: [http://arxiv.org/abs/1705.03551](http://arxiv.org/abs/1705.03551). [Accessed: Feb. 10, 2025]

[4]

A. Mallen, A. Asai, V. Zhong, R. Das, D. Khashabi, and H. Hajishirzi, “When Not to Trust Language Models: Investigating Effectiveness of Parametric and Non-Parametric Memories.” arXiv, Jul. 02, 2023. doi: [10.48550/arXiv.2212.10511](https://doi.org/10.48550/arXiv.2212.10511). Available: [http://arxiv.org/abs/2212.10511](http://arxiv.org/abs/2212.10511). [Accessed: Feb. 10, 2025]

[5]

S. Lin, J. Hilton, and O. Evans, “TruthfulQA: Measuring How Models Mimic Human Falsehoods.” arXiv, May 08, 2022. doi: [10.48550/arXiv.2109.07958](https://doi.org/10.48550/arXiv.2109.07958). Available: [http://arxiv.org/abs/2109.07958](http://arxiv.org/abs/2109.07958). [Accessed: Feb. 10, 2025]

[6]

J. Thorne, A. Vlachos, C. Christodoulopoulos, and A. Mittal, “FEVER: a large-scale dataset for Fact Extraction and VERification.” arXiv, Dec. 18, 2018. doi: [10.48550/arXiv.1803.05355](https://doi.org/10.48550/arXiv.1803.05355). Available: [http://arxiv.org/abs/1803.05355](http://arxiv.org/abs/1803.05355). [Accessed: Feb. 10, 2025]

[7]

A. Azaria and T. Mitchell, “The Internal State of an LLM Knows When It’s Lying.” arXiv, Oct. 17, 2023. doi: [10.48550/arXiv.2304.13734](https://doi.org/10.48550/arXiv.2304.13734). Available: [http://arxiv.org/abs/2304.13734](http://arxiv.org/abs/2304.13734). [Accessed: Feb. 10, 2025]

[8]

S. Narayan, S. B. Cohen, and M. Lapata, “Don’t Give Me the Details, Just the Summary! Topic-Aware Convolutional Neural Networks for Extreme Summarization.” arXiv, Aug. 27, 2018. doi: [10.48550/arXiv.1808.08745](https://doi.org/10.48550/arXiv.1808.08745). Available: [http://arxiv.org/abs/1808.08745](http://arxiv.org/abs/1808.08745). [Accessed: Feb. 11, 2025]

[9]

Y. Gao, L. Wang, J. Fang, L. Hu, and J. Cheng, “Empower Your Model with Longer and Better Context Comprehension.” arXiv, Jul. 27, 2023. doi: [10.48550/arXiv.2307.13365](https://doi.org/10.48550/arXiv.2307.13365). Available: [http://arxiv.org/abs/2307.13365](http://arxiv.org/abs/2307.13365). [Accessed: Feb. 11, 2025]

[10]

K. M. Hermann _et al._, “Teaching Machines to Read and Comprehend,” in _Advances in Neural Information Processing Systems_, Curran Associates, Inc., 2015. Available: [https://papers.nips.cc/paper_files/paper/2015/hash/afdec7005cc9f14302cd0474fd0f3c96-Abstract.html](https://papers.nips.cc/paper_files/paper/2015/hash/afdec7005cc9f14302cd0474fd0f3c96-Abstract.html). [Accessed: Feb. 11, 2025]

[11]

D. Chen, J. Bolton, and C. D. Manning, “A Thorough Examination of the CNN/Daily Mail Reading Comprehension Task.” arXiv, Aug. 08, 2016. doi: [10.48550/arXiv.1606.02858](https://doi.org/10.48550/arXiv.1606.02858). Available: [http://arxiv.org/abs/1606.02858](http://arxiv.org/abs/1606.02858). [Accessed: Feb. 11, 2025]

[12]

R. Xu, Z. Wang, R.-Z. Fan, and P. Liu, “Benchmarking Benchmark Leakage in Large Language Models.” arXiv, Apr. 29, 2024. doi: [10.48550/arXiv.2404.18824](https://doi.org/10.48550/arXiv.2404.18824). Available: [http://arxiv.org/abs/2404.18824](http://arxiv.org/abs/2404.18824). [Accessed: Jan. 24, 2025]