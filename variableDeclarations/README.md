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
