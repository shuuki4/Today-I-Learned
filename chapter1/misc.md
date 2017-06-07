# Miscellaneous

### Atexit

* `atexit` 을 이용해서 프로그램이 종료될 때 반드시 호출되는 종료 핸들러를 만들 수 있음
* Usage

```py
import atexit
def exit_func(arg1, arg2):
    do_something()
    
...
atexit.register(exit_func, args=(arg1, arg2))

==============================

# arg 필요 없을 경우
@atexit.register
def exit_func():
    do_some_cleaning()
```



