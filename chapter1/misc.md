# Miscellaneous

### Atexit

* `atexit` 을 이용해서 프로그램이 종료될 때 반드시 호출되는 종료 핸들러를 만들 수 있음
* Usage

```py
import atexit
def exit_func(arg1, arg2):
    do_something()

...
atexit.register(exit_func, arg1, arg2)

==============================

# arg 필요 없을 경우
@atexit.register
def exit_func():
    do_some_cleaning()
```

* 주의: Atexit은 파이썬이 정상적으로 종료되는 상황에서만 실행됨. 즉 Internal Error가 났거나, 외부에서 signal을 보내서 죽은 경우 등에는 종료되지 않



