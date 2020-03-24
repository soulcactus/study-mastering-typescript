# 인터페이스

TypeScript의 핵심 원리 중 하나는 값이 가지는 *형태*에 초점을 맞추는 타입 체킹을 한다는 점입니다.
이는 앞서 언급한 **덕 타이핑(duck typing)** 또는 **구조적 하위 유형화(structural subtyping)**라고 합니다.
TypeScript에서는 인터페이스가 이러한 타입의 이름을 지정하는 역할을 하며 코드 내에서 계약을 정의하고 프로젝트 외부에서 코드를 사용하는 계약을 정의하는 강력한 방법입니다.

## 📲 인터페이스(Interface)

인터페이스의 작동 방식을 확인하는 가장 쉬운 방법은 간단한 예를 들어 시작하는 것입니다.

```typescript
function printLabel(labelledObj: { label: string }) {
    console.log(labelledObj.label);
}

let myObj = { size: 10, label: 'size 10 object' };

printLabel(myObj);
```

타입 체커는 `printLabel`에 대한 호출을 확인합니다.
함수 printLabel에는 객체를 전달하는 데 필요한 단일 매개변수가 있으며, 이는 문자열 타입의 `label` 프로퍼티를 가집니다.
실제로 객체는 이보다 더 많은 프로퍼티를 가지고 있지만 컴파일러는 필요한 속성을 _최소한으로_ 가지고 있고 필요한 타입과 일치하는지만 검사합니다.
**오리처럼 생겼고, 오리처럼 꽥꽥댄다면 오리로 보는 것입니다.(덕 타이핑!)**
물론 TypeScript가 그렇게 관대하지 않은 경우도 있습니다.
이는 뒷 부분에서 좀더 자세히 다룰 예정입니다.

이번에는 `interface` 키워드를 사용하여 문자열 타입인 `label` 프로퍼티를 가져야 한다는 요구 사항을 설명하는 동일한 예제를 다시 작성할 수 있습니다.

```typescript
interface LabelledValue {
    label: string;
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}

let myObj = { size: 10, label: 'size 10 object' };

printLabel(myObj);
```

인터페이스 `LabelledValue`는 이전 예제의 요구 사항을 설명하는 데 사용할 수 있는 이름입니다.
여전히 `label`이라는 문자열 타입의 단일 프로퍼티가 존재합니다.
중요한 것은 형태입니다.
함수로 전달되는 객체가 나열된 요구 사항을 충적하는 경우 허용됩니다.
타입 체커는 이러한 프로퍼티가 순서대로 제공될 것을 요구하지 않습니다.

####

## ☛ 선택적 프로퍼티(Optional properties)

인터페이스의 모든 프로퍼티가 필수로 필요할 수는 없습니다.
어떤 것들은 특정한 조건 하에 존재하거나 아예 존재하지 않을 수도 ;있습니다.
이러한 선택적 프로퍼티는 프로퍼티 중에서 일부만 채워진 객체를 함수에 전달하는 *옵션 백(option bags)*과 같은 패턴을 생성할 때 많이 사용됩니다.
예제를 통해 살펴보겠습니다.

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = { color: 'white', area: 100 };

    if (config.color) {
        newSquare.color = config.color;
    }

    if (config.width) {
        newSquare.area = config.width * config.width;
    }

    return newSquare;
}

let mySquare = createSquare({ color: 'black' });
```

선택적 프로퍼티를 가진 인터페이스는 다른 인터페이스와 유사하게 작성되며 선언된 프로퍼티 이름 끝에 `?`로 표시합니다.
선택적 프로퍼티의 장점은 사용 가능한 프로퍼티를 설명하는 동시에 인터페이스에 포함되지 않은 프로퍼티의 사용을 방지할 수 있는 점입니다.

예를 들어 함수 createSquare 내에서 `color` 프로퍼티의 이름을 다음과 같이 잘못 입력하면 오류 메시지가 표시됩니다.

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = { color: 'white', area: 100 };

    if (config.color) {
        newSquare.color = config.clor; // error
    }

    if (config.width) {
        newSquare.area = config.width * config.width;
    }

    return newSquare;
}

let mySquare = createSquare({ color: 'black' });
```

####

## 📕 읽기 전용 프로퍼티(Readonly properties)

일부 프로퍼티는 객체를 처음 생성할 때만 수정할 수 있어야 합니다.
프로퍼티 이름 앞에 `readonly` 키워드를 붙여 지정할 수 있습니다.

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

객체 리터럴을 할당해 `Point`를 구성할 수 있습니다. 할당 후 x와 y 값을 바꿀 수 없습니다.

```typescript
let p1: Point = { x: 10, y: 20 };

p1.x = 5; // error
```

TypeScript에는 모든 변형 메서드가 제거된 `Array<T>`와 동일한 `ReadonlyArray<T>` 타입이 존재하므로 생성 후 배열을 변경하지 않도록 합니다.

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;

ro[0] = 12; // error
ro.push(5); // error;
ro.length = 100; // error;
a = ro; // error
```

마지막 줄에서 전체 `ReadonlyArray`를 일반적인 배열로 다시 할당하는 것조차 허용되지 않는 것을 확인할 수 있습니다.
하지만 타입 단언을 통해 오버라이드 할 수 있습니다.

```typescript
a = ro as number[];
```

####

## 🔥 `readonly` vs `const`

`readonly`를 사용할지 아니면
