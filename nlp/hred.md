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

![](/assets/hred-fig-1.png)

* 기본적으로 2개 \(물론, generalize해서 N개로 확장 될것\) RNN을 가지고 있음
* Query 단에서 word 단위로 encoding / decoding 을 하는 'Query RNN' 과 query sequence를 모델링하는 'Context RNN'이 있음

  * 



