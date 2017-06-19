# Class weights

* Dataset이 Unbalance일 때, class 별로 weight을 다르게 줌으로서 unbalance를 맞추려는 heuristic

* 클래스가 있을 때, 각 클래스의 1 / \(전체 샘플 중 그 클래스 샘플의 비율  \* 클래스 갯수\) 로 맞춰서, balance 된 갯수보다 갯수가 적을 경우 weight을 높여주는 방식으로 사용

* https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/utils/class\_weight.py

  * compute class weight function





