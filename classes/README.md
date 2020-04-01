# 클래스

전통적인 JavaScript는 재사용 가능한 컴포넌트를 만들기 위해 함수와 프로토타입 기반의 상속을 사용하지만
클래스가 함수를 상속하고 객체가 이러한 클래스에서 구축되는 객체 지향 접근 방식에 익숙하지 않은 개발자들에게는 다소 어색할 수 있습니다.
ECMAScript6로도 알려진 ECMAScript2015부터 JavaScript 개발자는 이 객체 지향 클래스 기반 접근 방식을 사용해 응용 프로그램을 구축할 수 있습니다.
TypeScript에서는 개발자가 이 기술을 사용하고 다음 JavaScript 버전을 기다리지 않고도 모든 메이저 브라우저와 플랫폼에서 작동하는 JavaScript로 컴파일할 수 있습니다.

## 🏫 클래스(Classes)

예제를 먼저 살펴보겠습니다.

```typescript
class Greeter {
    greeting: string;

    constructor(message: string) {
        this.greeting = message;
    }

    greet() {
        return `Hello, ${this.greeting}`;
    }
}

let greeter = new Greeter('world');
```
