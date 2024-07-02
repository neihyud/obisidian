---
title: 'C#'
tags: [Programming, Other]

---

###### tags: `Programming` `Other` 

- Naming Guideline: https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines
- to avoid error, use tryparse to convert type
- prive is defautl

![image](https://hackmd.io/_uploads/S1MU24wNa.png)

- Use properties instead of fields when you want to
validate what value can be stored, when you want to data bind in
XAML which we will cover in Chapter 20, Building Windows Desktop
Apps, and when you want to read and write to fields without using
methods.
- cùng assembly (cùng project)


## Trace And Debug
![image](https://hackmd.io/_uploads/HklRXDU4T.png)

- **Debug.WriteLine**: để write console, nó sẽ bị xóa khi lên release 
- appsettings file 
:::warning
- dotnet add package Microsoft.Extensions.Configuration
- dotnet add package Microsoft.Extensions.Configuration.Binder
- dotnet add package Microsoft.Extensions.Configuration.Json
- dotnet add package Microsoft.Extensions.Configuration. FileExtensions
- Add a file named appsettings.json
     {"PacktSwitch": { "Level": "Info" }}
- In Program.cs, import the Microsoft.Extensions.Configuration namespace.

https://github.com/neihyud/csharp_modern_cross_book/tree/master/DebugAndTrace/Instrumenting
:::
## Managing memory
- Stack memory is faster to work with (because it is managed directly by the CPU
and because it uses a first-in, first-out mechanism, it is more likely to have the data
in its L1 or L2 cache) but limited in size, while heap memory is slower but much
more plentiful
https://adamsitnik.com/Value-Types-vs-Reference-Types/

## Inheritance
- sealed: ngăn chặn kế thừa


# Chapper 7: Understanding .NET Core components
- .NET core:
    - Language compilers: chuyển đổi source viết bằng C#, F# thành IL (Intermediate Language) được lưu trữ trong **Assemblies**.
    - Common Language Runtime (CoreCLR): This runtime loads assemblies,
compiles the IL code stored in them into native code instructions for your
computer's CPU, and executes the code within an environment that manages
resources such as threads and memory.
    - Base Class Libraries (BCL) of assemblies in NuGet packages (CoreFX): These are prebuilt assemblies of types packaged and distributed using NuGet
for performing common tasks when building applications.

- An **assembly** is where a type is stored in the filesystem. **Assemblies** are a mechanism for deploying code.
- A namespace is the address of a type. Namespaces are a mechanism to uniquely identify a type by requiring a full address rather than just a short name

## Understanding dependent assemblies
- Nếu một assembly được biên dịch như một thư viện lớp và cung cấp các loại dữ liệu cho các assembly khác sử dụng, thì nó có đuôi tệp là .dll (dynamic link library - thư viện liên kết động), và không thể chạy độc lập.



## Dotnet CLI

![image](https://hackmd.io/_uploads/HyfDGXfBp.png)

![image](https://hackmd.io/_uploads/H1G2vXzSa.png)

# Regex

![image](https://hackmd.io/_uploads/rkLqFQfH6.png)
![image](https://hackmd.io/_uploads/BJ_2YQGHp.png)

https://www.regular-expressions.info/unicode.html

# Collections 

![image](https://hackmd.io/_uploads/S1XuqXfBT.png)	
	
# Authenticating and authorizing users

![image](https://hackmd.io/_uploads/H11DINMHp.png)

- exercise: ... page 391

# Chaper 11: EF CORE
page 329

## Loading patterns with EF core
??

## Chapter 13
- page 413
- Avoid using the String.Concat method or the + operator inside loops. Use StringBuilder instead.

# C#

- **decimal**: **M** suffix means a decimal literal value

:::info
- Use **int** for whole numbers and **double** for real numbers that will not be compared to other values. Use **decimal** for money, CAD drawings, general engineering, and wherever accuracy of a real number is important.
- Never compare **double** values using ==, because have error
:::

- **using static System.Console**;

## .NET Framework
- .NET Framework is a development platform that includes a Common Language Runtime (CLR), which manages the execution of code, and a Base Class Library (BCL), which provides a rich library of classes to build applications from
- ![image](https://hackmd.io/_uploads/BJNA5vG4a.png)


# Architech

![image](https://hackmd.io/_uploads/SkQ8yVaX6.png)

:::info
1. **CLR** (Common Language Runtime): It is a run-time environment that executes the code written in any .NET programming language.
2. **FCL** (Framework Class Library) : A large number of class libraries
3. **Types of Applications**
    - **WinForms**: for window
    - **ASP.NET**: for web
    - **ADO.NET**: includes the applications that are developed to communicate with the database like MS SQL Server, Oracle, etc. It mainly consists of classes that can be used to connect, retrieve, insert, and delete data.
4. **WPF** (Windows Presentation Foundation): Windows Presentation Foundation (WPF) is a graphical subsystem given by Microsoft that uses DirectX and is used in Windows-based applications for rendering UI (User Interface). WPF was initially released as part of .NET Framework 3.0 in 2006 and was previously known as “Avalon”.
5. **WCF** (Windows Communication Foundation): It is a framework for building connected and service-oriented applications used to transmit data asynchronously from one service endpoint to another service point. It was previously known as the Indigo.
6. **WF** (Windows Workflow Foundation): It is a technology given by Microsoft that provides a platform for building workflows within .Net applications.
7. **Card Space**: It is a Microsoft .NET Framework software client that is designed to let users provide their digital identity to online services in a secure, simple, and trusted way.
8. **LINQ** (Language Integrated Query): 
9. **Entity Framewor**
10. Parallel LINQ (Language Integrated Query):
11. TPL (Task Parallel Library)
12. .NET API For Store/UWP Apps:
13. Task-Based Asynchronous Model:
:::

- C# programs run on .NET, a virtual execution system called the common language runtime (CLR) and a set of class libraries
- C# run .NET (CLR và FCL)
- Source  C# is compiled into an intermediate language (IL) that conforms to the CLI specification. 
    - **IL** code and resources, such as bitmaps and strings, are stored in an **assembly**, typically with an extension of **.dll**.
    - An **assembly** contains a manifest that provides information about the assembly's types, version, and culture.
    - C# program is executed, the **assembly** is loaded into the CLR. CLR performs Just-In-Time (JIT) compilation to convert the IL code to native machine instructions
    - IL code produced by the C# compiler conforms to the Common Type Specification (CTS)

## Common Language Runtime (CLR)
-  provides essential runtime services: automatic memory management and exception handling
-   CLR is responsible for managing the execution of code written in any of the .NET-supported languages. When an application is run, the CLR loads the required libraries and compiles the code into machine code that can be executed by the computer’s processor. The CLR also provides a number of services, such as automatic memory management and security, that help ensure that applications are reliable and secure. 
-   The CLR provides other services related to automatic garbage collection, exception handling, and resource management. Code that's executed by the CLR is sometimes referred to as "managed code." "Unmanaged code," is compiled into native machine language that targets a specific platform.
## Framework Class Library(FCL)
-  It is the collection of reusable, object-oriented class libraries and methods, etc that can be integrated with CLR

## Name Space
- Namespaces provide a hierarchical means of organizing C# programs and libraries. Namespaces contain types and other namespaces

# Program building blocks
https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/program-building-blocks#members
## Member
- The members of a class are either static members or instance members. Static members belong to classes, and instance members belong to objects (instances of classes).
- **Constants**: Constant values associated with the class
- **Fields**: Variables that are associated with the class
- **Methods**: Actions that can be performed by the class
- **Properties**: Actions associated with reading and writing named properties of the class
- **Indexers**: Actions associated with indexing instances of the class like an array
- **Events**: Notifications that can be generated by the class
- **Operators**: Conversions and expression operators supported by the class
- **Constructors**: Actions required to initialize instances of the class or the class itself
- **Finalizers**: Actions done before instances of the class are permanently discarded
- **Types**: Nested types declared by the class
## Accessibility
- public: Access isn't limited.
- private: Access is limited to this class.
- protected: Access is limited to this class or classes derived from this class.
- internal: Access is limited to the current assembly (.exe or .dll).
- protected internal: Access is limited to this class, classes derived from this class, or classes within the same assembly.
- private protected: Access is limited to this class or classes derived from this type within the same assembly.

## Field
- A field is a variable that is associated with a class or with an instance of a class.
- A field declared with the static modifier defines a static field. A static field identifies exactly one storage location. No matter how many instances of a class are created, there's only ever one copy of a static field.
- A field declared without the static modifier defines an instance field. Every instance of a class contains a separate copy of all the instance fields of that class.
## Methods
- A method is a member that implements a computation or action that can be performed by an object or class. Static methods are accessed through the class. Instance methods are accessed through instances of the class.
- The **signature** of a method must be unique in the class in which the method is declared. The signature of a method consists of the name of the method, the number of type parameters, and the number, modifiers, and types of its parameters. The **signature** of a method doesn't include the return type.
## Parameters
- Parameters are used to pass values or variable references to methods. The parameters of a method get their actual values from the arguments that are specified when the method is invoked. There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.
- A **reference parameter** (**ref**) is used for passing arguments by reference. The argument passed for a reference parameter must be a variable with a definite value. During execution of the method, the reference parameter represents the same storage location as the argument variable
- An **output parameter** is used for passing arguments by reference.
- A **parameter array** permits a variable number of arguments to be passed to a method. A parameter array is declared with the params modifier. Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type

## Static and instance method
- A method declared with a static modifier is a static method. A static method doesn't operate on a specific instance and can only directly access static members.
- A method declared without a static modifier is an instance method. An instance method operates on a specific instance and can access both static and instance members. The instance on which an instance method was invoked can be explicitly accessed as this. It's an error to refer to this in a static method.
### Virtual, override, and abstract methods
- A **virtual** method is one declared and implemented in a base class where any derived class may provide a more specific implementation (đánh dấu có thể ghi đè bởi method in subclass)
-  An **override** method is a method implemented in a derived class that modifies the behavior of the base class' implementation.
-  An **abstract** method is a method declared in a base class that must be overridden in all derived classes
- The type of a variable determines its **compile-time type**. The compile-time type is the type the compiler uses to determine its members
-  The **run-time type** is the type of the actual instance a variable refers to.
- **Virtual method** is invoked, the **run-time type** of the instance for which that invocation takes place determines the actual method implementation to invoke. In a nonvirtual method invocation, the compile-time type of the instance is the determining factor.
- A **virtual method** can be overridden in a derived class
- An **abstract method** is a **virtual method** with no implementation.

### Method overloading
- Method overloading permits multiple methods in the same class to have the same name as long as they have unique signatures
- When compiling an invocation of an overloaded method, the compiler uses **overload resolution** to determine the specific method to invoke

## Constructors
- An **instance constructor** is a member that implements the actions required to initialize an instance of a class. 
- A **static constructor** is a member that implements the actions required to initialize a class itself when it's first loaded.
## Properties
- **Properties** are a natural extension of fields.
- Both are named members with associated types, and the syntax for accessing fields and properties is the same. However, unlike fields, properties don't denote storage locations. Instead, properties have accessors that specify the statements executed when their values are read or written. A get accessor reads the value. A set accessor writes the value.
- A property is declared like a field, except that the declaration ends with a get accessor or a set accessor written between the delimiters { and } instead of ending in a semicolon.
-  A property that has both a get accessor and a set accessor is a read-write property. A property that has only a get accessor is a read-only property. A property that has only a set accessor is a write-only property.
## Indexers
- An indexer is a member that enables objects to be indexed in the same way as an array
## Statement
- The **checked** and **unchecked** statements are used to control the overflow-checking context for integral-type arithmetic operations and conversions.
- **lock**: tại một thời điểm chỉ có một thread thực hiện
- The **using** statement is used to obtain a resource, execute a statement, and then dispose of that resource.

# Fundamentals
## Program structure
- There can only be one entry point in a C# program. If you have more than one class that has a Main method, you must compile your program with the StartupObject compiler option to specify which Main method to use as the entry point. For more information, see **[StartupObject (C# Compiler Options)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/compiler-options/advanced#mainentrypoint-or-startupobject)**.
- The Main method is the entry point of an executable program; it is where the program control starts and ends.
## Type System
- Namespaces are heavily used in C# programming in two ways. 
    - First, .NET uses namespaces to organize its many classes, as follows:
    - Second, declaring your own namespaces can help you control the scope of class and method names in larger programming projects.

## Object-oriented programming
- classes: reference types, a variable of a class object holds a reference to the address of the object on the managed heap. If a second variable of the same type is assigned to the first variable, then both variables refer to the object at that address. 
- structs: value types, a variable of a struct object holds a copy of the entire object
- Để xác định liệu hai thể hiện của class có trỏ đến cùng một vị trí trong bộ nhớ (điều này có nghĩa là chúng có cùng một định danh), sử dụng phương thức tĩnh Object.Equals. (System.Object là lớp cơ sở ngầm định cho tất cả các kiểu giá trị và kiểu tham chiếu, bao gồm cả các struct và class do người dùng định nghĩa.)
- Để xác định liệu các trường thể hiện trong hai thể hiện của struct có giá trị giống nhau hay không, sử dụng phương thức ValueType.Equals. Vì tất cả các struct đều kế thừa ngầm từ System.ValueType, bạn gọi phương thức trực tiếp trên đối tượng của mình như thể hiện trong ví dụ sau:
- **sealed**: prevent inheriting

# ASP.NET
- 1 solution có một hoặc nhiều project
- **launchSettings.json**: run in local machine
- **appsettings.json**: run in production, save config db

- **UseDeveloperExceptionPage**: config exception page display in server

## Understanding WebApplicationOptions Class in ASP.NET Core:
- Using WebApplicationOptions Class, we can set the EnvironmentName, ApplicationName, WebRootPath, ContentRootPath, etc.
 ![image](https://hackmd.io/_uploads/SJwVw3l8p.png)

## Dotnet CLI

- `dotnet new list`: -> display list template c#
- `dotnet new {template}`: 
- `dotnet new {template} -n {name project} -o {path_contain_project}`
- `dotnet add package {package}`
- `dotnet remove package {package}`
- `dot net run`

## Setup MVC ASP-NET-CORE
- https://dotnettutorials.net/lesson/setup-mvc-asp-net-core-application/
- UseRouting():  It matches a request to an endpoint.
- UseEndpoints(): It executes the matched endpoint.