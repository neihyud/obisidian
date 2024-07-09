- [[https://go.dev/ref/mod#authenticating]]
- **go.mod**:  tracks dependencies - code imports packages contained in other modules
- `go get`: add a dependencies, upgrade or downgrade dependencies
- `go mod init <command>`: create **go.mod** file
- **package**: group functions - create by all files in the same directory
- **fmt package**: contain funs formating text
- `go mod tidy`: to update dependencies -> create new `go.sum` to authenticating the module
- `pkg.go.dev` site to find published module

**Introduction**
- Module: 
	- collection of packages realeased, versioned and distributed
	- indentified by a [[module path]]
- module path: 
	- the canonical name for a module, declared `go.mod file`
		- prefix for package paths within the module
- version: 
	- immutable snapshot of a module
	- include: major, minor, path version (v0.2.1)
		- incremented major: for example - package is removed
		- incremented minor: ex - add new function
		- incremented minor: ex - fix bug, optimization
- **Package**
	- used to orgaized related code 
	- 2 type of package: 
		- `excutable`: packages are used to create a file, can run out computer 
		- `dependency`: packages defined function named 'main', automatically call when run
- `nil` : meaning no error
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
- 