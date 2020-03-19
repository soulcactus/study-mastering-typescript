# 변수 선언

`let`과 `const`는 JavaScript에서 상대적으로 새로운 유형의 변수 선언입니다.
이전에 언급했듯 `let`은 `var`과 유사하지만 사용자가 JavaScript로 실행하는 일반적인 *결함*을 피할 수 있습니다.
const는 변수에 재할당할 수 없다는 점에서 let의 기능 확장입니다.

TypeScript가 JavaScript의 상위 집합ㅇ리기 때문에 자연스럽게 `let`과 `const`를 지원합니다.
이 새로운 선언들에 대해 더욱 자세히 살펴보고 왜 그것들이 `var`보다 더 바람직한지 알아보도록 하겠습니다.

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
