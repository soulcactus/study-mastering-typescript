# 기본 타입

프로그램을 유용하게 사용하려면 숫자, 문자열, 구조, 불리언 값 등과 같은 몇 가지 간단한 데이터 단위로 작업할 수 있어야 합니다.
TypeScript에서는 사용자가 JavaScript에서 기대하는 것과 같은 타입을 지원하며 이를 지원하기 위해 편리한 열거형을 포함합니다.

## 🕵 Type

1. Boolean

    가장 기본적인 데이터 타입은 JavaScript와 TypeScriptrk `boolean` 값을 호출하는 단순한 true/false 값입니다.

    ```typescript
    let isDone: boolean = false;
    ```

2. Number

    JavaScript와 마찬가지로 TypeScript의 모든 숫자는 부동소수점 값입니다.
    이 부동소수점 숫자는 `number` 타입을 받습니다.
    TypeScript는 10진수 및 16진수와 함께 ECMAScript2015에 도입된 2진수 및 8진수 문자를 지원합니다.

    ```typescript
    let decimal: number = 6;
    let hex: number = 0xf00d;
    let binary: number = 0b1010;
    let octal: number = 0o744;
    ```

3. String

    웹 페이지와 서버를 위한 JavaScript 프로그램을 만드는 또 다른 기본적인 부분은 텍스트 데이터를 사용하는 것입니다.
    다른 언어와 같이 이러한 텍스트의 데이터 타입을 참조하기 위해 `string` 타입을 사용합니다.
    JavaScript와 마찬가지로 TypeScript 또한 문자열 데이터를 감싸기 위해 큰 따옴표 또는 작은 따옴표를 사용합니다.

    ```typescript
    let color: string = 'blue';
    ```

    여러 줄에 걸쳐 표현식을 포함할 수 있는 *템플릿 문자열*을 사용할 수도 있습니다.

    ```typescript
    let fullName: string = 'Soulcactus';
    let age: number = 31;
    let sentence: string = `hello, my name is ${fullName}.
    
    I'll be ${age + 1} years old next year.`;
    ```

4. Array

    TypeScript는 JavaScript와 같이 값의 배열을 사용할 수 있도록 합니다.
    배열의 타입은 두 가지 방법 중 하나로 작성될 수 있습니다.

    첫번째 방법으로는 요소 타입의 배열을 나타내기 위해 `[]` 다음에 오는 요소의 타입을 사용합니다.

    ```typescript
    let list: number[] = [1, 2, 3];
    ```

    두번째 방법으로는 제네릭 배열 타입을 사용합니다. `Array<item Type>`

    ```typescript
    let list: Array<number> = [1, 2, 3];
    ```

5. Tuple

    튜플 타입은 고정된 개수의 요소 타입을 알고 있지만 반드시 같을 필요는 없는 배열을 표현할 수 있도록 합니다.
    예를 들어, 다음과 같은 `string`과 `number`의 쌍으로 값을 나타낼 수 있습니다.

    ```typescript
    let x: [string, number];

    x = ['hello', 10]; // good
    x = [10, 'hello']; // error
    ```

    알려진 인덱스를 사용하여 요소에 접근하는 경우에 올바른 타입이 검색됩니다.

    ```typescript
    console.log(x[0].substr(1)); // good
    console.log(x[1].substr(1)); // error
    ```

    알려진 인덱스 집합 외부의 요소에 접근할 때는 다음과 같이 union 타입이 사용됩니다.

    ```typescript
    x[3] = 'world'; // good! string은 string | number에 할당할 수 있습니다.
    console.log(x[5].toString()); // good! string 및 number에 모두 toString이 존재합니다.
    x[6] = true; // error! boolean은 string | number 타입이 아닙니다.
    ```

6. Enum

    JavaScript 표준 데이터 타입 집합에 추가할 수 있는 유용하고 부가적인 추가 자료는 `enum`입니다.
    C#과 같은 언어에서처럼 enum은 numeric 값 집합에 더 친숙한 이름을 부여하는 방법입니다.

    ```typescript
    enum Color {
        Red,
        Green,
        Blue,
    }

    let c: Color = Color['green'];
    ```

    일반적으로 enums는 0부터 시작하는 자신의 멤버 번호를 매기기 시작합니다.
    멤버 중 하나의 값을 수동으로 설정하여 이를 변경할 수 있습니다.
    예를 들어 이전 예제를 0대신 1로 시작할 수 있습니다.

    ```typescript
    enum Color {
        Red = 1,
        Green,
        Blue,
    }

    let c: Color = Color['green'];
    ```

    또는 모든 열거형의 값을 수동으로 설정합니다.

    ```typescript
    enum Color {
        Red = 1,
        Green = 2,
        Blue = 4,
    }

    let c: Color = Color['green'];
    ```

    enum의 편리한 기능은 숫자값에서 enum의 해당값 이름으로 이동할 수 있다는 점입니다.

    ```typescript
    enum Color {
        Red = 1,
        Green,
        Blue,
    }

    let colorName: string = Color[2];

    console.log(colorName); // Green
    ```

7. Any

    어플리케이션을 작성할 때 알지 못하는 변수의 타입을 설정해야 할 수도 있습니다.
    이러한 값은 사용자 또는 써드파티 라이브러리와 같은 동적 컨텐츠에서 비롯될 수 있습니다.
    이러한 경우에는 타입 검사를 선택하지 않고 그 값이 컴파일타임 검사를 통과하도록 해야하기 때문에 `any` 타입으로 지정합니다.

    ```typescript
    let notSure: any = 4;

    notSure = '문자열일 수도 있음';
    notSure = false; // good
    ```

    `any` 타입은 기존 JavaScript로 작업할 수 있는 강력한 방법으로, 컴파일 과정에서 타입 검사를 점진적으로 실행(opt-in) 및 중지(opt-out)할 수 있습니다.

    다른 언어와 마찬가지로 `Object`도 비슷한 역할을 할 것으로 예상할 수 있습니다.
    그러나 `Object` 타입의 변수를 사용하면 해당ㄹ `Object`에는 값만 할당할 수 있습니다.
    (실제로 존재하는 것도 임의의 메서드를 호출할 수 없습니다)

    ```typescript
    let notSure: any = 4;

    notSure.ifItExists(); // good! 런타임에 ifITExists가 존재할 수 있습니다.
    notSure.toFixed(); // good! 그러나 컴파일러는 체크하지 않습니다.

    let prettySure: Ojbect = 4;

    prettySure.toFixed(); // error! Object 타입에 toFixed 프로퍼티는 존재하지 않습니다.
    ```

    `any` 타입은 일부를 알고 있는 경우에도 유용하지만 언제나 그런 것은 아닙니다.
    예를 들어 배열이 있지만 배열에는 다른 타입이 혼재돼 있는 경우입니다.

    ```typescript
    let list: any[] = [1, true, 'free'];

    list[1] = 100;
    ```

8. void

    `void`는 `any`와 정반대이지만 조금 비슷합니다.
    어떠한 타입의 부재도 전혀 없습니다.
    일반적으로 반환값을 반환하지 않는 함수의 반환 타입으로 사용합니다.

    ```typescript
    function warnUser(): void {
        alert('This is my warning message');
    }
    ```

    `void` 타입의 변수 선언은 `undefined` 또는 `null`만 할당할 수 있으므로 유용하지 않습니다.

    ```typescript
    let unusable: void = undefined;
    ```

9. null과 undefined

    TypeScript에서 `undefined`와 `null`은 실제로 각기 `undefined`와 `null`이라는 자체적인 타입을 가집니다.
    `void`와 마찬가지로 그것들은 **매우 극단적으로** 유용하지 않습니다.

    ```typescript
    let u: undefined = undefined;
    let n: null = null;
    ```

    기본적으로 `null`과 `undefined`는 다른 모든 타입의 서브 타입입니다.
    즉 `null`과 `undefined`를 `number`와 같은 것으로 할당할 수 있습니다.
    그러나 `--strictNullChecks` 플래그를 사용할 때 `null`과 `undefined`는 `void`와 그 각각의 타입에만 할당할 수 있습니다.
    이렇게 하면 _많은_ 일반적인 오류를 피할 수 있습니다.

    `string` 또는 `null`, `undefined` 중 하나를 전달하고자 하는 경우 `string | null | undefined` (union 타입)을 사용할 수 있습니다.

10. Never

    `never` 타입은 절대로 발생하지 않는 값의 타입을 나타냅니다.
    예를 들어 `never`는 함수 표현식의 반환 타입이거나 항상 예외를 던지는 화살표 함수 표현식이거나, 절대 반환하지 않는 표현식입니다.
    변수는 또한 `never`일 때 타입 가드에 의해 좁혀지더라도 결국 사실일 수 없으며 타입을 획득하지 못합니다.

    `never` 타입은 모든 타입의 서브 타입이며 모든 타입에 할당할 수 있습니다.
    `never` 자체를 제외하면 어떤 타입도 `never`의 서브 타입이거나 할당 가능한 타입은 아닙니다.
    `any` 조차도 `never`에 할당할 수 없습니다.

    `never`를 반환하는 함수의 몇 가지 예는 다음과 같습니다.

    ```typescript
    // 반환되는 함수에는 연결할 수 없는 end-point가 있어서는 안 됩니다.

    function error(message: string): never {
        throw new Error(message);
    }

    // 추론되는 반환 타입은 절대로 없습니다.
    function fail() {
        return error('Something failed');
    }

    // 반환되는 함수에는 연결할 수 없는 end-point가 있어서는 안 됩니다.
    function infiniteLoop(): never {
        while (true) {}
    }
    ```

####

## 🎙 타입 단언(Type assertions)

때로는 TypeScript보다 더 많은 값을 알아야 하는 상황에 놓일 수 있습니다.
일반적으로 이 문제는 일부 엔티티의 타입이 현재 타입보다 더 구체적일 수 있다는 것을 알고 있을 때 발생합니다.

타입 단언은 컴파일러에게 "나를 믿어. 내가 하고 있는 일을 안다"라고 말하는 방법입니다.
타입 단언은 다른 언어의 형 변환(타입캐스팅)과 비슷하지만 특별한 검사나 데이터를 재구성하지는 않습니다.
런타임에 영향을 미치지 않으며 컴파일러에서만 사용됩니다.
TypeScript는 개발자가 필요한 특별한 검사를 수행했다고 가정합니다.

타입 단언은 두 가지 형태를 가집니다.
하나는 꺽쇠괄호 구문입니다.

```typescript
let someValue: any = 'this is a string';

let strLength: number = (<string>someValue).length;
```

다른 하나는 `as`입니다.

```typescript
let someValue: any = 'this is a string';

let strLength: number = (someValue as string).length;
```

두 예제는 동일하며 선호도의 문제이나 TypeScript를 JSX와 함께 사용할 때는 `as` 스타일의 단언만 허용됩니다.
