# 변수 선언

`let`과 `const`는 JavaScript에서 상대적으로 새로운 유형의 변수 선언입니다.
이전에 언급했듯 `let`은 `var`과 유사하지만 사용자가 JavaScript로 실행하는 일반적인 *결함*을 피할 수 있습니다.
const는 변수에 재할당할 수 없다는 점에서 let의 기능 확장입니다.

TypeScript가 JavaScript의 상위 집합이기 때문에 자연스럽게 `let`과 `const`를 지원합니다.
이 새로운 선언들에 대해 더욱 자세히 살펴보고 왜 그것들이 `var`보다 더 바람직한지 알아보도록 하겠습니다.

**참고로 이번 파트는 TypeScript와 관련된 내용이라기보다는 JavaScript의 특징 설명에 가깝습니다.**

## 🌦 var 선언(var declarations)

전통적으로 JavaScript에서 변수 선언은 `var` 키워드를 사용했습니다

```javascript
var a = 10;

function f() {
    var message = 'hello, world!';

    return message;
}
```

다른 함수 내부의 동일한 변수에 접근 가능합니다.

```javascript
function f() {
    var a = 10;

    return function g() {
        var b = a + 1;

        return b;
    };
}

var g = f();
g(); // 11
```

위의 예제에서 함수 g는 함수 f에서 선언된 변수 a를 획득합니다.
함수 g가 호출되는 시점에 a 값은 함수 f 내의 a 값에 연결됩니다.
함수 f가 실행되는 시점에 g 함수가 호출되더라도 a에 접근해 수정할 수 있습니다.

```javascript
function f() {
    var a = 1;

    a = 2;

    var b = g();

    a = 3;

    return b;

    function g() {
        return a;
    }
}

f(); // 2
```

####

## 🔗 스코프 규칙(Scoping rules)

`var` 선언은 다른 언어의 스코프 규칙에 비해 이상한 스코프 규칙이 몇 가지 있습니다.
예제를 통해 살펴보겠습니다.

```typescript
function f(shouldInitialize: boolean) {
    if (shouldInitialize) {
        var x = 10;
    }

    return x;
}

f(true); // 10
f(false); // undefined
```

변수 x는 if 문 내에 선언돼 있지만 블록 바깥에서 접근할 수 있습니다.
`var` 선언은 함수, 모듈, 네임 스페이스 또는 전역 스코프에서 접근할 수 있기 때문에 가능합니다.
이를 var-scoping 또는 function-scoping이라고 합니다.
매개변수 또한 함수 스코프에 해당합니다.

이러한 스코프 규칙은 몇 가지 유형의 실수를 유발할 수 있습니다.

```typescript
function sumMatrix(matrix: number[][]) {
    var sum = 0;

    for (var i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];

        sum += currentRow[i];
    }

    return sum;
}
```

반복문 내에서 동일한 함수 스코프의 변수를 참조하기 때문에 실수로 변수 i를 덮어쓰는 실수 등이 발생할 수 있습니다.

####

## ⚠️ 변수 캡쳐링의 단점

```javascript
for (var i = 0; i< 10; i++) {
    setTimeout(function() { console.log(i); }, 100 * i});
}

// 10
// 10
// 10
// 10
// 10
// 10
// 10
// 10
// 10
// 10
```

`setTimeout`에 전달하는 모든 함수 표현식은 실제로 동일한 스코프의 i를 참조합니다.
또한 반복문은 `setTimeout`의 콜백함수가 실행되기까지 기다리지 않고 자신의 작업을 완료합니다.
반복문이 실행이 완료됐을 때 i의 값은 10이기 때문에 오직 10만 출력됩니다.

일반적인 해결 방법은 각 반복마다 i를 캡쳐하는 즉시 실행함수 표현식인 IIFE(Immediately Invoked Function Expressions)를 사용하는 것입니다.

```javascript
for (var i = 0; i < 10; i++) {
    (function(i) {
        setTimeout(function() {
            console.log(i);
        }, 100 * i);
    })(i);
}
```

####

## 🌝 let 선언(let declarations)

`var` 선언에는 앞서 설명한 것과 같은 문제점들이 있었기 때문에 `let`이 도입됐습니다.
`let`은 `var`와 동일한 방식으로 작성됩니다.

```javascript
let hello = 'hello!';
```

####

## 🛣️ 블록 스코프(Block-scoping)

`let`을 사용하여 변수르 선언할 때 렉시컬 스코프 또는 블록 스코프를 사용합니다.
스코프가 포함된 함수로 누출되는 `var` 변수와 달리 블록 스코프 변수는 가장 가깝게 포함된 블록 또는 반복문 외부에서 사용할 수 없습니다.

```typescript
function f(input: boolean) {
    let a = 100;

    if (input) {
        let b = a + 1;

        return b;
    }

    return b; // error
}
```

두 개의 지역변수 a와 b가 있습니다.
a의 스코프는 함수 f의 지역변수로 제한돼 있고, b의 스코프는 if문 블록에 제한돼 있습니다.
catch문 안에 선언된 변수에도 유사한 스코프 규칙이 적용됩니다.

```javascript
try {
    throw 'oh no!';
} catch (e) {
    console.log('oh well.');
}

console.log(e); // error
```

####

## 👥 재선언과 Shadowing(Re-declarations and Shadowing)

`var`를 선언한 횟수가 중요하지 않다고 언급했습니다.
단지 변수를 하나 얻었을 뿐입니다.
아래 예제에서 변수 x의 모든 선언은 실제로 동일한 x를 참조하며 이는 완전히 유효합니다.
이것은 종종 버그의 원인이 됩니다.

```javascript
function f(x) {
    var x;
    var x;

    if (true) {
        var x;
    }
}
```

`let`에서는 허용되지 않습니다.

```javascript
let x = 10;
let x = 20; // error
```

TypeScript에서도 블록 스코프에 두 개의 같은 변수가 필요하지 않다고 알립니다.

```typescript
function f(x) {
    let x = 100; // error
}

function g() {
    let x = 100;
    let x = 100; // error
}
```

블록 스코프 변수가 함수 스코프 변수로 선언될 수 없다는 뜻은 아닙니다.
블록 스코프 변수는 명확하게 다른 블록 내에서 선언돼야 합니다.

```javascript
function f(condition, x) {
    if (condition) {
        let x = 100;

        return x;
    }

    return x;
}

f(false, 0); // 0
f(true, 0); // 100
```

중첩된 스코프에서 기존 변수의 이름을 사용하는 것을 *shadowing*이라고 합니다.
shadowing은 우발적인 버그를 유발하는 동시에 버그를 예방할 수 있다는 점에서 양날의 검입니다.
예를 들어, `let` 변수를 사용하여 이전 함수인 subMatrix를 작성했다고 가정해 보겠습니다.

```typescript
function subMatrix(matrix: number[][]) {
    let sum = 0;

    for (let i = 0; i < currentRow.length; i++) {
        sum += currentRow[i];
    }

    return sum;
}
```

이번 버전의 반복문은 내부으 반복문의 i가 외부 반복문의 i를 shadows하기 때문에 실제로 계산을 합니다.
shadowing은 보통 더 명확한 코드를 작성하기 위해 피해야 합니다.
이를 활용할 수 있는 몇 가지 시나리오가 있을 수 있지만 최선의 판단을 내려야 합니다.

####

## 📷 블록 스코프 변수 캡쳐

```javascript
function theCity() {
    let getCity;

    if (true) {
        let city = 'seoul';

        getCity = function() {
            return city;
        };
    }

    return getCity();
}
```

환경 내에서 변수 city를 캡쳐했으므로 조건문 블록이 실행을 완료했음에도 불구하고 여전히 접근할 수 있습니다.
이전의 `setTimeout` 예제에서 반복문을 반복할 때마다 변수의 상태를 캡쳐하기 위해 IIFE를 사용해야 한다는 결론을 얻었습니다.
실제로 작동하는 방식은 캡쳐한 변수를 위한 새로운 변수 환경을 만드는 것이었습니다.
다행스럽게도 ES6 이상 및 TypeScript에서는 그렇게 할 필요가 없습니다.

`let`은 루프의 일부로 선언될 때 크게 다른 동작을 합니다.
루프 자체에 새로운 환경을 도입하기는 것이 아니라 반복마다 새로운 스코프를 만듭니다.
때문에 IIFE로 구현한 것을 `let`을 사용하여 이전의 `setTimeout` 예제를 바꿀 수 있습니다.

```javascript
for (let i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 100 * i);
}

// 0
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9
// 10
```

####

## 🌞 const 선언(const declarations)

`const`는 변수를 선언하는 또 다른 방법입니다.

```javascript
const numLivesForCat = 0;
```

`let` 선언과 선언 방법은 같지만 이름에서 알 수 있듯 바인딩된 후에는 값을 변경할 수 없습니다.
즉, `let`과 동일한 스코프 규칙을 갖고 있지만 값을 다시 할당할 수 없습니다.
*불변성*과는 의미가 다릅니다.

```javascript
const numLivesForCat = 9;
const kitty = {
    name: 'Aurora',
    numLives: numLivesForCat,
};

kitty = {
    // error
    name: 'Danielle',
    numLives: numLivesForCat,
};

kitty.name = 'Rory'; // good
kitty.name = 'Kitty'; // good
kitty.name = 'Cat'; // good
kitty.numLives--; // good
```

####

## 🔨 비구조화(Destructuring)

1.  배열 비구조화(Array destructuring)

    가장 간단한 구조 해체 방법은 배열 비구조화 할당입니다.

    ```javascript
    let input = [1, 2];
    let [first, second] = input;

    console.log(first); // 1
    console.log(second); // 2
    ```

    이는 인덱스를 사용하는 것과 동일하지만 훨씬 직관적입니다.
    비구조화는 이미 선언된 변수에서도 작동합니다.

    ```javascript
    [first, second] = [second, first]; // 변수 교환
    ```

    함수의 매개변수로 사용하는 경우는 다음과 같습니다.

    ```typescript
    function f([first, second]: [number, number]) {
        console.log(first);
        console.log(second);
    }

    f([1, 2]);
    ```

    전개 연산자를 이용해 목록의 나머지 항목에 대한 변수를 생성할 수 있습니다.

    ```javascript
    let [first, ...rest] = [1, 2, 3, 4];

    console.log(first); // 1
    console.log(rest); // [ 2, 3, 4 ]
    ```

    다음과 같이도 사용할 수 있습니다.

    ```javascript
    let [, second, , fourth] = [1, 2, 3, 4];
    ```

2.  객체 비구조화(Object destructuring)

    객체 또한 해체할 수 있습니다.

    ```javascript
    let o = {
        a: 'foo',
        b: 12,
        c: 'bar',
    };

    let { a, b } = o;
    ```

    배열 비구조화처럼 선언없이 할당할 수 있습니다.

    ```javascript
    ({ a, b } = { a: 'baz', b: 101 });
    ```

    JavaScript는 일반적으로 `{` 를 블록의 시작으로 파싱하기 때문에 문장을 괄호로 묶어야합니다.

    전개 연산자를 이용해 객체의 나머지 항목에 대한 변수 역시 만들 수 있습니다.

    ```javascript
    let { a, ...passthrough } = o;

    let total = passthrough.b + passthrough.c.length;
    ```

    3. 프로퍼티 이름 변경(Property renaming)

    프로퍼티의 이름 또한 다른 이름으로 지정할 수 있습니다.

    ```javascript
    let { a: newName1, b: newName2 } = o;
    ```

    `a: newName1`을 `a as newName1`로 읽을 수 있습니다.

    ```javascript
    let newName1 = o.a;
    let newName2 = o.b;
    ```

    여기서 콜론은 타입을 나타내는 콜론은 아닙니다.
    형식을 지정하는 경우 전체 형식이 비구조화된 후에도 형식을 작성해야 합니다.

    ```typescript
    let { a, b }: { a: string; b: number } = o;
    ```

    4. 기본값 (Default values)

    기본값을 사용하면 프로퍼티가 정의되지 않은 경우의 기본값을 지정할 수 있습니다.

    ```typescript
    function keepWholeObject(wholeObject: { a: string; b?: number }) {
        let { a, b = 1001 } = wholeObject;
    }
    ```

    b?는 b가 선택적이라는 것을 의미합니다.
    따라서 함수 keepWholeObject는 b가 정의되지 않더라도 a, b 프로퍼티뿐만 아니라 wholeObject의 모든 프로퍼티를 가집니다.

    5. 함수 선언(Function declarations)

    비구조화는 함수 선언에서도 작동합니다.
    간단한 예를 살펴보겠습니다.

    ```typescript
    type C = { a: string; b?: number };

    function f({ a, b }: C): void {
        // ...
    }
    ```

    ```typescript
    function f({ a, b } = { a: '', b: 0 }): void {
        // ...
    }

    f(); // good! { a: "", b: 0 }
    ```

    ```typescript
    function f({ a, b = 0 } = { a: '' }): void {
        // ...
    }
    f({ a: 'yes' }); // good! { a: "yes", b: 0 }
    f(); // good! { a: "", b: 0 }
    f({}); // error
    ```

    비구조화는 유의해서 사용해야 합니다.
    가장 단순한 비구조화 표현식을 제외하고는 혼란스러운 면이 많습니다.
    이름 바꾸기 기본값 및 타입을 주석으로 써놓지 않고는 이해하기 힘든 깊은 형태를 비구조화하는 것은 특히 그렇습니다.
    비구조화 표현식은 작고 심플하게 유지하는 것이 좋습니다.

####

## 📖 전개 연산자(Spread)

전개는 비구조화의 반대입니다.
배열을 다른 배열로, 객체를 다른 객체로 전개하는 것을 허용합니다.

```javascript
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];
```

bothPlus에 `[0, 1, 2, 3, 4, 5]`가 할당됩니다.
전개 연산자는 first와 second의 얕은 복사본을 만듭니다.
또한 객체 역시 전개 연산자를 이용해 전개할 수 있습니다.

```javascript
let defaults = { food: 'spicy', price: '$$', ambiance: 'noisy' };
let search = { ...defaults, food: 'rich' };
```
