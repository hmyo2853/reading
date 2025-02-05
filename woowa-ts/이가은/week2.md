## 3장: 고급 타입

### 3.1 타입스크립트만의 독자적 타입 시스템

### 1. any 타입

> **any 타입은 온전히 개발자 스스로 책임을 져야함을 의미한다.**

- 타입스크립트의 독자적 타입 시스템 중 하나는 `any`이다. 자바스크립트의 `typeof`, `Object.prototype.toString.call(...)`로 잡아낼 수 없다.
- 모든 값을 오류 없이 받아낸다. 자바스크립트 기존 사용 방식과 같다.
- `tsconfig.sjon` 파일에서 `noImplicitAny` 옵션을 활성화하면 `any` 타입의 경고를 발생시킬 수 있다.
- 개발 단계에서 임시로 값을 지정할 때 임시로 사용 -> 하지만 후에 바꾸는 과정이 누락되면 문제가 발생될 수 있다. API 요청, 응답 처리 및 외부 라이브러리를 사용할때 사용

### 2. unknown 타입

> **어떤 타입이 할당되었음을 알 수 없다.**

- 할당하는 시점에는 에러가 없지만, 호출 시점에서 에러를 발생시킨다. (어떤 타입인지 알 수 없기 때문에)
- `unknown` 타입으로 선언된 변수는 값을 가져오거나 내부 속성에 접근할 수 없다.
- `unknown as Type` 같은 타입 캐스팅을 사용할 수 있으나 `any`와 다를 바 없어서 지양하는게 좋다.


### 3. void 타입

> **함수 타입이 어느 값도 반환하지 않는다.**

- 함수에서 반환하는 값이 없다면 컴파일러가 알아서 `void` 타입으로 추론한다.

### 4. never 타입

> **값을 반환할 수 없다.**

- 의도적으로 에러를 반환할 경우 `throw new Error(...)` 가 `never`타입이다.
- 드물게 반복문을 사용하여 무한 루프를 실행할 때, 함수가 종료되지 않기 때문에 반환되는 타입은 `never`이다.
- `never`는 최하위 타입이기 때문에 자신 외에는 `any`조차도 할당될 수 없다.

### 5. Array 타입

> **자바스크립트의 객체 타입이 아닌 타입스크립트의 특수한 타입 중 하나.**

- `Array<number>`와 `number[]`는 같은 `Array` 타입
- 타입으로 대괄호 `[]`를 사용하면 튜플을 가르킨다.
- 대괄호 `[]` 안에 선언하는 타입 개수가 튜플이 가질 수 있는 원소의 개수이다.
```js
let tuple:[number] = [1]; // 1개만 할당 가능
tuple = [1, 2] // X
```
- 튜플 사용의 예, `React`의 `useState`
- 옵셔널 프로퍼티(선택적 속성)를 적용하고 싶으면 물음표를 사용할 수 있다.
```js
const optional: [number, number, string?] = [1,2] // Ok
const optional: [number, number, string?] = [1,2, 'string'] // Ok
```

### 6. enum 타입

> **열거되는 구조체를 만드는 타입스크립트만의 특수한 타입 중 하나.**

- 기본 추론 방식은 0부터 1개씩 증가한다.
```js
enum SomeType {
  a, // 0
  b, // 1
  c, // 2
}

SomeType.a; // 0
SomeType[b]; // 1
SomeType[1]; // "b"
```
- 열거형은 관련이 높은 멤버들을 모아서 문자열 상수처럼 사용하고자 할 때 유용하게 쓸 수 있다.
- 하지만 할당된 값을 넘어서는 범위로 역방향으로 접근하더라도 막지 않는다. `e.g. SomeType[100]; // undefined 에러 x`
- 열거형은 타입 공간과 값 공간에서 모두 사용된다. 즉시 실행 함수로 변환될 때 일부 번들러에서 트리쉐이킹 과정 중 사용하지 않는 코드로 인식될 수 있다.


---



### 3.2 타입 조합

### 1. 교차 타입

> **기존에 존재하는 다른 타입들을 합쳐서 해당 타입의 모든 멤버를 가지는 새로운 타입으로 생성.**

- `A & B` 형식으로 선언한다.


### 2. 유니온 타입

> **기존에 존대하는 다른 타입들 중에 하나가 될 수 있다.**

- `A | B` 형식으로 선언하며, 2개 이상의 타입을 이어 붙여 중복을 줄일 수 있다.

### 3. 인덱스 시그니처

> **특정 타입의 속성 이름은 알 수 없지만, 속성값의 타입을 알고있을때 사용.**

- 객체의 `Key`는 `K` 타입으로, `value`는 `T` 타입으로 선언할 경우, `[Key: K]: T`로 명시해서 사용한다. `e.g. [key: string]: number;`

### 4. 인덱스드 엑세스 타입

> **다른 타입의 특정 속성이 가지는 타입을 조회하기 위해 사용.**

```js
type Example = {
  a: number;
  b: string;
};

type IndexedAccess1 = Example["a"]; // number 만 가능
type IndexedAccess2 = Example["b"]; // string 만 가능
```

### 5. 맵드 타입

> **유사한 형태를 가진 여러 항목의 목록 A를 변환된 항목의 B로 바꾸는 것.**

- 자바스크립트의 `map` 성질과 비슷하다.
- 다른 타입을 기반으로 한 타입을 선언할 때 사용, 반복적인 타입 선언을 효과적으로 줄일 수 있다.
```js
type Example = {
  a: number;
  b: string;
}

type Subset<T> = {
  [K in keyof T]?: T[K];
}

const aExample: Subset<Example> = {a: 3};
```
- `?`는 옵셔널 파라미터로 사용된다. (있을 수도 있고, 없을 수도 있다.)
- `-` 로 타입을 제거 할 수 있다???? ???????

### 6. 템플릿 리터럴 타입

> **타입에 변수처럼 리터럴 형태로 사용.**

- 자바스크립트의 템플릿 리터럴 문자열을 사용한 특정 타입.
```js
type Stage =
| 'init'
| 'select-image'
| 'edit-image'
| 'decorate-card';
type StageName = `${Stage}-stage` // init-stage, select-image-stage, ... 와 같은 타입으로 사용
```

### 7. 제네릭

> **특징이 없거나, 일반적인 것.**

- 함수, 타입, 클래스 등에서 내부적으로 미리 정해두지 않고, 타입 변수를 사용하여 해당 자리를 비워 둔 후에 호출시점에 타입을 지정하여 사용하는 방식.
- `extends`를 사용하여 특정 하위타입만 오도록 허용


### 3.3 제네릭 사용법

### 1. 함수의 제네릭

```js
function 함수명<T>(arg: ObjectType<T> | EntityScheme<T> | string): Repository<T> {
  return ~~~
}
```

### 2. 호출 시그니처의 제네릭

구체 타입 ????

```js
export type 타입명 = <RequestData = void, ResponseData = void>(url: string)
=> [RequestStatus, Request<RequestData, ResponseData>];
```

### 3. 제네릭 클래스

```js
class LocalDB<T>{
  async put(table: string, row:T): Promise<T> {
    return new Promise<T>((resolved, rejected) => { ... });
  }
  ...
}
```

### 4. 제한된 제네릭

```js
function 함수명<T extends 타입명>({
  a,
  b,
  c
}: 특정타입<T>): {
  ...
}{ ... }
```


### 5. 확장된 제네릭

```js
<Key extends string> // 제네릭의 유연성을 해친다. 사용X

export class APIResponse<Ok, Err = string>{
  ...
}
```

### 6. 제네릭 예시

```js
// API 응답값을 지정할 때 사용
export interface ApiResponse<Data> {
  data: Data;
  statusCode: string;
  statusMessage?: string;
};

export const fetchData = (): Promise<ApiResponse<PriceInfo>> => {
  ...
  return request({
    method: 'GET',
    url: PriceUrl
  })
}

export const fetchData = (): Promise<ApiResponse<OrderInfo>> => {
  ...
  return request({
    method: 'GET',
    url: OrderUrl
  })
}
```


---

## 질문

```js
Q1: any와 unknown의 차이점은 뭔가요?

Q2: enum의 단점을 한가지이상 알려주세요.

Q3: 반환값을 never로 선언할 수 있는 경우 2가지가 있습니다. 어느 경우일까요?
```

---

## 정답

```js
A1: any는 어떤 타입이든 선언 가능하고, 어떤 타입이든 반환 가능하다. unknown은 어떤 타입이든 선언 가능하지만, 아무 타입도 반환하지 않는다.

type UnknownType = {
  data: unknown
};
data.length; // error

A2: enum은 할당된 값을 넘어서 역방향으로 접근해도 에러가 나지 않는다. undefined를 반환.
A3: throw new Error의 경우와 무한 루프 함수의 경우 반환 값이 never.
```
