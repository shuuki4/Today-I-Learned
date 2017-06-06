# Coroutine

### Concept

* Generator의 확장판처럼 사용하는 기능
  * Generator는 내부에서 값을 만들고, 값이 나올때마다 `yield` 문을 사용해서 메인 코드에게 값을 넘겨주고, 그 외의 상황에는 돌아가지 않음
  * 코루틴도 비슷한 느낌이지만, Generator가 메인 코드의 서브로 돌아가는 느낌이라면 Coroutine은 메인 코드와 상호작용하면서 데이터를 주고받으면서 서로 다른 두개의 코드가 돌아가는 느낌
* [PEP380 - Syntax for Delegating to a Subgenerator](https://www.python.org/dev/peps/pep-0380/)

### Sample Usage

```py
# 이동평균 계산
def averager():
    total = 0.0
    count = 0
    average = None
    try:
        while True:
            term = yield average  #(1)
            total += term
            count += 1
            average = total / count
    except SomewhatException:  #(2)
        handle exception..
    finally:  #(3)
        print('Final Average: {}'.format(average))

===================================

coro_avg = averager()
next(coro_avg)  #(4)
coro_avg.send(10)
>> 10.0
coro_avg.send(20)
>> 15.0
coro_avg.close()  #(5)
>> Final Average: 15.0
```

* \(1\) Coroutine의 핵심은 `yield`문. Coroutine은 다음 yield문이 나올 때까지 진행하여 값을 반환하고, 다음 값이 들어오기를 기다린다. 이 코드의 경우,
  * \(4\)에서 `next(coro_avg)`를 실행한 경우, 코루틴 내부에서 처음 `yield`가 나왔을 때까지 진행. 이 경우 먼저 `yield average`문 때문에 average값이 튀어나오고 \(맨 처음엔 None\), 그 다음부터 메인 코드에서 `send`로 다음 값을 주기를 기다린다.
  * 메인 코드에서 `send`로 넘겨주는 값은 `yield` 부분에 바인딩이 되어, `term` 변수에 값이 넘어간다
* \(2\) Exception 처리도 가능. 메인 코드에서 `coro_avg.throw(SomewhatException)`으로 예외를 던져줄 수도 있다
* \(3\) 코루틴이 끝날때 반드시 처리되어야할 구문이 있을 경우, try-finally 구문을 이용해서 처리할 수 있다
* \(4\) `next(coroutine)`문을 실행하여야만 처음 yield문까지 진행하여 코루틴이 값을 받을 수 있는 상태가 된다. 이를 코루틴을 기동한다고 한다 \(기동하기 전에 send를 하면 `TypeError: can't send non-None value to a just-started generator` 에러가 난다\)
  * 이를 쉽고 직관적으로 사용하기 위해 next만 해주는 `@coroutine` 데코레이터를 만들어서 쓰는 경우가 많음
* \(5\) `.close()` 문으로 코루틴 종료 가능

### Returning Value

* Python 3.3 이상부터는 generator 함수에서 값을 넘겨주는 것이 가능
* Generator의 마지막에 `return ret`을 할 경우, Generator에서 발행하는 `StopIteration` Exception에 값이 담겨서 호출자로 넘겨보낼 수 있다
  * `except StopIteration as exc: result = exc.value`

## yield from

* PEP 380에 추가된 syntax로서, 2단계 이상의 깊은 generator \(coroutine\)을 관리할 때 사용할 수 있는 syntax
* 매우 간단하게 생각하면, 하위 제네레이터에서 yield하는 값을 쉽게 for문으로 yield한다고 생각할 수 있음
* 1단계 제네레이터를 메인 제네레이터, 2단계 제네레이터를 하위 제네레이터, 그리고 메인 제네레이터의 사용자를 호출자라고 할때 메인 제네레이터에서 `result = yield from <EXPR>` 같은 방식으로 `yield from` 문을 사용할 수 있음. 이렇게 될 경우
  * 메인 제네레이터가 `yield from`문에 걸려있을 경우, 호출자가 `send` 를 통해 보내는 값이나, 하위 제네레이터가 `yield`로 보내는 값은 서로에게 바로 전달됨
  * 하위 제네레이터가 `return` 할 경우, 그 값은 `StopIteration` exception을 통해 메인 제네레이터에게 전달됨. 이 exception / value handling을 `yield from`문이 해줘서, 하위 제네레이터가 return으로 끝날경우 result 값에 그 결과값이 바인딩됨
  * 호출자에서의 `throw()`나 `close()`도 하위 제네레이터에 전달됨. 이로 인해 Error가 발생할 경우 Error Propagation이 되거나, 제대로 종료될 경우 GeneratorExit exception이 발생하여 상위 제네레이터로 넘어감
* yield from의 경우, 하위 제네레이터가 기동되지 않았다고 생각하고 자동으로 기동해주므로 주의





