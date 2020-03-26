# 인터페이스

TypeScript의 핵심 원리 중 하나는 값이 가지는 *형태*에 초점을 맞추는 타입 체킹을 한다는 점입니다.
이는 앞서 언급한 **덕 타이핑(duck typing)** 또는 구조적 하위 유형화(structural subtyping)라고 합니다.
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
이러한 선택적 프로퍼티는 프로퍼티 중에서 일부만 채워진 객체를 함수에 전달하는 옵션 백(option bags)과 같은 패턴을 생성할 때 많이 사용됩니다.
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
**선택적 프로퍼티의 장점은 사용 가능한 프로퍼티를 설명하는 동시에 인터페이스에 포함되지 않은 프로퍼티의 사용을 방지할 수 있는 점입니다.**

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

`readonly`를 사용할지 아니면 `const`를 사용할지 기억할 수 있는 가장 쉬운 방법은 변수에서 사용하지 혹은 프로퍼티에서 사용할지를 묻는 것입니다.
변수는 `const`를 사용하고 프로퍼티는 `readonly`를 사용합니다.

####

## 💦 프로퍼티 초과 검사(Excess property checks)

인터페이스를 사용하는 첫번째 예시에서 TypeScript를 사용하면 `{ size: number, label: string }`을 `{ label: string }`으로만 예상하는 항목으로 전달할 수 있습니다.
또한 선택적 프로퍼티에 대해 학습했고, 그것이 옵션 백(option bags)을 설명할 때 어떻게 유용한지 알아봤습니다.

그러나 두 가지를 결합하는 것은 JavaScript에서 하고 있는 것과 마찬가지로 무덤을 파는 일과 같습니다.
이전 예제를 활용한 예제를 통해 살펴보겠습니다.

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

let mySquare = createSquare({ colour: 'red', width: 100 });
```

함수 createSquare의 인수는 color가 아닌 colour입니다.
보통 JavaScript에서는 이러한 종류의 작업은 조용히 실패합니다.
width 프로퍼티가 호환되고 color 프로퍼티가 없으며 특별하게 colour 프로퍼티가 대수롭지 않기 때문에 이 프로그램이 올바른 타입임을 주장할 수 있습니다.

그러나 TypeScript에서는 이 코드에 버그가 있을 수 있음을 알립니다.
객체 리터럴은 다른 변수에 할당하거나 인수로 전달할 때 특별한 처리를 받아 프로퍼티 초과 검사(Excess Property Checks)를 거칩니다.
객체 리터럴에 **대상 타입**에 없는 프로퍼티가 존재할 경우 오류가 발생합니다.

컴파일 오류를 피할 수 있는 가장 쉬운 방법은 타입 단언(type assertion)을 사용하는 것입니다.

```typescript
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```

하지만 객체에 추가 프로퍼티가 존재하는 것이 확실한 경우 문자열 인덱스 시그니처(string index signature)을 추가하는 것이 더 좋습니다.

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

이후에 인덱스 시그니처(index signatures)에 대해 이야기하겠지만 SquareConfig은 여러 프로퍼티들을 가질 수 있으며 color 또는 width가 아닌 다른 프로퍼티들의 타입은 문제 되지 않습니다.

컴파일 오류를 피할 수 있는 마지막 방법 중 하나는 객체를 다른 변수에 할당하는 것입니다.
squareOptions는 프로퍼티 초과 검사를 거치지 않기 때문에 컴파일러가 오류를 제공하지 않습니다.

```typescript
let squareOptions = { colour: 'red', width: 100 };
let mySquare = createSquare(squareOptions);
```

하지만 위와 같이 간단한 코드의 경우에는 컴파일 검사를 _회피하는_ 시도를 하지 말아야 합니다.
대부분의 초과 프로퍼티 오류는 실제로 버그입니다.
즉 옵션 백(option bags)과 같은 물건에 대해 초과 프로퍼티 검사 문제가 발생하는 경우 타입 선언 중 일부를 수정해야 할 수도 있습니다.
함수 createSquare에 color 또는 colour 프로퍼티를 모두 포함한 객체를 전달하려고 의도하는 경우 squareConfig의 정의를 수정하는 것이 맞습니다.

####

## 🎁 함수 타입(Function types)

인터페이스는 JavaScript 객체가 취할 수 있는 다양한 형태를 형성할 수 있습니다.
프로퍼티를 가진 객체를 설명한느 것 외에도 인터페이스는 함수 타입을 형성할 수도 있습니다.

인터페이스가 포함된 함수의 타입을 형성하기 위해 인터페이스에 호출 시그니처(call signature)를 제공합니다.
이것은 매개변수 목록과 반환 타입만 주어진 함수 선언과 같습니다.
매개변수 목록의 각 매개변수에는 이름과 타입이 모두 필요합니다.

```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}
```

일단 정의되면 다른 인터페이스처럼 이 함수 타입의 인터페이스를 사용할 수 있습니다.
여기서는 함수 타입의 변수를 생성하고 동일한 타입의 함수 값을 할당하는 방법을 살펴보겠습니다.

```typescript
let mySearch: SearchFunc;

mySearch = function(source: string, subString: string) {
    let result = source.search(subString);

    return result > -1;
};
```

함수 타입의 타입을 검사할 때 매개 변수의 이름이 일치할 필요는 없습니다.
예를 들어 다음과 같은 예를 작성할 수 있습니다.

```typescript
let mySearch: SearchFunc;

mySearch = function(src: string, sub: string): boolean {
    let result = src.search(sub);

    return result > -1;
};
```

함수 매개변수는 하나씩 검사되며 각 해당 파라미터 위치의 타입을 서로 비교하며 검사합니다.
타입을 지정하지 않으려는 경우 함수값이 `SearchFunc` 타입의 변수에 직접 지정되므로 TypeScript의 컨텍스트 타입(contextual typing)에 따라 인수 타입을 추론할 수 있습니다.
또한 여기서 함수 표현식의 반환 타입은 반환되는 값에 의해서도 암시적으로 나타납니다.
함수 표현식이 숫자나 문자열을 반환하는 경우 타입-체커가 반환 타입이 SearchFunc 인터페이스에 설명된 반환 타입과 일치하지 않는다는 경고했을 것입니다.

```typescript
let mySearch: SearchFunc;

mySearch = function(src, sub) {
    let result = src.search(sub);

    return result > -1;
};
```
