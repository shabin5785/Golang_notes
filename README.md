# Golang_notes
Notes taken during learning Go lang

**Why GO**
- Go is written for multiple cores. To take advantage of multipe cores and is designed to do that. Concurrecy is also easy in go by design. Its compiled language. So very fast, as its converted to binary before executing. Its garbage collected, clean syntax and powerful std library

- It complies in many machines, compile once run every where. Open source by google.

- go lang playground to live check the code working..

- generally semi colons are not inserted by user. They are automatically inserted by compiler. we can add semi colons if we need them or to help the compiler. 

- firt line in the go file needs to be the package declartion. Then comes the import statement. then the main function that needs to run.

- golang is the standard package site. Godoc has third party packages as well..

- its preferable to keep a single workspace for go code. A single folder having all code that we write, This allows us easy package management and namespacing. So there wont be any conficts. Third party librarires come with this structure. so be used to it.

- if using a third party package, we need to use the entire package name, while std packages needs just the name. One of the reason while package strucutre is important. Clear separation of packages and namespaces.

- definitely set the gopath to your workspace and goroot to where we installed go.. go path defines where the pacakges we pull wll be saved.

- file with main fn can be named anything.. Not jut main...but nice to do that so we can easily identify.

- go run , runs the file while go build creates an executable which we can run ourselves. go clean will remvoe the built file. go install will build and put the exe in the bin folder. Now we can run the exe file in the bin folder from any where as the bin folder is path.. cool

- Printf is to print by formatting.

- Go will trigger warning if we have packages impported that we are  not using.


**Packages**

- package names should be same as folder name
- system packages can be imported with the name, but third party packages need to given their full path for import

- functions and variables in files within same packages are accesible to each other. No need to import them. Package scope.

- When something is lower case, its not exported outside the package, like func with lower case names. Ones with uppercase are exported to outside the package. But lower case funcs are visible within packages .

- if we run go build without a file name in a package with a main function, then it will build and create executable. 
- go clean remvoes the build files..

**Variables**
- shorthand assignment of variables can be used within a func only.

-  a := 10 is the shorthand declaration. Type is inferred by go