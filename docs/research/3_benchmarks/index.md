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
