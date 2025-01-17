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

예를 들어 위에서 사용한 매개변수를 선택적으로 사용할 수 있도록 아래와 같이 작성합니다.

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

모든 선택적 매개변수는 필수 매개변수를 따라와야 합니다.
last name 대신 firstName을 선택적 매개변수로 만들고 싶다면 함수의 매개변수 순서를 변경해야 합니다.
즉, 목록의 firstName을 마지막에 삽입해야 합니다.

TypeScript에서 사용자가 매개변수를 제공하지 않거나 사용자가 대신 `undefined`를 전달하더라도 매개변수에 할당되는 값을 설정할 수 있습니다.
이것을 기본 매개변수라고 합니다.

앞의 예제를 따라 lastName의 기본값을 변경해 보겠습니다.

```typescript
function buildName(firstName: string, lastName = 'Smith') {
    return `${firstName} ${lastName}`;
}

let result1 = buildName('Bob'); // Bob Smith
let result1 = buildName('Bob', undefined); // Bob Smith
let result1 = buildName('Bob', 'Adams', 'Sr.'); // error
let result1 = buildName('Bob', 'Adams'); // Bob Adams
```

필수 매개변수의 뒤에 오는 기본 매개변수는 선택적 매개변수로 취급되어 함수를 호출할 때 선택적 매개변수처럼 생략할 수 있습니다.
이것은 선택적 매개변수와 후행 기본 매개변수가 해당 타입의 공통점을 공유한다는 것을 의미합니다.

```typescript
function buildName(fitstName: string, lastName?: string) {
    /* ... */
}
```

```typescript
function buildName(firstName: string, lastName = 'Smith') {
    /* ... */
}
```

일반 선택적 매개변수와 달리 기본 매개변수는 필수 매개변수 뒤에 나올 필요가 없습니다.
기본 매개변수가 필수 매개변수 앞에 오는 경우 사용자는 명시적으로 `undefined`를 전달하여 기본 초기화된 값을 가져와야 합니다.

예를 들어 firstName에 기본 초기화만 있는 마지막 예제를 작성할 수 있습니다.

```typescript
function buildName(firstName = 'Will', lastName: string) {
    return `${firstName} ${lastName}`;
}

let result1 = buildName('Bob'); // error
let result2 = buildName('Bob', 'Adams', 'Sr.'); // error
let result3 = buildName('Bob', 'Adams'); // Bob Adams
let result4 = buildName(undefined, 'Adams'); // Will Adams
```

####

## 💭 나머지 매개변수(Rest Parameters)

필수 매개변수와 선택적 매개변수 그리고 기본 매개변수 모두 공통점이 하나 있습니다.
한번에 하나의 매개변수에 대해 이야기한다는 점입니다.
때로는 여러 매개변수를 그룹으로 사용하거나 함수가 마지막으로 가져올 매개변수의 수를 모를 수 있습니다.
JavaScript에서는 모든 함수 본문에서 볼 수 있는 `arguments`를 사용해 인수를 직접 사용할 수 있습니다.

TypeScript에서는 다음과 같은 인수를 변수로 함께 모을 수 있습니다. (ES6 이상에서도 가능합니다.)

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return `${firstName} ${restOfName.join(' ')}`;
}

let employeeName = buildName('Joseph', 'Samuel', 'Lucas', 'MacKinzie');
```

나머지 매개변수는 무한적인 수의 선택적 매개변수로 처리됩니다.
생략 부호는 나머지 매개변수가 있는 함수의 타입에도 사용됩니다.

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + ' ' + restOfName.join(' ');
}

let buildNameFun: (fname: string, ...rest: string[]) => string = buildName;
```

####

## ☛ this

`this`가 JavaScript에서 어떻게 쓰이는지 아는 것은 일종의 통과의례입니다.
TypeScript는 JavaScript의 상위 집합이므로 TypeScript 개발자들 역시 this가 어떻게 쓰이는지 또는 this가 잘못 쓰일 때를 발견하는 방법을 배울 필요가 있습니다.
다행히 TypeScript는 몇 가지 기술들로 잘못된 `this` 사용을 잡아낼 수 있습니다.

### this와 화살표 함수

JavaScript에서 `this`는 함수가 호출될 때 정해지는 변수입니다.
매우 강력하고 유연한 기능이지만 이것은 항상 함수가 실행되는 컨텍스트에 대해 알아야 한다는 수고가 생깁니다.
특히 함수를 반환하거나 인자로 넘길 때의 혼란스러움은 악명 높습니다.
예제 코드를 통해 살펴보겠습니다.

```typescript
let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function() {
        return function() {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert(`card: ${pickedCard.card} of ${pickedCard.suit});
```

`createCardPicker`가 자시 자신을 반환하는 함수임에 주목해야 합니다.
이 예제를 작동시키면 기대했던 경보창 대신 오류가 발생합니다.
`createCardPicker`에 의해 생성된 함수에서 사용중인 `this`가 `deck` 객체가 아닌 `window`에 설정됐기 때문입니다.
`cardPicker()`의 자체적인 호출 때문에 생긴 일입니다.
최상위 레벨에서의 비메서드 문법의 호출은 `this`를 `window`로 설정합니다.
참고로 strict mode에서는 `this`가 `window`대신 `undefined`가 됩니다.

이 문제는 나중에 사용할 함수를 반환하기 전에 바인딩을 알맞게 하는 것으로 해결할 수 있습니다.
이 방법대로라면 나중에 사용하는 방법에 상관없이 원본 `deck` 객체를 계속해서 볼 수 있습니다.
이를 위해, 함수 표현식을 ES6ㅇ릐 화살표 함수로 바꿀 것입니다.
화살표 함수는 함수가 호출된 곳이 아닌 함수가 생성된 쪽의 `this`를 캡쳐합니다.

```typescript
let deck = {
    suits: ['hearts', 'spades', 'clubs', 'diamonds'],
    cards: Array(52),
    createCardPicker: function () {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
        };
    },
};

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert('card: ' + pickedCard.card + ' of ' + pickedCard.suit);
```

### this 매개변수

불행히도 `this.suits[pickedSuit]`의 타입은 여전히 `any`입니다.
`this`가 객체 리터럴 내부의 함수에서 왔기 때문입니다.
이것을 고치기 위해 명시적으로 `this` 매개변수를 줄 수 있습니다.
`this` 매개변수는 함수의 매개변수 목록에서 가장 먼저 나오는 가짜 매개변수입니다.

```typescript
function f(this: void) {
    // 독립형 함수에서 `this`를 사용할 수 없습니다.
}
```

명확하고 재사용하기 쉽게 `Card`와 `Deck` 두 가지 인터페이스 타입들을 예시에 추가해 보겠습니다.

```typescript
interface Card {
    suit: string;
    card: number;
}
interface Deck {
    suits: string[];
    cards: number[];
    createCardPicker(this: Deck): () => Card;
}
let deck: Deck = {
    suits: ['hearts', 'spades', 'clubs', 'diamonds'],
    cards: Array(52),
    // 아래 함수는 이제 callee가 반드시 Deck 타입이어야 함을 명시적으로 지정합니다.
    createCardPicker: function (this: Deck) {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
        };
    },
};

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert('card: ' + pickedCard.card + ' of ' + pickedCard.suit);
```

이제 TypeScript는 `createCardPicker`가 `Deck` 객체에서 호출된다는 것을 알게 됐습니다.
이것은 `this`가 `any` 타입이 아니라 `Deck` 타입이며 따라서 `--noImplicitThis` 플래그가 어떤 오류도 일으키지 않는다는 것을 의미합니다.

### 콜백에서 this 매개변수

나중에 호출할 콜백 함수를 라이브러리에 전달할 때 `this` 때문에 오류가 발생할 수 있습니다.
라이브러리는 콜백을 일반 함수처럼 호출하므로 `this`는 `undefined`가 됩니다.
일부 작업에서는 `this` 매개변수를 콜백 오류를 막는데 사용할 수 있습니다.
먼저 라이브러리 작성자는 콜백 타입을 `this`로 표시해야 합니다.

```typescript
interface UIElement {
    addClickListener(onclick: (this: void, e: Event) => void): void;
}
```
