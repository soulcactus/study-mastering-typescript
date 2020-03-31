# 인터페이스

TypeScript의 핵심 원리 중 하나는 값이 가지는 *형태*에 초점을 맞추는 타입 체킹을 한다는 점입니다.
이는 앞서 언급한 **덕 타이핑(duck typing)** 또는 구조적 하위 유형화(structural subtyping)라고 합니다.
TypeScript에서는 인터페이스가 이러한 타입의 이름을 지정하는 역할을 하며 코드 내에서 계약을 정의하고 프로젝트 외부에서 코드를 사용하는 계약을 정의하는 강력한 방법입니다.

인터페이스는 객체가 구현해야 하는 속성과 메서드를 정의해 사용자 타입을 만드는 방법입니다.
강타입 변수를 사용해 인터페이스 변수가 같은 타입의 속성을 갖게 할 수 있습니다.

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

가장 간단한 형태의 인터페이스를 다시 살펴보겠습니다.

```typescript
interface IComplexType {
    id: number;
    name: string;
}
```

id와 name 속성을 같은 IComplexType 인터페이스를 정의했습니다.
id 속성은 숫자, name 속성은 문자열 타입입니다.
인터페이스 정의는 변수에 적용할 수 있습니다.

```typescript
let complexType: IComplexType;

complexType = { id: 1, name: 'test' };
```

complexType 변수를 IComplexType 타입으로 정의했습니다.
객체 인스턴스를 만들면서 객체의 속성값을 지정했습니다.
IComplexType 인터페이스에서 id와 name 속성을 정의한다는 점에 주목해야 합니다.
위의 예제에서 인터페이스 속성값은 반드시 있어야 합니다.
모든 속성을 넣지 않은 채 객체를 만들면 어떻게 되는지 예제를 통해 살펴보겠습니다.

```typescript
let incompleteType: IComplexType;

incompleteType = { id: 1 }; // error
```

인터페이스는 컴파일 시점 언어 기능입니다.
TypeScript 컴파일러는 프로젝트에 포함된 인터페이스로는 자바스크립트 코드를 생성하지 않습니다.
인터페이스는 컴파일 단계에서 타입 확인을 위해 컴파일러에서만 사용합니다.

####

## ☛ 선택적 프로퍼티(Optional properties)

함수의 선택적 인자처럼 인터페이스 정의도 선택적 속성을 가질 수 있습니다.
예제를 통해 살펴보겠습니다.

```typescript
interface IOptionalProp {
    id: number;
    name?: string;
}
```

숫자 타입의 id와 문자열 타입의 선택적 속성 name을 갖는 IOptionalProp 인터페이스를 정의했습니다.
선택적 속성 구문이 함수의 선택적 인자 구문과 비슷하다는 점에 주목해야 합닏가.
name 속성 뒤에 오는 `?` 기호로 선택적 속성임을 표시했습니다.
정의한 인터페이스의 사용 예제를 살펴보겠습니다.

```typescript
let idOnly: IOptionalProp = { id: 1 };
let idAndName: IOptionalProp = { id: 2, name: 'idAndName' };

idAndName = inOnly;
```

IOptionalProp 인터페이스를 구현하는 두 개의 변수를 선언했습니다.
첫번째인 idOnly는 id 속성만 갖고 있습니다.
IOptionalProp 인터페이스의 name 속성은 선택적이기 때문에 올바른 구문입니다.
두번째인 idAndName은 id와 name 속성을 모두 정의했습니다.

예제의 마지막 줄에 주목해야 합니다.
두 변수 모두 IOptionalProp 인터페이스를 구현하고 있으므로 서로간의 할당이 가능합니다.
인터페이스 정의에서 선택적 속성을 사용하지 않는다면 예제와 같은 코드는 보통 오류를 발생시킵니다.

타입스크립트를 지원하는 편집기를 사용하면 인터페이스를 사용할 때 자동완성이나 인텔레센스 옵션이 동작하는 것을 확인 할 수 있습니다.
편집기가 타입스트립트 언어 서비스를 사용해 작업 중인 타입을 자동으로 찾아 사용할 수 있는 속성이나 함수를 골라줍니다.

인터페이스의 모든 프로퍼티가 필수로 필요할 수는 없습니다.
어떤 것들은 특정한 조건 하에 존재하거나 아예 존재하지 않을 수도 있습니다.
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
함수 표현식이 숫자나 문자열을 반환하는 경우 타입 체커가 반환 타입이 `SearchFunc` 인터페이스에 설명된 반환 타입과 일치하지 않는다는 경고했을 것입니다.

```typescript
let mySearch: SearchFunc;

mySearch = function(src, sub) {
    let result = src.search(sub);

    return result > -1;
};
```

클래스와 같은 인터페이스는 함수를 다룰 때 동일한 규칙을 따릅니다.

```typescript
interface IComplexType {
    id: number;
    name: string;
    print(): string;
    usingTheAnyKeyword(arg1: any): any;
    usingOptionalParameters(optionalArg1?: number);
    usingDefaultParameters(defaultArg1?: number);
    usingRestSyntax(...argArray: number[]);
    usingFunctionCallbacks(callback: (id: number) => string);
}
```

클래스 함수처럼 생겼지만 함수 본문이 없다는 점에 주목해야 합니다.
usingOptionalParameters 함수 정의는 인터페이스 안에서 선택적 인자 사용법을 보여줍니다.
인터페이스 usingDefaultParameters 함수 정의는 클래스 안의 함수 정의와는 약간 다릅니다.
인터페이스는 클래스나 객체의 모양만을 정의하기 때문에 변수나 값을 가질 수 없습니다.
인터페이스에서는 인자인 defaultArg1을 선택적 인자로 정의하고 기본값은 인터페이스를 구현하는 클래스 구현체에서 결정하도록 합니다.

usingFunctionCallbacks 함수 정의는 콜백 함수 시그니처를 정의하는 방법을 보여줍니다.
인터페이스 함수는 클래스 함수와 상당히 유사합니다.
인터페이스 정의에 딱 하나 빠져 있는 것이 생성자 함수인데, 인터페이스는 생성자 함수를 포함할 수 없습니다.
생성자인 constructor 함수를 IComplexType 인터페이스 정의에 포함해 보도록 하겠습니다.

```typescript
interface IComplexType {
    constructor(arg1: any, arg2: any);
}
```

TypeScript는 컴파일 오류를 발생시킵니다.
오류는 constructor 함수를 사용할 때, 타입스크립트 컴파일러가 생성자의 반환타입을 암묵적으로 정의해 놓았음을 알립니다.
IComplexType 생성자의 반환타입은 IComplexType이 돼야 합니다.
다른 클래스가 IComplexType 인터페이스를 구현하더라도 실제로는 서로 다른 두 개의 타입이 되는 것입니다.
반환하는 타입이 다르므로 constructor 함수 시그니처는 항상 호환되지 않습니다.

####

## 🎰 인덱싱 가능 타입(Indexable types)

함수 타입을 설명하기 위해 인터페이스를 사용하는 방법과 마찬가지로 `a[10]` 또는 `ageMap['daniel']`처럼 *인덱스*를 생성할 수 있는 타입을 만들 수 있습니다.
인덱싱 가능 타입에는 객체로 인덱싱하는 데 사용할 수 있는 타입과 인덱싱할 때 해당 반환 타입을 설명하는 **인덱스 시그니처**가 있습니다.
예제 코드를 통해 살펴보겠습니다.

```typescript
interface StringArray {
    [index: number]: string;
}

let myArray: StringArray;

myArray = ['Bob', 'Fred'];

let myStr: string = myArray[0];
```

인덱스 시그니처를 가진 인터페이스 StringArray가 있습니다.
이 인덱스 시그니처는 StringArray가 number로 인덱싱될 때 string을 반환하는 것을 표현합니다.

지원되는 인덱스 시그니처에는 문자열과 숫자 두 가지 타입이 있습니다.
두 가지 타입의 인덱서(indexer)를 모두 지원할 수 있지만, 숫자 인덱서에서 반환되는 타입은 문자열 인덱서에서 반환된 타입의 하위 타입이어야 합니다.
number로 인덱싱을 생성하는 시점에 JavaScript가 객체로 인덱싱하기 전에 string으로 변환하기 때문입니다.

```typescript
class Animal {
    name: string;
}

class Dog extends Animal {
    breed: string;
}

// error
interface NotOkay {
    [x: number]: Animal;
    [x: string]: Dog;
}
```

문자열 인덱스 시그니처가 딕셔너리 패턴을 만드는 강력한 방법이지만 모든 프로퍼티가 반환 타입과 일치하도록 강요합니다.
문자열 인덱스의 `obj.property`가 `obj['property']`로도 사용할 수 있다고 선언하기 때문입니다.
다음 예시에서는 name의 타입이 문자열 인덱스의 타입과 일치하지 않으며 타입 체커에서 오류가 발생합니다.

```typescript
interface NumberDictionary {
    [index: string]: number;
    length: number; // good! length는 인덱서의 하위 타입입니다.
    name: string; // error! name은 인덱서의 하위 타입이 아닙니다.
}
```

인덱스에 할당되지 않도록 인덱스 시그니처를 읽기 전용으로 만들 수도 있습니다.

```typescript
interface ReadonlyStringArray {
    readonly [index: number]: string;
}

let myArray: ReadonlyStringArray = ['Alice', 'Bob'];

myArray[2] = 'Mallory'; // error
```

위의 예제의 인덱서 시그니처는 읽기 전용이므로 `myArray[2]`를 설정할 수 없습니다.

####

## 🏫 클래스 타입(Class types)

클래스와 인터페이스의 관계를 살펴보겠습니다.
클래스는 속성과 함수를 포함하는 객체를 정의합니다.
인터페이스는 속성과 함수를 포함하는 사용자 타입을 정의합니다.
실질적인 차이점은 클래스는 함수 본문을 구현해야 하고, 인터페이스는 함수 정의를 설명하기만 합니다.
이를 통해 인터페이스로 클래스 그룹의 동일한 동작을 설명하고 클래스로 동작하는 코드를 작성할 수 있습니다.
예제를 통해 살펴보겠습니다.

```typescript
class ClassA {
    print() {
        console.log('ClassA.print()');
    }
}

class classB {
    print() {
        console.log('ClassB.print()');
    }
}
```

print 함수만 갖고 있는 두 개의 클래스를 정의했습니다.
클래스 타입을 신경쓰지 않고 동작하는 코드를 작성하고 싶다면 print 함수를 가졌는지만 확인하면 됩니다.
두 클래스의 인스턴스를 모두 다루는 복잡한 클래스 대신 필요한 동작을 설명하는 인터페이스를 쉽게 만들 수 있습니다.
예제를 통해 살펴보겠습니다.

```typescript
interface IPrint {
    print();
}

function printClass(a: IPrint) {
    a.print();
}
```

printClass 함수에서 필요한 객체의 속성을 설명하는 IPrint 인터페이스를 만들었습니다.
IPrint 인터페이스는 print 함수를 갖고 있습니다.
어떤 변수든 printClass 함수에 인자로 전달하려면 print라는 함수가 있어야 합니다.
printClass 함수에서 두개의 클래스를 모두 사용할 수 있도록 클래스 정의를 수정할 수 있습니다.

```typescript
class ClassA implements IPrint {
    print() {
        console.log('ClassA.print()');
    }
}

class ClassB implements IPrint {
    print() {
        console.log('ClassB.print()');
    }
}
```

클래스 정의에서 implements 키워드로 IPrint 인터페이스를 구현합니다.
인터페이스 구현으로 printClass 함수에서 두 클래스 모두 사용할 수 있습니다.
예제를 통해 살펴보겠습니다.

```typescript
let classA = new ClassA();
let classB = new ClassB();

printClass(classA);
printClass(classB);
```

ClassA와 ClassB 인스턴스를 만들어 같은 printClass 함수를 호출합니다.
printClass 함수는 IPrint 인터페이스를 구현하는 모든 객체를 인자로 받게돼 있어 두 클래스 타입에 정상적으로 동작합니다.

인터페이스는 클래스의 동작을 설명하는 방법입니다.
인터페이스는 클래스의 특정 동작에 대해 반드시 구현해야 하는 요구조건이 되기도 합니다.

C# 및 Java와 같은 언어로 인터페이스를 사용하는 가장 일반적인 방법 중 하나로 클래스가 특정 계약을 충족하도록 명시적인 강제가 TypeScript에서도 가능합니다.

```typescript
interface ClockInterface {
    currentTime: Date;
}

class Clock implements ClockInterface {
    currentTime: Date;
    constructor(h: number, m: number) {}
}
```

또한 `setTime`과 마찬가지로 클래스에 구현된 인터페이스의 메서드를 만들 수도 있습니다.

```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) {}
}
```

인터페이스는 public 측면의 class를 만듭니다.
클래스를 사용하여 클래스 인스턴스의 private 측에 특정 타입이 있는지 검사하는 것은 금지돼 있습니다.

클래스와 인터페이스로 작업할 때 클래스에 _두 가지_ 타입이 있음을 명심해야 합니다.
스태틱 측면의 타입과 인스턴스 측만의 타입 constructor signature로 인터페이스를 만들고 이 인터페이스를 구하면하는 클래스를 생성하려고 하면 오류가 발생할 수 있습니다.

```typescript
interface ClockConstructor {
    new (hour: number, minute: number);
}

class Clock implements ClockConstructor {
    currentTime: Date;
    constructor(h: number, m: number) {}
}
```

클래스가 인터페이스를 구현할 때 클래스의 인스턴스 측면만 검사하기 때문입니다.
생성자는 정적인 측면이기 때문에 이 검사에 포함되지 않습니다.

대신 클래스의 정적인 측면에서 직접 작업해야 합니다.
이 예제에서는 생성자를 위한 `ClockConstructor`와 인스턴스 메서드를 위한 `ClockInterface`라는 두개의 인터페이스를 정의합니다.
편의상 전달된 타입의 인스턴스를 생성하는 `createClock` 생성자 함수를 정의합니다.

```typescript
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}

interface ClockInterface {
    tick();
}

function createClock(
    ctor: ClockConstructor,
    hour: number,
    minute: number,
): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) {}
    tick() {
        console.log('beep beep');
    }
}

class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) {}
    tick() {
        console.log('tick tock');
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

createClock의 첫 번째 매개 변수는 createClock(AnalogClock, 7, 32)에 ClockConstructor 타입이므로 AnalogClock이 올바른 생성자 시그니처(constructor signature)를 가지고 있는지 확인합니다.

####

## ⛲ 인터페이스 확장(Extending interfaces)

클래스처럼 인터페이스도 서로 확장할 수 있습니다.
이렇게 하면 한 인터페이스의 멤버를 다른 인터페이스로 복사할 수 있으므로 인터페이스를 재사용 가능한 컴포넌트로 분리하는 방법을 더 유연하게 할 수 있습니다.

```typescript
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square = <Square>{};

square.color = 'blue';
square.sideLength = 10;
```

여러 인터페이스를 확장하여 모든 인터페이스를 결합하여 만들 수 있습니다.

```typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};

square.color = 'blue';
square.sideLength = 10;
square.penWidth = 5.0;
```

####

## 🏍 하이브리드 타입(Hybrid types)

이전에 언급했듯 인터페이스는 실제 JavaScript에서 제공되는 풍부한 타입을 만들 수 있습니다.
JavaScript의 동적이고 유연한 특성으로 인해 위에 설명된 몇 가지 타입의 조합으로 작동하는 객체를 종종 볼 수 있습니다.

이러한 예로는 다음과 같이 추가 프로퍼티로 함수와 객체 역할을 모두 하는 객체가 있습니다.

```typescript
interface Counter {
    (start: number): string;
    interval: nuber;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function(start: number) {};

    counter.interval = 123;
    counter.reset = function() {};

    return counter;
}

let c = getCounter();

c(10);
c.reset();
c.interval = 5.0;
```

써드파티 JavaScript와 상호작용할 때 타입의 형태를 완전히 형성하려면 위와 같은 패턴을 사용해야 할 수 있습니다.

####

## 🌟 인터페이스 확장 클래스(Interfaces extending classes)

먼저 인터페이스를 상속하는 예제를 살펴보겠습니다.

```typescript
interface IBase {
    id: number;
}

interface IDerivedFromBase extends IBase {
    name: string;
}

class InterfaceInheritClass implements IDerivedFromBase {
    id: number;
    name: string;
}
```

숫자로 된 id 속성을 갖는 IBase 인터페이스를 만들었습니다.
그리고 IBase를 상속하는 IDerivedFromBase 인터페이스를 정의했습니다.
id 속성은 자동으로 포함됩니다.
IDerivedFromBase 인터페이스에는 문자열 타입의 name 속성을 추가했습니다.
IDerivedFromBase는 IBase 인터페이스를 상속하고 있으므로 자동으로 id, name 두 개의 속성을 갖습니다.

다음으로 IDerivedFromBase 인터페이스를 구현하는 InterfaceInheritClass 클래스를 생성했습니다.
InterfaceInheritClass 클래스는 id, name 속성을 모두 갖고 있어야 합니다.
예제에서는 속성만 언급했지만, 함수에도 같은 규칙이 적용됩니다.

인터페이스 타입이 클래스 타입을 확장하면 해당 클래스의 멤버들을 상속하지만 구현을 상속하지는 않습니다.
마치 인터페이스가 구현을 제공하지 않고 클래스의 모든 멤버를 선언한 것과 같습니다.
인터페이스는 기본 클래스의 private 및 protected 멤버까지 상속합니다.

즉, private 또는 protected 멤버가 있는 클래스를 확장하는 인터페이스를 생성하면 해당 인터페이스 타입은 해당 클래스 또는 해당 클래스의 서브 클래스에서만 구현할 수 있습니다.

상속 계층이 크지만 특정 프로퍼티를 가진 서브 클래스에서만 코드가 작동하도록 지정하려는 경우에 유용합니다.
서브 클래스는 기본 클래스에서 상속받는 것 외에는 관련이 없습니다.
예제 코드를 통해 살펴보겠습니다.

```typescript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control implements SelectableControl {
    select() {}
}

class TextBox extends Control {
    select() {}
}

// error
class Image implements SelectableControl {
    select() {}
}

class Lacation {}
```

위의 예제에서 `SelectableControl`에는 Private `state` 프로퍼티를 포함한 `Control`의 모든 멤버가 포함돼 있습니다.
`state`는 private 멤버이기 때문에 `Control`의 자식만 `SelectableControl`을 구현할 수 있습니다.
`Control`의 자식들만이 같은 선언에서 시작된 `state` private 멤버를 가지기 때문입니다.

클래스 `Control` 내에서 `SelectableControl`의 인스턴스를 통해 `state` private 멤버에 접근할 수 있습니다.
실제로 `SelectableControl`은 알려진 대로 `select` 메서드를 가진 `Control`과 같은 역할을 합니다.
`Button`과 `TextBox` 클래스는 `SelectableControl`의 하위 타입입니다.
`Image`와 `Location` 클래스는 그렇지 않습니다.
