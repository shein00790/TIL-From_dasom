### Context
파이썬에서 리스트 컴프리헨션(List Comprehension)은 데이터 필터링과 변환을 한 줄로 처리할 수 있는 강력한 도구입니다. 하지만 필터링 조건이 복잡해질수록 코드의 가로 길이가 길어지고 가독성이 급격히 떨어지는 문제가 발생합니다. 이에 대한 대안으로 `filter()` 함수를 고려할 수 있으나, 처리 속도와 파이썬의 설계 철학 관점에서 두 방식의 명확한 차이점을 이해하고 상황에 맞는 가이드를 제시하고자 합니다.

### Core
```python
# 데이터 준비
data = range(1, 1000000)

# 1. 다중 조건을 포함한 리스트 컴프리헨션 (성능 우위)
# 모든 조건을 한 줄에 기술하여 함수 호출 오버헤드 최소화
result_comp = [x for x in data if x % 2 == 0 if x % 3 == 0 if x % 5 == 0]

# 2. filter()와 람다(Lambda) 사용 (가독성 고려 시작)
# 각 요소마다 lambda 함수를 호출하므로 대량 데이터에서 성능 저하 발생
result_filter = list(filter(lambda x: x % 2 == 0 and x % 3 == 0 and x % 5 == 0, data))

# 3. 명명된 함수와 filter() 조합 (가독성 극대화)
# 로직을 별도 함수로 분리하여 비전공자도 이해하기 쉬운 구조 제공
def is_target(x):
    return x % 2 == 0 and x % 3 == 0 and x % 5 == 0

result_named_filter = list(filter(is_target, data))
```

### Insight
* 성능 차이의 원인: 리스트 컴프리헨션은 파이썬 인터프리터 내부의 C 루프에서 직접 실행되는 반면, `filter(lambda ...)`는 매 요소마다 파이썬 레벨의 함수 호출(Function Call Overhead)을 발생시킵니다. 이로 인해 대량 데이터 처리 시 컴프리헨션이 통상적으로 더 빠릅니다.
* 가독성 가이드라인: 복잡한 다중 `if` 문이 들어가는 컴프리헨션은 유지보수가 어렵습니다. 따라서 협업이 중요하거나 비전공자가 포함된 팀에서는 성능 손실을 감수하더라도 명확한 이름을 가진 함수와 `filter()`를 조합하는 것을 권장합니다.
* 파이썬의 지향점: 파이썬 공식 문서와 커뮤니티에서는 일반적으로 리스트 컴프리헨션을 더 선호(Pythonic)하지만, `filter()`는 제너레이터(Generator) 특성을 활용하여 메모리 효율성을 높여야 하는 특정 상황에서 여전히 유효한 선택지입니다.

**출처:** [Python Speed - List Comprehensions vs Filter](https://wiki.python.org/moin/PythonSpeed/PerformanceTips#Loops)
**출처:** [Real Python - Python's filter() Function: Filtering Iterables](https://realpython.com/python-filter-function/)
**출처:** [Stack Overflow - List comprehension vs filter performance](https://stackoverflow.com/questions/3013449/list-comprehension-vs-lambda-filter)