# HRED \(Hierarchical Recurrent Encoder-Decoder\) and its Variants

## A Hierarchical Recurrent Encoder-Decoder for Generative Context-aware Query Suggestion

### Introduction

* Search Engine에서의 Query Suggestion 문제를 풀기 위해 제안된 모델

  * \(최근에 검색한\) 쿼리들의 sequence가 주어졌을 대, 다음으로 나타날 sequence of words를 예측함
  * 다음과 같은 성질들을 만족하고자 함

    * Synthetic \(Pairwise retrieval을 통해 찾는것이 아니라, 만들어내는 것도 가능하도록\)

    * Context-aware \(이전에 검색한 쿼리들의 context를 이용\)

    * Long-tail aware \(Popular 쿼리 뿐만 아니라 자주 등장하지 않는 쿼리들에도 robust하게 작동\)

* \(Token 단위의\) RNN based model -&gt; Synthetic and Context-aware and Long-tail aware..?

  * 그렇다고 하니... 근데 너무 거창한 인트로 적어놓고 RNN으로 퉁치려는듯한..

### HRED architecture

* 기본적으로 2개 \(물론, generalize해서 N개로 확장 될것\) RNN을 가지고 있음
* Query 단에서 word 단위로 encoding / decoding 을 하는 'Query RNN' 과 query sequence를 모델링하는 'Context RNN'이 있음

* Query RNN들 같은 경우

  * Encoding RNN에서 query를 encoding 하고, final query encoding state를 Context RNN의 input으로 넣음

  * Context RNN의 state를 non-linear mapping 해서 Decoding RNN의 input으로 넣음

    * 이때, Decoding RNN에서 hidden -&gt; softmax로 바로 먹이는 것이 아니라, hidden 및 previous vocab을 concat해서 linear mapping 한번 때린 후 softmax 먹임

* Learning Objective는 typical log-likelihood, with BPTT learning

### Experiments

* 다음과 같은 모델들을 사용

* 원래 있는 co-occurrence based candidate generation으로 candidate들을 만듬 \(ADJ\)

* 여기에, supervised context-aware ranker \(Baseline Ranker\)를 먹여서 ranking

  * LambdaMART라고 하는 supervised ranker 사용

* 2의 ranker에 HRED에서 나온 likelihood score를 더해서 feature로 사용

* Mean Reciprocal Rank \(MRR\)로 evaluate

#### Exp 1: 일반적인 세팅

* 일반적인 세팅에서 실험시, ADJ 0.5334, Baseline 0.5563, HRED 0.5749: Significant under t-test
* 특히, Short보다는 Medium/Long 세팅에서 훨씬 성능이 올라간것을 발견

#### Exp 2: Noisy Query

* 실험 1의 세팅에, 중간중간 굉장히 frequent한 쿼리를 넣음 \(google, facebook.. 등\) -&gt; HRED가 이러한 쿼리에 robust한지 보기위함
* 이 경우 ADJ 0.4507, Baseline 0.4831, HRED 0.5309
* Hierarchical Structure가 frequent query가 들어올 때 update gate를 block할 수 있기 때문이라고 주장

#### Exp 3: Long-tail Query

* Anchor query \(prev query\) 에 Long-tail이 들어올 때에 대한 실험

* ADJ 0.3830, Baseline 0.6788, HRED 0.7112

  * 이 케이스에서 원래 성능보다 Baseline / HRED가 좋게 나오는 이유는, 분석해보니 이 케이스에서 noisy query가 적었기 때문이라고 함



