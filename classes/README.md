# 클래스

클래스는 데이터와 동작을 포함하는 객체를 정의합니다.
클래스와 인터페이스는 객체지향 프로그래밍의 기본 구성으로, 자주 디자인 패턴과 함께 쓰입니다.
디자인 패턴은 일반적 프로그래밍 문제를 클래스와 인터페이스로 해결하는 데 도움이 되는 방법입니다.

전통적인 JavaScript는 재사용 가능한 컴포넌트를 만들기 위해 함수와 프로토타입 기반의 상속을 사용하지만
클래스가 함수를 상속하고 객체가 이러한 클래스에서 구축되는 객체 지향 접근 방식에 익숙하지 않은 개발자들에게는 다소 어색할 수 있습니다.
ECMAScript6로도 알려진 ECMAScript2015부터 JavaScript 개발자는 이 객체 지향 클래스 기반 접근 방식을 사용해 응용 프로그램을 구축할 수 있습니다.
TypeScript에서는 개발자가 이 기술을 사용하고 다음 JavaScript 버전을 기다리지 않고도 모든 메이저 브라우저와 플랫폼에서 작동하는 JavaScript로 컴파일할 수 있습니다.

## 🏫 클래스(Classes)

설명에 앞서 예제를 먼저 살펴보겠습니다.

```typescript
class SimpleClass {
    id: number;

    print(): void {
        console.log(`print called ${this.id}.`);
    }
}

let mySimpleClass = new SimpleClass();

mySimpleClass.print();
```

`class` 키워드로 SimpleClass 클래스를 정의했습니다. SimpleClass는 id, name의 속성과 print 메서드를 갖습니다.
클래스에서 자기 자신의 속성에 접근하려면 `this` 키워드를 사용해야합니다.
print는 콘솔에 간단한 메시지를 기록합니다.
클래스 인스턴스를 할당할 변수를 지정하고 `new` 키워드로 클래스의 인스턴스를 만들었습니다.

또 다른 예제를 살펴보겠습니다.

```typescript
class ClassWithContructor {
    id: number;
    name: string;

    constructor(_id: number, _name: string) {
        this.id = _id;
        this.name = _name;
    }
}

let classWithConstructor = new ClassWithContructor(1, 'name');
```

숫자 타입의 id, 문자열 타입의 name 속성을 갖는 ClassWithContructor 클래스를 정의했습니다.
클래스는 생성자에서 인자를 받아 클래스를 생성하면서 값을 설정하는 동작을 한줄로 만들 수 있습니다.
ClassWithContructor 클래스는 두 개의 인자를 받는 생성자를 갖고 있습니다.
생성자는 \_id 인자의 값을 id 속성에, \_name 인자의 값을 name 속성에 저장합니다.

또 다른 예제를 살펴보겠습니다.

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

이전에 C# 또는 Java를 사용한 적 있는 경우 위 구문이 익숙할 수 있습니다.

먼저 새로운 클래스인 `Greeter`를 선언합니다.
이 클래스에는 `greeting` 프로퍼티와 생성자, `greet` 함수 3개의 멤버가 있습니다.
클래스의 멤버 중 하나를 참조할 때 `this`를 접두어로 붙입니다.
이것은 멤버에 접근하는 것을 뜻합니다.
마지막 줄에서는 `new`를 사용하여 `Greeter` 클래스의 인스턴스를 만들었습니다.
이것은 이전에 정의한 생성자를 호출해 `Greeter` 형태의 새 객체를 만들고 생성자를 실행해 이를 초기화합니다.

클래스 안의 함수에서는 다음과 같은 일을 할 수 있습니다.

-   강타입을 사용할 수 있습니다.
-   강타입을 우회하려면 `any` 키워드를 사용해야 합니다.
-   선택적 인자를 가질 수 있습니다.
-   기본 인자를 가질 수 있습니다.
-   인자 배열을 사용하거나 나머지 인자 구문을 사용할 수 있습니다.
-   함수 콜백과 콜백 함수 시그니처를 사용할 수 있습니다.
-   함수 오버로드를 사용할 수 있습니다.

다양한 함수 시그니처를 가진 클래스 예제를 통해 각각의 규칙을 살펴보겠습니다.

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

class ComplexType implements IComplexType {
    id: number;
    name: string;
    constructor(idArg: number, nameArg: string);
    constructor(idArg: string, nameArg: string);

    constructor(idArg: any, nameArg: any) {
        this.id = idArg;
        this.name = nameArg;
    }

    print(): string {
        return `id: ${this.id}, name: ${this.name}`;
    }

    usingTheAnyKeyword(arg1: any): any {
        this.id = arg1;
    }

    usingOptionalParameters(optionalArg1?: number) {
        if (optionalArg1) {
            this.id = optionalArg1;
        }
    }

    usingDefaultParameters(defaultArg1: number = 0) {
        this.id = defaultArg1;
    }

    usingRestSyntax(...argArray: number[]) {
        if (argArray.length > 0) {
            this.id = argArray[0];
        }
    }

    usingFunctionCallbacks(callback: (id: number) => string) {
        callback(this.id);
    }
}
```

첫번째는 constructor 함수입니다.
ComplexType 클래스는 생성자인 함수를 오버로딩해 숫자와 문자열, 문자열과 문자열 두 가지 조합으로 클래스를 생성할 수 있게 합니다.
생성자 함수 안에서 idArg 인자는 id 속성에 할당됩니다.
클래스의 id 속성 타입은 숫자로 정의했지만, 생성자 오버로드로 문자열을 넣어 생성자를 호출할 수 있습니다.

이어서 usingTheAnyKeyword 함수를 살펴보겠습니다.

```typescript
let ct_1 = new ComplexType();

ct_1.usingTheAnyKeyword(true);
ct_1.usingTheAnyKeyword({ id: 1, name: 'string' });
```

위의 예제의 첫번째 usingTheAnyKeyword 함수 호출은 불리언 값을 사용하고 두번째 호출은 임의의 객체를 사용합니다.
인자 타입이 `any`이므로 둘 다 유효한 호출입니다.

다음으로는 usingOptionalParameters 함수를 살펴보겠습니다.

```typescript
ct_1.usingDefaultParameters(2);
ct_2.usingDefaultParameters();
```

두 가지 호출 모두 유효합니다.
첫번째는 기본값인 0 대신 값을 넣었고, 두번째는 인자 없이 호출해 기본값인 0을 사용합니다.

이어서 usingRestSyntax 함수를 살펴보겠습니다.

```typescript
ct_1.usingRestSyntax(1, 2, 3);
ct_1.usingRestSyntax(1, 2, 3, 4, 5);
```

usingRestSyntax 함수는 나머지 인자를 사용하기 때문에 몇 개의 인자든 넣을 수 있습니다.
인자는 배열에 저장되고 두가지 호출 방식 모두 유효합니다.

마지막으로 usingFunctionCallbacks 함수를 살펴보겠습니다.

```typescript
function myCallbackFunction(id: number): string {
    return id.toString();
}

ct_1.usingFunctionCallbacks(myCallbackFunction);
```

위의 예제는 usingFunctionCallbacks 함수에서 사용하는 콜백 함수 시그니처에 맞는 myCallbackFunction 함수를 정의합니다.
usingFunctionCallbacks 함수에 myCallbackFunction를 인자로 호출할 수 있습니다.

####

## 🐟 상속(Inheritance)

TypeScript에서는 일반적인 객체 지향 패턴을 사용할 수 있습니다.
클래스 기반 프로그래밍에서 가장 기본적인 패턴 중 하나는 상속을 사용해 기존 클래스를 확장하여 새로운 클래스를 생성할 수 있는 것입니다.
예제를 통해 살펴보겠습니다.

```typescript
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal move ${distanceInMeters}m.`);
    }
}

class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!');
    }
}

const dog = new Dog();

dog.bark(); // Woof! Woof!
dog.move(10); // Animal move 10m.
dog.bark(); // Woof! Woof!
```

이 예제는 가장 기본적인 상속 기능을 보여줍니다.
클래스는 기본 클래스에서 속성과 메서드를 상속받습니다.
여기서 Dog는 extends 키워드를 사용해 Animal 기본 클래스에서 유래된 파생 클래스입니다.
파생 클래스는 종종 하위 클래스(subclasses)라고 하며 기본 클래스는 수퍼 클래스(superclasses)라고도 합니다.

Dog는 Animal로부터 기능을 확장했기 때문에 `bark()`와 `move()` 메서드를 둘 다 사용할 수 있는 Dog의 인스턴스를 만들 수 있습니다.
조금 더 복잡한 예제를 살펴보겠습니다.

```typescript
class Animal {
    name: string;

    constructor(theName: string) {
        this.name = theName;
    }

    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) {
        super(name);
    }

    move(distanceInMeters = 5) {
        console.log('Slitering...');
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) {
        super(name);
    }

    move(distanceInMeters = 45) {
        console.log('Galloping...');
        super.move(distanceInMeters);
    }
}

let sam = new Snake('Sammy the Python');
let tom: Animal = new Horse('Tommy the Palomino');

sam.move();
// Silithering...
// Sammy the Python moved 5m.
tom.move(34);
// Galloping...
// Tommy the Palomino moved 34m.
```

위의 예제는 앞서 언급하지 않은 몇 가지 다른 기능을 다룹니다.
이번에도 Animal의 새로운 하위 클래스인 Horse와 Snake를 만드는 extends 키워드가 등장합니다.

이전 예제와 한 가지 다른 점은 생성자 함수를 포함하는 각 파생 클래스가 기본 클래스의 생성자를 실행할 `super()`를 호출해야 한다는 점입니다.
게다가 생성자 내에서 `this`에 있는 프로퍼티에 접근하기 전에 항상 `super()`를 호출해야 합니다.
이것은 TypeScript에서 적용해야할 중요한 규칙입니다.

또한 이 예제에서는 기본 클래스의 메서드를 하위 클래스에 특화된 메서드로 오버라이드 하는 방법도 보여 줍니다.
여기에서 Snake와 Horse는 Animal의 move를 오버라이드하고 각 클래스에 고유한 기능을 부여하는 move 메서드를 만듭니다.
tom은 Animal로 선언됐지만 Horse의 값을 가지므로 `tom.move(34)`를 호출하면 Horse의 오버라이딩 메서드가 호출됩니다.

####

## 🔑 Public, Private, Protected

### public

다른 언어의 클래스에 익숙하다면 위의 예제에서 `public`을 사용하지 않아도 된다는 사실을 눈치챘을 것입니다.
예를 들어 C#의 경우 각 멤버를 `public`으로 표시하도록 명시해야 합니다.
TypeScript에서는 기본적으로 각 멤버가 `public`입니다.
그럼에도 불구하고 `public` 멤버를 명시적으로 표기할 수 있습니다.
이전 섹션의 Animal 클래스를 다음과 같이 작성할 수 있습니다.

```typescript
class Animal {
    public name: string;

    public constructor(theName: string) {
        this.name = theName;
    }

    public move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

`public`으로 설정한 클래스 속성은 어디에서나 접근할 수 있습니다.

```typescript
class ClassWithPublicProperty {
    public id: number;
}

let publicAccess = new ClassWithPublicProperty();

publicAccess.id = 10;
```

id 속성을 갖는 ClassWithPublicProperty 클래스를 정의했습니다.
publicAccess라는 이름으로 인스턴스를 생성하고 인스턴스의 id 속성에 10을 할당했습니다.

### private

먼저 위의 예제의 속성을 `private`로 설정하면 어떻게 되는지 살펴보겠습니다.

```typescript
class ClassWithPrivateProperty {
    private id: number;

    constructor(_id: number) {
        this.id = _id;
    }
}

let privateAccess = new ClassWithPrivateProperty(10);

privateAccess.id = 20;
```

id 속성을 갖는 ClassWithPrivateProperty 클래스를 정의했습니다.
id 속성은 private로 성정했습니다.
ClassWithPrivateProperty 클래스의 생성자는 id 속성에 인자로 받은 \_id 값을 할당합니다.
생성자 안에서 this.id 구문을 사용한다는 점에 주목하시기 바랍니다.
privateAccess라는 이름으로 인스턴스를 만들어 private로 돼 있는 id 속성에 20을 할당하면 예제는 오류를 발생시킵니다.

오류 내용을 통해 TypeScript는 `private` 속성으로 돼 잇는 경우, 클래스 밖에서는 값을 할당할 수 없음을 알 수 있습니다.
다른 예제 코드를 통해 살펴보겠습니다.

```typescript
class Animal {
    private name: string;

    constructor(theName: string) {
        this.name = theName;
    }
}

new Animal('Cat').name; // error
```

TypeScript는 구조적인 타입의 시스템입니다.
두 개의 다른 타입을 비교할 때 그것들이 어디에서 왔는지에 관계없이 모든 멤버의 타입이 호환 가능하다면 그 타입 자체가 호환성이 있다고 표현합니다.

그러나 `private` 및 `protected` 멤버가 있는 타입을 비교할 때 이런 타입들은 다르게 처리됩니다.
호환성이 있는 것으로 판단되는 두 가지 타입 중 `private` 멤버가 있는 경우 다른 멤버는 동일한 선언에서 유래된 `private` 멤버가 있어야 합니다.
이것은 `protected` 멤버에도 적용됩니다.
예제 코드를 통해 살펴보겠습니다.

```typescript
class Animal {
    private name: string;

    constructor(theName: string) {
        this.name = theName;
    }
}

class Rhino extends Animal {
    constructor() {
        super('Rhino');
    }
}

class Employee {
    private name: string;

    constructor(theName: string) {
        this.name = theName;
    }
}

let animal = new Animal('Goat');
let rhino = new Rhino();
let employee = new Employee('Bob');

animal = rhino; // good
animal = employee; // error
```

위의 예제에서는 Animal과 Rhino 클래스가 있습니다.
Rhino는 Animal의 하위 클래스입니다.
또한 Animal과 같아 보이는 Employee라는 새로운 클래스가 있습니다.

Animal과 Rhino는 Animal의 `private name: string` 선언으로부터 `private` 형태를 공유하기 때문에 호환됩니다.
그러나 Employee의 경우에는 그렇지 않습니다.

Employee를 Animal에 할당하려고 할 때 이 타입들은 호환되지 않는 오류가 발생합니다.
Employee도 name이라는 `private` 멤버가 있지만 Animal에서 선언한 것이 아니기 때문입니다.

### protected

`protected` 지정자는 `private` 지정자와 매우 유사하게 동작합니다.
`protected` 멤버도 선언된 파생 클래스의 인스턴스에서 접근할 수 있습니다.
예제 코드를 통해 살펴보겠습니다.

```typescript
class Person {
    protected name: string;

    constructor(name: string) {
        this.name = name;
    }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee('Howord', 'Sales');

console.log(howard.getElevatorPitch()); // Hello, my name is Howord and I work in Sales.
console.log(howard.name); // error
```

Person의 외부에서 name을 사용할 수는 없지만 Employee는 Person으로부터 파생되기 때문에 Employee의 인스턴스 메서드 내에서 여전히 사용할 수 있습니다.
`private`와 `protected`의 차이점은 `private`로 설정된 속성과 메서드는 현재 class에서만 접근 가능하고 자식 class에서 직접 접근할 수 없다는 것입니다.

생성자 또한 `protected`로 표시할 수 있습니다.
즉, 클래스를 포함하는 클래스 외부에서 클래스를 인스턴스화 할 수는 없지만 확장할 수는 있습니다.
예제 코드를 통해 살펴보겠습니다.

```typescript
class Person {
    protected name: string;

    protected constructor(theName: string) {
        this.name = theName;
    }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee('Howard', 'Sales'); // Hello, my name is Howard and I work in Sales.
let john = new Person('John'); // error
```

### Readonly

TypeScript에는 `public`, `private`, `protected` 접근 제어자와 더불어 클래스 속성에 readonly 접근 제어자가 있습니다.
`readonly`로 설정된 속성은 값을 한번 설정하면 클래스 안에서도 수정할 수 없습니다.
`readonly` 속성값은 클래스의 생성자에서만 설정할 수 있습니다.
예제를 통해 살펴보겠습니다.

```typescript
class ClassWithReadOnly {
    readonly name: string;

    constructor(_name: string) {
        this.name = _name;
    }

    setReadOnly(_name: string) {
        this.name = _name; // error
    }
}
```

비슷한 예제를 하나 더 살펴보겠습니다.

```typescript
class Octopus {
    readonly name: string;

    readonly numberOfLegs: number = 8;

    constructor(theName: string) {
        this.name = theName;
    }
}

let add = new Octopus('Man with the 8 strong legs');

dad.name = 'Man with the 3-piece suit'; // error
```

### 클래스 속성 접근자

ECMAScript5는 속성 접근자 개념을 도입했습니다.
접근자는 클래스 사용자가 속성을 설정하거나 속성을 가져올 때 호출하는 간단한 함수입니다.
접근자로 클래스 사용자가 속성을 설정하고 읽을 때를 알 수 있고 다른 로직에 대한 시발점으로도 사용할 수 있습니다.
접근자를 사용하려면 `get`, `set` 키워드를 이용해 뒤에 같은 함수명이 오는 함수를 만들어야 합니다.
예제를 통해 살펴보겠습니다.

```typescript
class ClassWithAccessors {
    private _id: number;

    get id() {
        console.log('inside get id');
        return this._id;
    }

    set id(value: number) {
        console.log('inside set id');
        this._id = value;
    }
}

let classWithAccessors = new ClassWithAccessors();

classWithAccessors.id; // inside get id
classWithAccessors.id = 2; // inside set id
```

`get`, `set` 함수를 그냥 속성처럼 사용한다는 점에 주목하시기 바랍니다.

### 매개변수 프로퍼티(Parameter properties)

앞선 예제의 Octopus 클래스에서 readonly 멤버 name와 생성자 매개변수 theName을 선언했습니다.
그 다음 바로 name을 theName으로 설정했습니다.
이것은 매우 일반적인 방법입니다.
**매개변수 프로퍼티**를 사용하면 한 곳에서 멤버를 생성하고 초기화할 수 있습니다.

```typescript
class Octopus {
    readonly numberOfLegs: number = 8;

    constructor(readonly name: string) {}
}
```

생성자에서 `readonly name: string` 매개 변수를 사용해 멤버 name을 생성하고 초기화할 수 있습니다.
선언과 할당을 하나의 장소로 통합했습니다.
매개변수 프로퍼티는 접근 지정자 또는 `readonly` 또는 둘 모두로 생성자 매개변수를 접두어로 붙여 선언합니다.
매개변수 프로퍼티에 `private`을 사용하면 private 멤버가 선언되고 초기화됩니다.
`public`과 `protected`, `readonly`도 마찬가지입니다.

####

## 📍 정적 프로퍼티(Static properties)

지금까지는 인스턴스의 클래스 멤버들에 대해서만 설명했습니다.
인스턴스가 아닌 클래스 자체에 정적 멤버도 생성할 수 있습니다.
예제를 통해 살펴보겠습니다.

```typescript
class Grid {
    static origin = { x: 0, y: 0 };

    calculateDistanceFromOrigin(point: { x: number; y: number }) {
        let xDist = point.x - Grid.origin.x;
        let yDist = point.y - Grid.origin.y;

        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }

    constructor(public scale: number) {}
}

let grid1 = new Grid(1.0);
let grid2 = new Grid(5.0);

console.log(Grid.origin); // { x: 0, y: 0}
console.log(grid1.calculateDistanceFromOrigin({ x: 10, y: 10 }));
console.log(grid2.calculateDistanceFromOrigin({ x: 10, y: 10 }));
```

이 예제에서는 모든 grid의 일반적인 값이기 때문에 origin에 `state`을 사용합니다.
각 인스턴스는 클래스의 이름을 미리 정의하여 이 값에 접근합니다.
인스턴스의 접근자 앞에 this를 추가하는 것과 비슷하게 `state` 접근자 앞에 Grid를 추가해 호출할 수 있습니다.
정적 멤버는 클래스 인스턴스를 만들지 않고 호출 가능한 클래스 멤버입니다.
클래스명을 이름 앞에 붙여 호출해야 합니다.

####

## 👀 추상 클래스(Abstract Classes)

추상 클래스는 다른 클래스가 파생될 수 있는 기본 클래스입니다.
추상 클래스는 직접적으로 인스턴스화할 수 없습니다.
인터페이스와 달리 추상 클래스는 클래스 멤버에 대한 구현 세부 정보를 포함할 수 있습니다.
`abstract` 키워드는 추상 클래스뿐만 아니라 추상 클래스 내의 추상 메서드를 정의하는 데 사용됩니다.

```typescript
abstract class Animal {
    abstract makeSound(): void;

    move(): void {
        console.log('roaming the earth...');
    }
}
```

`abstract`으로 표시된 추상 클래스 내의 메서드는 구현을 포함하지 않으므로 파생된 클래스에서 구현해야 합니다.
추상 메서드는 인터페이스 메서드와 유사한 구문을 사용합니다.
둘 다 메서드 본문을 포함하지 않고 메서드를 정의합니다.
그러나 추상 메서드는 `abstract` 키워드를 사용해야 하며 선택적으로 접근 지정자를 포함할 수 있습니다.

```typescript
abstract class Department {
    constructor(public name: string) {}

    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting(): void;
}

class AccountingDepartment extends Department {
    constructor() {
        super('Accounting and Auditing');
    }

    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }

    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}

let department: Department; // good! abstract 타입에 대한 참조를 만듭니다.
department = new Department(); // error! 추상 클래스의 인스턴스를 생성할 수 없습니다.
department = new AccountingDepartment(); // good! 추상적이지 않은 하위 클래스를 생성하고 할당합니다.

department.printName(); // Department name: Accounting and Auditing
department.printMeeting(); // The Accounting Department meets each Monday at 10am.
department.generateReports(); // error: abstract 타입으로 선언된 메서드가 존재하지 않습니다.
```
