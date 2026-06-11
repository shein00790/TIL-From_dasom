### Context
* n8n 워크플로우를 설계할 때 여러 이전 단계(Nodes)에서 생성된 개별 숫자 데이터를 취합하여 합계를 구해야 하는 상황이 빈번하게 발생함.
* 사용자는 'Edit Fields(기존 Set)' 노드를 변수 관리 도구로 활용하여, 자바스크립트 표현식을 통해 동적으로 데이터를 연산하고 저장하는 방법을 실습함.

### Core
* n8n 표현식(Expressions)을 사용한 데이터 합산 및 노드 참조 방식:
```javascript
// 1. 특정 이전 노드의 필드값을 직접 참조하여 합산
{{ $node["Calculate Node"].json.subtotal + $node["Tax Node"].json.amount }}

// 2. 데이터 타입 오류 방지를 위한 명시적 숫자 형변환 (Type Casting)
{{ Number($json.value1) + Number($json.value2) }}

// 3. 다수의 아이템(Items)을 합산할 경우 (Summarize 노드 활용 권장)
// - Operation: Sum
// - Field to Summarize: 합산할 필드명
```
* 'Edit Fields' 노드는 단일 아이템 내의 필드 연산에 적합하며, 여러 개의 출력 아이템 전체를 합산해야 할 때는 'Summarize' 노드를 사용하는 것이 설계상 더 유리함.

### Insight
* n8n은 자바스크립트 기반의 표현식 엔진을 사용하므로 데이터 타입(Data Type) 처리가 결과의 정확도를 결정함.
* 입력값이 문자열일 경우 산술 연산 대신 문자열 결합(Concatenation)이 발생할 수 있으므로, 연산 전 `Number()` 함수를 사용하여 숫자형임을 보장하는 것이 기술적 모범 사례임.
* 변수 노드를 워크플로우 중간 단계에 배치하면 복잡한 연산 과정을 단계별로 시각화할 수 있어, 오류 발생 시 디버깅 속도를 획기적으로 높일 수 있음.

**출처:** [n8n Expressions Documentation](https://docs.n8n.io/code/expressions/), [n8n Edit Fields Node Guide](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.set/)