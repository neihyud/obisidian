---
title: Typescript
tags: [Web, Framework]

---

###### tags: `Web` `Framework`
# Typescript

# **File Config**

Basic TypeScript
● **noImplicitAny**
`Khi typescript không thể xác định được kiểu dữ liệu của một biến nào đó, thì biến đó sẽ được mang
kiểu dữ liệu any.Khi bật lên true, nó sẽ báo lỗi nếu trường hợp này xảy ra, ngược lại bỏ qua lỗi này.`
● **Exclude**: ["core.ts","sys.ts","types.ts","scanner.ts","parser.ts"]
Định nghĩa những thư mục, những files mà sẽ bị loại ra khi biên dịch.
● "**strictPropertyInitialization**": true
Đảm bảo các thuộc tính của class phải được khởi tạo.
● "**noImplicitThis**": true
Đảm bảo “this” không mang kiểu dữ liệu any
● "**outDir**": "./dist"
Cấu hình đường dẫn lưu trữ các file js sau khi được biên dịch.
● **target**: “es5”
Sử dụng phiên bản ECMAScript 2009 (ES6: ECMAScript 2015). Bạn có thể thay đổi nếu muốn dùng ES6 để biên dịch.
● **module**: “commonjs”
Chỉ ra cách để các modules được load lên khi project hoạt động. Ở đây sử dụng commonjs. Ngoài ra còn các patterns: CommonJS, AMD, UMD, System
● “**sourceMap**”: true
Cho phép tạo ra file *.map tương ứng.
● “**strict**”: true
Bật strict=true sẽ bật tất cả options type-checking


# **Simple Types**

- **Primitive** (tham trị): number, boolean, string, undefined, null, bigint, symbol
- **Reference** (tham chiếu): object, function
- 3 kiểu nguyên thủy
    - boolean: true or false 
    - number: float and number
    - string: text
- 2 kiểu gán: implicit(ngầm hiểu, typescript tự đoán), explicit (rõ ràng)
- Cũng có kiểu không thể suy luận: lúc này type data là `any`. Hành vi này có thể vô hiệu hóa bằng cách bật `noImplicitAny` trong file `tsconfig.json`

# **Special Types**

## **Type: any**

- là kiểu vô hiệu kiểm tra kiểu và cho phép tất cả các kiểu hoạt động
- Không giống như unknown, các biến kiểu any cho phép bạn truy cập các thuộc tính tùy ý, ngay cả những thuộc tính không tồn tại.

## **Type: unknown**

- Tương tự như `any` n nó an toàn hơn
- Được sử dụng khi không biết kiểu dữ liệu được nhập
> e.g let w: unknow = 1
> w= "string" // no error

- Giống như type any, type unknown có thể assign bất kỳ value nào.
- Nếu như type any cho phép thực hiện bất kỳ operation nào mà không check type thì unknown type lại gần như không cho phép thực hiện operation nào.
- Chúng ta có thể sử dụng type-checking để có thể thực hiện các operation trên unknown type
- Khác nhau giữa any và unknown:
    - Cũng khá tương tự với **unknown** khi chúng ta không biết trước được kiểu dữ liệu, tuy nhiên với any chúng ta có thể truy cập vào thuộc tính mặc dù không tồn tại của nó, còn **unknown** thì sẽ thông báo lỗi khi truy cập vào attr không tồn tại
    - Các biến type **unknown** không thể assign giá trị cho biến kiểu khác, ngược lại các biến type any có thể được sử dụng để assign giá trị cho các kiểu biến khác.


## **Type: undefined & null**

- k được sử dụng nhiều trừ khi bật `stricNullChecks` trong file `tsconfig.json`

# **Arrays**

- **Syntax**: :type_data[] = ...
- `Readonly`: ngăn cản sự thay đổi của mảng
```javascript=
const names: readonly string[] = ["Dylan"];
names.push("Jack"); 
// Error: Property 'push' does not exist on type 'readonly string[]'
// try removing the readonly modifier and see if it works?
```



- Type Inference (kiểu suy luận): sẽ suy luận là kiểu mảng nếu **nó có giá trị**

# **TypeScript Tuples**

- Một `Tuple` là một kiểu mảng với một độ dài và kiểu của mỗi index được định nghĩa trước. Chỉ cần khởi tạo đúng kiểu cho các giá trị ban đầu của kiểu. 

```javascript=
e.g // define our tuple
let ourTuple: [number, boolean, string];
// initialize correctly
ourTuple = [5, false, 'Coding God was here'];
```

- Readonly Tuple
    - Chỉ có các loại được xác định rõ ràng cho các giá trị ban đầu
    - k thể thêm vào giá trị mới vào ( vì có thuộc tính `readonly`)

```javascript=
// define our readonly tuple
const ourReadonlyTuple: readonly [number, boolean, string] = [5, true, 'The Real Coding God'];
// throws error as it is readonly.
ourReadonlyTuple.push('Coding God took a day off');
```
>

- Named Tuples
    - Cung cấp context giá trị cho mỗi chỉ mục
> const graph: [x: number, y: number] = [55.2, 41.3];

- Destructuring Tuples
> const graph: [number, number] = [55.2, 41.3];
const [x, y] = graph;

# **TypeScript Object Types**

- Type Inference: tự suy luận
```javascript=
const car = {
  type: "Toyota",
};
car.type = "Ford"; // no error
car.type = 2; // Error: Type 'number' is not assignable to type 'string'.
```

- Optional Properties: thuộc tính tùy chọn
    - Nếu không có tùy chọn
```javascript=
 const car: { type: string, mileage: number } = { 
    // Error: Property 'mileage' is missing in type '{ type: string; }' 
    // but required in type '{ type: string; mileage: number; }'.
      type: "Toyota",
    };
    car.mileage = 2000;
```
------- Nếu có tùy chọn (?:)
```javascript=
const car: { type: string, mileage?: number } = { // no error
  type: "Toyota"
};
car.mileage = 2000;
```

# **Types Enum**

- Một enum type đại diện cho một group constants
- Có hai kiểu giá trị cơ bản là: string và numberic
```javascript=
enum CardinalDirections {
    North,
    East,
    West,
    South
}

- Không khởi tạo giá trị gì thì nó mặc định là 0, nếu khởi tạo 
giá trị đầu tiên là 1 thì nó sẽ tự động tăng dần
- Nếu gán giá trị thì dùng n obj
- Có thể trộn số và chữ n k nên làm vậy
```

# **TypeScript type Aliases and Interface**

- Type có Unions type còn interface thì không
- Type k thể add properties còn interface thì có thể
## **Aliases (Type)** 

- Định nghĩa kiểu với một tên tùy chỉnh
- Có thể sử dụng kiểu nguyên thủy hoặc phức tạp (obj, array)

```javascript=
type CarYear = number (định nghĩa kiểu CarYear có kiểu là string)
type CarType = string
type CarModel = string
type Car = {
  year: CarYear,
  type: CarType,
  model: CarModel
}
```

## **Interfaces**

- Tương tự Aliases, nhưng chỉ cho phép kiểu obj

```javascript=
interface Rectangle {
  height: number,
  width: number
}

const rectangle: Rectangle = {
  height: 20,
  width: 10
};
```
## **Extending Interfaces**
- Có thể kế thừa interfaces

```javascript=
interface Rectangle {
  height: number,
  width: number
}

interface ColoredRectangle extends Rectangle {
  color: string
}

const coloredRectangle: ColoredRectangle = {
  height: 20,
  width: 10,
  color: "red"
};
```

# **TypeScript Union Types**

- Sử dụng khi một giá trị có nhiều kiểu
- Union: |
```javascript=
const code: string | number 
=> code có kiểu string hoặc number
```

# TypeScript Functions

- Return type: Loại giá trị được trả về bởi hàm có thể được xác định rõ ràng.
```javascript=
function getTime(): number {
  return new Date().getTime();
}
=> return type number
- nếu không có giá trị nào trả về thì nó sẽ cố gắng suy luận dựa vào biến hoặc biểu thức trả về
```
- Void return type: k trả về giá trị nào
- Tham số:
```javascript=
function multiply(a: number, b: number) {
  return a * b;
}
=> nếu không định nghĩa thì nó mặc định là kiểu any, trừ khi có thông tin bổ sung Default Parameters, Type
```
## **Optional Parameters(?)**
- Mặc định param là required, nếu sử dụng ? thì có thể bỏ qua
```javascript=
// the `?` operator here marks parameter `c` as optional
function add(a: number, b: number, c?: number) {
  return a + b + (c || 0);
}
```
## **Default Parameter**
```javascript=
function pow(value: number, exponent: number = 10) {
  return value ** exponent;
}
```

## **Named Parameters**
- Nhập tên tham số và định nghĩa kiểu
```javascript=
function divide({ dividend, divisor }: { dividend: number, divisor: number }) {
  return dividend / divisor;
}
```
## **Rest Parameters**
- Rest Parameters: trả về một mảng
```javascript=
function add(a: number, b: number, ...rest: number[]) {
  return a + b + rest.reduce((p, c) => p + c, 0);
}
```
## **Type Alias**
```javascript=
type Negate = (value: number) => number;

// in this function, the parameter `value` automatically gets assigned 
// the type `number` from the type `Negate`
const negateFunction: Negate = (value) => value * -1;
```

# TypeScript Casting

- Ghi đè một kiểu
## **Casting with `as`**
```javascript=
let x: unknown = 'hello';
console.log((x as string).length);

=> nếu x: unknown = 4 thì nó sẽ hiển thị lỗi (vì number k có length)
=> có thể sử dụng The Force casting để có thể ghi đè kiểu này
```
## **Casting with <>**
- Tương tự as, n k sử dụng với TSX(vd khi làm việc với file React)
```javascript=
let x: unknown = 'hello';
console.log((<string>x).length);
```
## **Force casting**
- To override type errors that TypeScript may throw when casting, first cast to unknown, then to the target type.
```javascript=
let x = 'hello';
console.log(((x as unknown) as number).length); 
// x is not actually a number so this will return undefined
```

# **TypeScript Classes**

## **Member Type**
- Member in class (properties, method) sử dụng type chú thích
```javascript=
class Person {
  name: string;
}

const person = new Person();
person.name = "Jane";
```
## **Readonly**    

- Ngăn các member trong class thay đổi
```javascript=
class Person {
  private readonly name: string;

  public constructor(name: string) {
    // name cannot be changed after this initial definition, which has to be either at it's declaration or in the constructor.
    this.name = name;
  }

  public getName(): string {
    return this.name;
  }
}

const person = new Person("Jane");
console.log(person.getName());
```

## **Inheritance: Implements**
- Định nghĩa kiểu class 
- Có thể inplement nhiều interface

```javascript=
interface Shape {
  getArea: () => number;
}

class Rectangle implements Shape {
  public constructor(protected readonly width: number, protected readonly height: number) {}

  public getArea(): number {
    return this.width * this.height;
  }
}
```

## **Inheritance: Extends**

- Có thể kế thừa một class
```javascript=
interface Shape {
  getArea: () => number;
}

class Rectangle implements Shape {
  public constructor(protected readonly width: number, protected readonly height: number) {}

  public getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  public constructor(width: number) {
    super(width, width);
  }

  // getArea gets inherited from Rectangle
}
```

# **Basic Generics**

- Cho phép tạo 'type variables', có thể là class, func, type aliases mà k cần phải định nghĩa rõ ràng

## **Functions**
```javascript=
function createPair<S, T>(v1: S, v2: T): [S, T] {
  return [v1, v2];
}
console.log(createPair<string, number>('hello', 42)); // ['hello'class NamedValue<T> {
  private _value: T | undefined;

  constructor(private name: string) {}

  public setValue(value: T) {
    this._value = value;
  }

  public getValue(): T | undefined {
    return this._value;
  }

  public toString(): string {
    return `${this.name}: ${this._value}`;
  }
}

let value = new NamedValue<number>('myNumber');
value.setValue(10);
console.log(value.toString()); // myNumber: 10, 42]
```

## **Classes**
```javascript=
class NamedValue<T> {
  private _value: T | undefined;

  constructor(private name: string) {}

  public setValue(value: T) {
    this._value = value;
  }

  public getValue(): T | undefined {
    return this._value;
  }

  public toString(): string {
    return `${this.name}: ${this._value}`;
  }
}

let value = new NamedValue<number>('myNumber');
value.setValue(10);
console.log(value.toString()); // myNumber: 10
```

## **Type Aliases && Interface**

- 
```javascript=
type Wrapped<T> = { value: T };

const wrappedValue: Wrapped<number> = { value: 10 };

- Interface cũng tương tự
```

## **Ràng buộc Genetic Type**

### **Ràng buộc với `Extends`**

```javascript=
function merge<U, V>(obj1: U, obj2: V) {
    return {
        ...obj1,
        ...obj2
    };
}

let person = merge(
    { name: 'John' },
    { age: 25 }
);

console.log(result); //{ name: 'John', age: 25 }
-------------------------------------------------------
- Tuy nhiên nếu truyền như thế này thì nó vẫn chạy

let person = merge(
    { name: 'John' },
    25
);

console.log(person); //{ name: 'John' }

-------------------------------------------------------
- Để ràng buộc hàm truyền vào là một obj, ta làm như sau

function merge<U extends object, V extends object>(obj1: U, obj2: V) {
    return {
        ...obj1,
        ...obj2
    };
}
```
### **Ràng buộc với `extends keyof`** 
```javascript=
function prop<T, K>(obj: T, key: K) {
    return obj[key];
}
=> Type 'K' cannot be used to index type 'T'.
-------------------
- Sử dụng `extend keyof`
function prop<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}
```

# **TypeScript Utility Types**

## Partial
- Thay đổi một phần properties trong một obj là tùy chọn
```javascript=
interface Point {
  x: number;
  y: number;
}

let pointPart: Partial<Point> = {}; // `Partial` allows x and y to be optional
pointPart.x = 10; 
console.log(pointPart); // { x: 10 }
```

## **Required**
- Các properties là một obj được yêu cầu
```javascript=
interface Car {
  make: string;
  model: string;
  mileage?: number;
}

let myCar: Required<Car> = {
  make: 'Ford',
  model: 'Focus',
  mileage: 12000 // `Required` forces mileage to be defined
};
```

## **Record**
- Định nghĩa kiểu obj là một cặp key-value
```javascript=
const nameAgeMap: Record<string, number> = {
  'Alice': 21,
  'Bob': 25
};

=> Record<string, number> is equivalent to { [key: string]: number }
```
## **Omit**
- Remove keys từ một kiểu obj
```javascript=
interface Person {
  name: string;
  age: number;
  location?: string;
}

const bob: Omit<Person, 'age' | 'location'> = {
  name: 'Bob'
  // `Omit` has removed age and location from the type and they can't be defined here
};
```

## Pick
- Ngược lại với Omit
- Pick removes all but the specified keys from an object type.
```javascript=
interface Person {
  name: string;
  age: number;
  location?: string;
}

const bob: Pick<Person, 'name'> = {
  name: 'Bob'
  // `Pick` has only kept name, so age and location were removed from the type and they can't be defined here
};
```

## Exclude
- Remove kiểu từ một union
```javascript=
type Primitive = string | number | boolean
const value: Exclude<Primitive, string> = true; // a string cannot be used here since Exclude removed it from the type.
```

## Parameters
- extracts các kiểu tham số của một type func như một array
```javascript=
type PointPrinter = (p: { x: number; y: number; }) => void;
const point: Parameters<PointPrinter>[0] = {
  x: 10,
  y: 20
};
```

# Keyof
- Sử dụng để extract key type từ một obj type
## **`keyof` with explicit keys**
- When used on an object type with explicit keys, keyof creates a union type with those keys.
```javascript=
interface Person {
  name: string;
  age: number;
}
// `keyof Person` here creates a union type of "name" and "age", other strings will not be allowed
function printPersonProperty(person: Person, property: keyof Person) {
  console.log(`Printing person property ${property}: "${person[property]}"`);
}
let person = {
  name: "Max",
  age: 27
};
printPersonProperty(person, "name"); // Printing person property name: "Max"
```

## **`keyof` with index signatures**
- keyof can also be used with index signatures to extract the index typ
```javascript!
type StringMap = { [key: string]: unknown };
// `keyof StringMap` resolves to `string` here
function createStringPair(property: keyof StringMap, value: string): StringMap {
  return { [property]: value };
}
```
# Null
## Option Chaining
- `?.`: có thể tồn tại hoặc không, thường sử dụng khi truy xuất properties
## Nullish Coalescence
- `??`: nếu là null hoặc undefined