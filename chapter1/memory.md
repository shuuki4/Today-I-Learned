# Memory

## Multiprocessing

* 기본적으로 Multiprocessing을 할 때는 프로세스간 메모리를 공유하지 않기 때문에 이론적으로는 파일 시스템을 거쳐서 데이터를 주고받아야 함
  * Python의 경우, Queue 등을 이용해 프로세스간 통신을 할 경우 실제로는 데이터를 \(caller\) pickling -&gt; file -&gt; \(callee\) unpickling 하는 식으로 통신이 이루어짐
* 그러나 기본적으로 UNIX-based system에서는 process를 `fork()`할 때 copy-on-write 기반으로 움직이기 때문에, 데이터를 Read만 할 경우 실제로는 Shared Memory로 공유하게 되어 큰 read-only object들을 share할 때 메모리를 아낄 수 있음
* 그러나 Python에서 실제로 큰 데이터를 이런 방식으로 share할 경우, 메모리가 분리되는 현상이 발생함
  * 이유: Garbage Collector
    * [Instagram이 Python Garbage Collector를 없앤 이유](https://b.luavis.kr/python/dismissing-python-garbage-collection-at-instagram)
    *  Garbage Collection에 사용되는 `GC_HEAD`가 Object마다`PyObject` 구조체의 바로 옆에 붙어있고, Garbage collection을 하는 과정에서 얘들이 수정되면서 그 블록이 통째로 COW 되는 것 같음
    * GC 이외에도, 읽으면서 Reference Count가 증가/감소 될텐데 얘가 문제가 되지 않는 이유는?
      * 잘 모르겠다... Reference count가 `PyObject_HEAD`에 있어서 그럴거라고 생각하고 있엇는데, 이게 struct가 아니라 macro라서 reference count 자체는 계속 늘었다 줄었다가 할거 같은데...
  * Solution: Disable GC
    * `gc.disable()` 내지 `gc.set_threshold(0)`
    * 이러면 Garbage collection의 이득은 볼 수 없지만 기본적으로 CPython이 Reference count based 최적화는 해주기때문에 \(CPython의 Reference count decrease 매크로 자체가 ref count가 0이 되면 dealloc 함\) GC가 처리하는 순환참조등의 특별한 케이스가 아니면 괜찮을지도...? 물론 매우 조심해야할듯



