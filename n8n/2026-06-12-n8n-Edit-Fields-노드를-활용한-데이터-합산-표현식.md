### Context
n8n 워크플로우를 설계하다 보면 이전 노드들에서 생성된 여러 개의 데이터 항목(Items) 중 특정 필드의 수치를 모두 더해야 하는 상황이 발생합니다. 기존의 'Set' 노드에서 명칭이 변경된 'Edit Fields' 노드를 활용하여, 자바스크립트 표현식을 통해 분산된 데이터의 총합을 구하는 실습을 진행했습니다.

### Core
n8n의 'Edit Fields' 노드에서 모든 입력 항목의 특정 필드 값을 합산하려면 다음과 같은 JavaScript 표현식을 사용합니다.

```javascript
// 'amount' 필드의 모든 값을 합산하는 표현식
{{ $input.all().reduce((sum, item) => sum + (Number(item.json.amount) || 0), 0) }}
```

*   **$input.all()**: 노드로 들어온 모든 아이템 객체를 배열 형태로 가져옵니다.
*   **reduce()**: 배열의 각 요소를 순회하며 하나의 결과값(합계)을 만듭니다.
*   **Number()**: 문자열로 인식될 수 있는 데이터를 숫자형으로 강제 변환하여 연산 오류를 방지합니다.
*   **|| 0**: 데이터가 비어있거나(null/undefined) 숫자가 아닐 경우 0을 기본값으로 사용합니다.

### Insight
n8n에서 데이터를 합산할 때는 'Aggregate' 노드를 사용할 수도 있지만, 복잡한 로직 없이 단순히 변수를 생성하고 결과값을 다음 노드로 넘길 때는 'Edit Fields' 노드 내에서 표현식을 사용하는 것이 워크플로우를 훨씬 간결하게 만듭니다.

*   **데이터 타입의 중요성**: API 결과값은 간혹 숫자가 문자열 형태로 들어오기 때문에, 합산 전 반드시 `Number()` 처리를 하는 습관이 필요합니다. 그렇지 않으면 `10 + 20`이 `1020`이라는 문자열 결합 결과로 나타날 수 있습니다.
*   **유연성**: `reduce` 함수 내에서 `if` 조건문을 추가하면 특정 조건을 만족하는 데이터만 선별하여 합산하는 등 더욱 고도화된 데이터 가공이 가능합니다.

**출처:** [n8n Expressions Documentation](https://docs.n8n.io/code/expressions/), [n8n Forum: Summing values from items](https://community.n8n.io/t/sum-values-from-the-expression-field/42111)