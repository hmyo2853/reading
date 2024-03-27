## 3장: 고급 타입

아래는 타입스크립트에만 있는 타입들이다.

1. any 타입
- 타입을 명시하지 않은 것과 동일한 효과
- 지양하지만, 어쩔 수 없이 사용해야할 3가지 경우가 있음
  - 개발 단계에서 임시로 값을 지정할 때
  - 어떤 값을 받아올지 or 넘겨줄지 정할 수 없을 때
  - 값을 예측할 수 없을때 암묵적으로 사용

2. unknown 타입
- any와 유사하게 모든 타입의 값이 할당될 수 있지만, any가 아닌 다른 타입으로 선언된 변수에는 unknown 타입 값을 할당할 수 없음
- unknown 타입 변수는 값을 가져오거나 내부 속성에 접근할 수 없음. 어떤 값이든 올수 있으면서도 개발자에게 엄격한 타입 검사를 강제하려는 의도
  - any 타입과 유사하지만 타입 검사를 강제하고 타입이 식별된 후에 사용할 수 있기에 any보다 안전함.
- 강제 타입 캐스팅으로 타입을 전환할 때 사용 (`const env = process.env as unknown as ProcessEnv`) 하지만 any와 다를 바 없다
- TS 4.4부터 try-catch 에러의 타입이 any에서 unknown으로 변경되어 에러 핸들링할 때도 사용 가능

3. void 타입
- 함수가 아무것도 반환하지 않는 경우

4. never 타입
- 모든 타입의 하위 타입 (= 어떤 타입도 never에 할당될 수 없음)
- 값을 반환할 수 없는 타입 (2가지)
  - 에러를 던지는 경우
  - 무한히 함수가 실행되는 경우

5. Array 타입

### 튜플

- 배열 타입의 하위 타입으로, 기존 TS의 배열 기능 + 길이 제한을 추가
- 원소 개수와 타입을 보장 가능

6. enum 타입
- 숫자 0부터 1씩 늘려가며 속성의 값을 수수로 추론 
- 관련이 높은 멤버들을 문자열 상수처럼 사용하고자 할 때 유용
- 단, 역방향으로 접근할 수 있고 할당된 값을 넘어서는 범위로 접근해도 TS는 막지 않음
  - const enum으로 열거형을 선언하면 위 접근을 막아 JS 객체와 유사한 동작 보장
  - 그러나 const enum으로 선언해도 숫자 상수로 관리되면 선언한 값 이외의 값을 할당, 접근하는 걸 막지 못함

### 타입 조합

1. Intersection (`ProductItem & {discount: number}`)
2. Union (`ProductItem | CardItem`)
3. Index Signature (`[key:string]: number`)
4. Indexed Access Types (`type ElementOf<T> = typeof T[number]`)
5. Mapped Types (`ProductItem | CardItem`)
6. 템플릿 리터럴 타입
7. 제네릭