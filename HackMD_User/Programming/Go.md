- [[https://go.dev/ref/mod#authenticating]]
- **go.mod**:  tracks dependencies - code imports packages contained in other modules
- `go get`: add a dependencies, upgrade or downgrade dependencies
- `go mod init <command>`: create **go.mod** file
- **package**: group functions - create by all files in the same directory
- **fmt package**: contain funs formating text
- `go mod tidy`: to update dependencies -> create new `go.sum` to authenticating the module
- `pkg.go.dev` site to find published module

**Introduction**
 - **Package**
	 - program run package main first
	- used to orgaized related code, 
	- A package is a collection of source files in the same directory that are compiled together.
	- 2 type of package: 
		- `excutable`: packages are used to create a file, can run out computer 
		- `dependency`: packages defined function named 'main', automatically call when run
- **Module**: 
	- collection of packages realeased, versioned and distributed
	- collection of related Go packages that are released together
	- indentified by a [[module path]]
- **Module path:** 
	- the canonical name for a module, declared `go.mod file`
		- prefix for package paths within the module
- **Import path:**
	- string used to import a package
- **go.mod**: declare **module path** - import path prefix for all packages within the module
- version: 
	- immutable snapshot of a module
	- include: major, minor, path version (v0.2.1)
		- incremented major: for example - package is removed
		- incremented minor: ex - add new function
		- incremented minor: ex - fix bug, optimization
-
- `nil` : meaning no error, the zero-value for pointers, interfaces, and some other types
**Compile and install the application**
 1. `go build` (ex: file - **hello**)
 2. Add the Go install directory to your system's shell path.
	- ` go list -f '{{.Target}}'`: get where go install in current package (~path to your install directory)
	- `export PATH=$PATH:/path/to/your/install/directory`
		- or  `go env -w GOBIN=/path/to/your/bin`
3. `go install`
4. Run file build (ex: **hello**)
		
**Create module**
![[Pasted image 20240709175205.png]]
![[Pasted image 20240709215532.png]]
- The [`go build` command](https://go.dev/cmd/go/#hdr-Compile_packages_and_dependencies) compiles the packages, along with their dependencies, but it doesn't install the results.
- The [`go install` command](https://go.dev/ref/mod#go-install) compiles and installs the packages.

###### Multi-module workspaces
- easily build and run code in those modules
- `go work init <folder>`: create new **`go.work`** file -  workspace
	- `go work use <path>`: add module to `go.work` file
- `go work use [-r] [dir]` adds a `use` directive to the `go.work` file for `dir`, if it exists, and removes the `use` directory if the argument directory doesn’t exist. The `-r` flag examines subdirectories of `dir` recursively.
- `go work edit` edits the `go.work` file similarly to `go mod edit`
- `go work sync` syncs dependencies from the workspace’s build list into each of the workspace modules.
###### Exported names
- a name **exported** if it begins with a capitai letter
- any "**unexported**" names are not accessible from **outside package**

**Type**
```go=
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point
float32 float64

complex64 complex128
```
###### defer
- defer execution until surrounding function returns
**stack defer**: 
```
fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
=> first in last out: couting done 9 -> 0
```
###### Pointer
- A pointer holds the memory address of a value
- **& operator**: generates a pointer to its operand
```go
var p *int
i:= 42
p = &i

fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```
**Structs**: 
- collection of fields
```
type Vertex struct {
	X int
	Y int
}
```

- golang: 
	- The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.
```
s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s) - len=6 cap=6 [2 3 5 7 11 13]

	// Slice the slice to give it zero length.
	s = s[:0] 
	printSlice(s) - len=0 cap=6 []

	// Extend its length.
	s = s[:4]
	printSlice(s) -len=4 cap=6 [2 3 5 7]

	// Drop its first two values.
	s = s[2:]
	printSlice(s) - len=2 cap=4 [5 7]
```
zero value of slice = `nil`
**Slice**
```
func make([]T, len, cap) []T

--- make allocate an array and returns refer to that array

var s []byte
s = make([]byte, 5, 5)
// s == []byte{0, 0, 0, 0, 0}
```
- zero value of slice = `nil`
- slice no 