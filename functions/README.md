# 함수

함수는 JavaScript의 모든 애플리케이션을 구성하는 기본 요소입니다.
클래스는 추상화 계층, 클래스, 정보 은닉 및 모듈을 구축하는 방법입니다.
TypeScript에서는 클래스와 네임 스페이스 그리고 모듈이 있지만 그럼에도 불구하고 함수는 작업 방법을 설명하는 데 중요한 역할을 합니다.
또한 TypeScript는 표준 JavaScript 기능에 몇가지 새로운 기능을 추가하여 작업을 더 쉽게 해 줍니다.

## ⚙ 함수(Functions)

JavaScript와 마찬가지로 TypeScript 함수는 기명 함수 또는 익명 함수로 만들 수 있습니다.
이를 통해 API의 함수 목록을 작성하든 다른 함수에 전달할 일회성 함수든 애플리케이션에 가장 적합한 접근 방법을 선택할 수 있습니다.

```typescript
// 기명 함수
function add(x, y) {
    return x + y;
}

// 익명 함수
let myAdd = function (x, y) {
    return x + y;
};
```

JavaScript에서와 마찬가지로 함수는 함수 본문 외부의 변수를 참조할 수 있습니다.
이러한 변수들을 `capture`라고 합니다.
이 기법의 사용방법과 사용할 때의 절충 사항을 이해하는 것은 이번 장의 범위를 벗어나지만
캡쳐의 메커니즘이 JavaScript와 TypeScript에 얼마나 중요한 부분인지 확실히 이해해야 합니다.

```typescript
let z = 100;

function addToZ(x, y) {
    return x + y + z;
}
```

####

## 📝 함수 타입(Function types)

### 함수 작성하기

앞서 살펴본 간단한 예제에 타입을 추가해 보겠습니다.

```typescript
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function (x: number, y: number): number {
    return x + y;
};
```

각 매개변수에 타입을 추가한 다음 함수 자체에 타입을 추가해 반환 타입을 추가할 수 있습니다.
TypeScript는 리턴문을 보고 반환 타입을 파악할 수 있기 때문에 대부분 선택적으로 반환 타입을 생략할 수 있습니다.

### 함수 타입 작성하기

```typescript
let myAdd: (x: number, y: number) => number = function (
    x: number,
    y: number,
): number {
    return x + y;
};
```

함수 타입은 인수 타입과 반환 타입 두 개의 파트로 나뉩니다.
전체 함수 타입을 작성할 때 두 파트 모두 필요합니다.
매개변수 타입과 같이 매개변수 목록을 기록해 각 매개변수에 이름과 타입을 지정합니다.
이 이름은 가독성을 돕기 위한 것입니다.

위의 코드를 다음과 같이 작성할 수 있습니다.

```typescript
let myAdd: (baseValue: number, increment: number) => number = function (
    x: number,
    y: number,
): number {
    return x + y;
};
```

매개변수 타입이 정렬돼 있는 한 함수의 타입에 매개변수를 제공하는 이름에 관계 없이 매개변수 타입이 유효한 타입으로 간주됩니다.
두 번째 파트는 반환 타입입니다.
매개변수와 반환 타입 사이에 화살표를 사용하여 반환 타입을 명확하게 합니다.
앞서 언급한 것처럼 이것은 함수 타입의 필수적인 부분이므로 함수가 값을 반환하지 않는 경우에는 반환 값을 남겨두지 않고 `void`를 사용합니다.

주의할 점은 매개변수와 반환 타입만 함수 타입을 구성한다는 점입니다.
캡쳐된 변수는 타입에 반영되지 않습니다.
실제로 캡쳐된 변수는 함수의 **숨겨진 상태**의 일부이며 해당 API를 구성하지 않습니다.

####

## ❗ 타입 추론(Inferring the types)

예를 들어 TypeScript 컴파일러는 한쪽에는 타입이 있지만 다른 한쪽에 타입이 없는 경우 그 타입을 이해할 수 없다는 것을 알게 됩니다.
이것을 타입 추론의 한 종류인 **상황적 타이핑**이라고 합니다. 이를 통해 프로그램을 계속 유지하는 데 드는 노력을 줄일 수 있습니다.

```typescript
// myAdd는 완벽하게 함수 타입을 가지고 있습니다.
let myAdd = function (x: number, y: number): number {
    return x + y;
};

// 매개변수 'x'와 'y'에는 number 타입이 있습니다.
let myAdd: (baseValue: number, increment: number) => number = function (x, y) {
    return x + y;
};
```

####

## 👉 선택적 매개변수와 기본 매개변수

TypeScript에서는 모든 매개변수가 함수에 필요하다고 가정합니다.
`null` 또는 `undefined`가 주어질 수 없다는 것을 의미하는 것이 아니라 함수가 호출될 때 컴파일러가 각 매개변수에 값을 제공했는지 확인한다는 것입니다.

또한 컴파일러는 이러한 매개변수들이 함수로 전달되는 유일한 매개변수라고 가정합니다.
간단히 말해서 함수에 주어진 인수의 수는 그 함수에서 기대하는 매개변수의 수와 일치해야 합니다.

```typescript
function buildName(firstName: string, lastName: string) {
    return `${firstName} ${lastName}`;
}

let result1 = buildName('Bob'); // error
let result2 = buildName('Bob', 'Adams', 'Sr.'); // error
let result3 = buildName('Bob', 'Adams'); // good
```

JavaScript에서 모든 매개변수는 선택 사항이며 매개변수를 원하는 대로 사용하지 않을 수 있습니다.
그렇게 되면 그 매개변수들의 값은 `undefined`입니다.
TypeScript에서 선택적인 매개변수를 사용하려면 선택적으로 사용하려는 매개변수 끝에 `?`를 추가하세요.

예를 들어 위에서 사용한 매개변수를 선택적으로 사용할 수 있도록 해 보겠습니다.

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return `${firstName} ${lastName}`;
    } else {
        return firstName;
    }
}

let result1 = buildName('Bob'); // good
let result2 = buildName('Bob', 'Adams', 'Sr.'); // error
let result3 = buildName('Bob', 'Adams'); // good
```
