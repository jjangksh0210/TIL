
### Context
파이썬에서 리스트를 생성하거나 변형할 때, 여러 조건을 적용해야 하는 경우가 흔히 발생합니다. 이때 `리스트 컴프리헨션(List Comprehension)` 내에 조건문을 여러 개 사용하는 방식(`[x for x in list if a if b]`)과 내장 함수인 `filter()`를 사용하는 방식 중 어느 것이 코드의 가독성과 성능 측면에서 더 효율적인지 비교 분석할 필요가 있습니다. 특히 조건이 많아질수록 가독성이 저하될 수 있으며, 대량의 데이터를 처리할 때는 성능 차이가 중요한 고려 사항이 됩니다.

### Core

다음은 리스트 컴프리헨션과 `filter()`를 사용하여 여러 조건으로 리스트를 필터링하는 예제 코드와 각 방식의 성능을 비교하는 코드입니다.

```python
import timeit

# 데이터 준비
data = list(range(1, 1000001)) # 1부터 100만까지의 숫자 리스트

# 조건: 2의 배수이면서 3의 배수인 숫자 필터링
# 조건 1: x % 2 == 0
# 조건 2: x % 3 == 0

# 1. 리스트 컴프리헨션 (다중 if 문)
comprehension_code = '''
result_comprehension = [x for x in data if x % 2 == 0 if x % 3 == 0]
'''
time_comprehension = timeit.timeit(comprehension_code, globals=globals(), number=100)
print(f"리스트 컴프리헨션 실행 시간: {time_comprehension:.6f} 초")

# 2. filter() 함수 (람다 사용)
filter_code = '''
result_filter = list(filter(lambda x: x % 2 == 0 and x % 3 == 0, data))
'''
time_filter = timeit.timeit(filter_code, globals=globals(), number=100)
print(f"filter() 함수 실행 시간: {time_filter:.6f} 초")

# 3. 리스트 컴프리헨션 (단일 if 문에 논리 연산자 사용)
# 가독성과 성능 측면에서 더 흔히 사용되는 방식
comprehension_and_code = '''
result_comprehension_and = [x for x in data if x % 2 == 0 and x % 3 == 0]
'''
time_comprehension_and = timeit.timeit(comprehension_and_code, globals=globals(), number=100)
print(f"리스트 컴프리헨션 (and 연산자) 실행 시간: {time_comprehension_and:.6f} 초")
```

### Insight
위 성능 테스트 결과에서 알 수 있듯이, 대량의 데이터를 처리할 때는 일반적으로 `리스트 컴프리헨션(List Comprehension)`이 `filter()` 함수보다 더 빠른 성능을 보입니다. 이는 `filter()`가 내부적으로 함수 호출 오버헤드를 발생시키기 때문입니다. 특히 파이썬의 C로 구현된 `리스트 컴프리헨션`은 최적화되어 있어 빠른 속도를 자랑합니다.

가독성 측면에서는 조건문이 적을 때는 `리스트 컴프리헨션`이 간결하고 직관적일 수 있습니다. 하지만 조건문의 수가 많아지거나 로직이 복잡해질 경우, `filter()` 함수에 별도의 명명된 함수를 전달하거나 `lambda` 표현식 내에 `and`와 같은 논리 연산자를 사용하는 것이 더 명확할 수 있습니다. 비전공자나 코드 리뷰어가 코드 로직을 빠르게 파악해야 하는 상황에서는 `filter()`와 명시적인 함수 조합이 가독성을 높일 수 있습니다.

결론적으로, 성능이 중요한 대규모 데이터 처리 작업에는 `리스트 컴프리헨션`을 사용하는 것이 유리하며, 특히 `if x % 2 == 0 and x % 3 == 0`와 같이 `and` 연산자를 사용하여 단일 `if` 문으로 조건을 표현하는 것이 가독성과 성능 모두에서 좋은 선택입니다. 반면, 코드의 가독성이 최우선이며 성능 요구사항이 낮을 경우에는 `filter()`를 고려할 수 있습니다.

**출처:**
*   [Python List Comprehension Vs. Map Vs. Filter - GeeksforGeeks](https://www.geeksforgeeks.org/python-list-comprehension-vs-map-vs-filter/)
*   [List Comprehensions vs. filter in Python for speed - Stack Overflow](https://stackoverflow.com/questions/52118318/list-comprehensions-vs-filter-in-python-for-speed/)