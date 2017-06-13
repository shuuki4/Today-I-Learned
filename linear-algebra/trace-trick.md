# Trace Trick

* 주로 Multivariate Normal Distribution을 가지고 적분할 일이 있을 때 많이 쓰이는 기법으로, 적분의 계산을 쉽게 하기 위해 스칼라 계산 값을 trace 값으로 바꾸어서 처리하는 기법
* trace의 다음과 같은 성질에 의해 성립된다
  * $tr\(AB\) = tr\(BA\)$ if dimensions work out \(증명: 직접 trace 계산해보면 자명\)
  * $tr\(c\) = c$
* 이러한 성질을 이용하면 다음과 같은 식을 쉽게 처리할 수 있음



