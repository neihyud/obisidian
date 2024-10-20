# Inversion of Control (IoC)
- IoC là một đối tượng không tự quản lý luồng điều khiển của mình mà giao nó cho một thành phần bên ngoài
- Trước khi có **IoC**
```java
class Engine {
  start() {
    console.log('Engine started');
  }
}

class Car {
  private engine: Engine;

  constructor() {
    // Car tự tạo đối tượng Engine
    this.engine = new Engine();
  }

  start() {
    this.engine.start();
  }
}

const car = new Car();
car.start();  // "Engine started"


=> Car phụ thuộc vào Engine, khi Engine thay đổi cách tạo thì Car cũng phải thay đổi theo

```

- Sau khi sử dụng **IoC**
```java
class Engine {
  start() {
    console.log('Engine started');
  }
}

class Car {
  private engine: Engine;

  // Engine được truyền vào qua constructor, không tự khởi tạo
  constructor(engine: Engine) {
    this.engine = engine;
  }

  start() {
    this.engine.start();
  }
}

// Bên ngoài quản lý việc khởi tạo và cung cấp đối tượng Engine
const engine = new Engine();
const car = new Car(engine); // Engine được cung cấp từ bên ngoài
car.start();  // "Engine started"

```
# Dependency Injection (DI)
- cơ chế inject các phụ thuộc từ bên ngoài vào class, thay vì class tự quản lý.

```java
class Engine {
  start() {
    console.log('Engine started');
  }
}

class Car {
  private engine: Engine;

  // Inject Engine qua setter thay vì constructor
  setEngine(engine: Engine) {
    this.engine = engine;
  }

  start() {
    this.engine.start();
  }
}

// Bên ngoài quản lý việc cung cấp đối tượng Engine
const engine = new Engine();
const car = new Car();
car.setEngine(engine);  // Engine được inject qua setter
car.start();  // "Engine started"

```

# IoC Controller
- Quản lý các lớp phụ thuộc của chúng
```java
class IoCContainer {
  private dependencies = new Map();

  // Đăng ký một class vào container
  register(className: string, dependency: any) {
    this.dependencies.set(className, dependency);
  }

  // Lấy ra một instance của class đã đăng ký
  resolve<T>(className: string): T {
    const dep = this.dependencies.get(className);
    if (!dep) {
      throw new Error(`Dependency ${className} not found`);
    }
    return dep;
  }
}

// Các class cần quản lý
class Engine {
  start() {
    console.log('Engine started');
  }
}

class Car {
  private engine: Engine;

  constructor(engine: Engine) {
    this.engine = engine;
  }

  start() {
    this.engine.start();
  }
}

// Tạo IoC container
const container = new IoCContainer();

// Đăng ký các lớp và phụ thuộc của chúng vào container
container.register('engine', new Engine());
container.register('car', new Car(container.resolve<Engine>('engine')));

// Lấy Car từ container và sử dụng
const car = container.resolve<Car>('car');
car.start();  // "Engine started"

```