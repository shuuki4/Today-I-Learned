# HRED \(Hierarchical Recurrent Encoder-Decoder\)

## A Hierarchical Recurrent Encoder-Decoder for Generative Context-aware Query Suggestion

### Introduction

* Search Engine에서의 Query Suggestion 문제를 풀기 위해 제안된 모델

  * \(최근에 검색한\) 쿼리들의 sequence가 주어졌을 대, 다음으로 나타날 sequence of words를 예측함
  * 다음과 같은 성질들을 만족하고자 함

  * Synthetic \(Pairwise retrieval을 통해 찾는것이 아니라, 만들어내는 것도 가능하도록\)

  * Context-aware \(이전에 검색한 쿼리들의 context를 이용\)

  * Long-tail aware \(Popular 쿼리 뿐만 아니라 자주 등장하지 않는 쿼리들에도 robust하게 작동\)

* \(Token 단위의\) RNN based model -&gt; Synthetic and Context-aware and Long-tail aware..?

  * 그렇다고 하니...

### HRED architecture

* 기본적으로 2개 \(물론, generalize해서 N개로 확장 될것\) RNN을 가지고 있음
* Query 단에서 word 단위로 encoding / decoding 을 하는 'Query RNN' 과 query sequence를 모델링하는 'Context RNN'이 있음

* Query RNN들 같은 경우

  * Encoding RNN에서 query를 encoding 하고, final query encoding state를 Context RNN의 input으로 넣음

  * Context RNN의 state를 non-linear mapping 해서 Decoding RNN의 input으로 넣음

    * 이때, Decoding RNN에서 hidden -&gt; softmax로 바로 먹이는 것이 아니라, hidden 및 previous vocab을 concat해서 linear mapping 한번 때린 후 softmax 먹임

* Learning Objective는 typical log-likelihood, with BPTT learning

### Experiments

* 






