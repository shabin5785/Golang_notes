
-Go is a compiled langu age. The Go toolchain converts a source program and the things it 
depends on into instruc tions in the nat ive machine language of a computer. These tools are
accessed through a single command cal le d go that has a number of subcommands. The simplest
of these subcommands is run, which compiles the source code from one or more source
files whose names end in .go, lin ks it with librar ies, then runs the resulting executable file.

-If the program is more than a one-shot exp eriment, it’s likely that you would want to compile
it once and save the compiled result for later use. That is done with go build:

-Go code is organize d into packages, which are similar
to libraries or modules in other langu ages. A package consists of one or more .go source files
in a single direc tor y that define what the package does. Each source file beg ins with a package
de clarat ion, here package main, that states which package the file belongs to, followed by a list
of other packages that it imports, and then the declarat ions of the program that are stored in
that file.

-Package main is speci al. It defines a standalone executable program, not a library. Within
package main the function main is also special—it’s where execution of the program beg ins.
Whatever main do es is what the program does

-You must import exactly the packages you need. A program will not compile if there are
missing imports or if there are unnecessary ones. This strict requirement prevents references
to unused packages from acc umulat ing as programs evolve.

-Go does not require semicolons at the ends of statements or declarations, except where two or
more appear on the same line. In effect, newlines following certain tokens are converted into
semicolons, so where newlines are placed matters to proper parsing of Go code.

-Go takes a strong stance on code formatting. The gofmt tool rewrites code into the standard
format, and the go tool’s fmt subcommand applies gofmt to all the files in the specified package,
or the ones in the cur rent direc tor y by defau lt.

**Command Line Args**

-The os package provides functions and other values for dealing with the operat ing system in a
platform-indep endent fashion. Command-line arguments are avai lable to a program in a
var iable named Args that is par t of the os package; thus its name anywhere outside the os
package is os.Args.

-The first element of os.Args, os.Args[0], is the name of the command its elf; the other elements
are the arguments that were presented to the program when it started execution. A
slice expression of the form s[m:n] yields a slice that refers to elements m through n-1

-For Range loop, In each iteration of the loop, range produces a pair of values: the index and the value of the
element at that index. In this example, we don’t need the index, but the syntax of a range lo op
requires that if we deal with the element, we must deal with the index too. One ide a would be
to assig n the index to an obviously temporar y var iable like temp and ignore its value, but Go
do es not per mit unused local variables, so this would result in a compilat ion error.
The solution is to use the blank identifier, whose name is _ (t hat is, an underscore). The blank
identifier may be used whenever syntax requires a variable name but prog ram log ic do es not,
for instance to discard an unwanted lo op index when we require only the element value


**Program Structure**
- Go is case Sensitive

-there are about three dozen predeclared names like int and true for built-in constants,
types, and functions. We can redefine and use them . But around 25 keywords are not permitted to be used in our programs as variable names.

-If an entity is declared within a function, it is local to that function. If declared outside of a
function, however, it is visible in all files of the package to which it belongs. The cas e of the
first letter of a name deter mines its visibility across package boundaries. If the name beg ins
with an upper-case letter, it is exported, which means that it is visible and accessible outside of
its own package and may be refer red to by other par ts of the program, as with Printf in the
fmt package. Package names themselves are always in lower case.

-A declaration names a program entity and specifies some or all of its proper ties. There are
four major kinds of declarat ions: **var, const, type, and func**.

- A Go program is stored in one or more files whose names end in .go. Each file beg ins with a
package de clarat ion that says what package the file is par t of.

-The name of each package-le vel entity is visible not only
throughout the source file that contains its declarat ion, but throughout all the files of the package.
By contrast, local declarat ions are visible only within the function in which they are
de clared and perhaps only within a small part of it


**Variables** 

-A var de clarat ion creates a var iable of a par tic ular typ e, attaches a name to it, and sets its initial
value. Each declarat ion has the general form
_var name type = expression_

Either the typ e or the = expression part may be omitted, but not both.

-If the type is omitted, it is deter mined by the initializer expression. If the expression is omitted, the initial value is the zero value for the typ e, which is 0 for numbers, false for boole ans, "" for str ings, and nil
for interfaces and reference typ es (slice, pointer, map, channel, function). The zero value of an
aggregate typ e li ke an array or a str uct has the zero value of all of its elements or fields

-The zero-value mechanism ensures that a var iable always holds a well-defined value of its typ e;
**in Go there is no such thing as an uninitialized var iable.** This simplifies code and often
ensures sensible behavior of boundary conditions without extra work

-It is possible to declare and optionally initialize a set of var iables in a single declarat ion, with a
matching list of expressions. Omitting the type allows declaration of multiple var iables of different
types:
var i, j, k int // int, int, int
var b, f, s = true, 2.3, "four" // bool, float64, string

-Initializers may be literal values or arbitrar y expressions. Package-le vel var iables are initialize
d before main begins, and local variables are initialize d as their declarat ions are
encountered during function execution.

-A set of var iables can also be initialize d by cal ling a function that retur ns multiple values:
	var f, err = os.Open(name) // os.Open returns a file and an error
    
-Within a function, an alternate form called a short variable declaration may be used to declare
and initialize local var iables. It takes the form name := expression, and the typ e of name is
deter mined by the typ e of expression
  anim := gif.GIF{LoopCount: nframes}
  freq := rand.Float64() * 3.0
  t := 0.0
  
- A var declaration tends to be res erved for local variables
that need an explicit typ e that differs from that of the initializer expression, or for when the
var iable will be assig ned a value later and its initial value is unimportant

-As with var de clarat ions, multiple var iables may be declared and initialize d in the same short
var iable declarat ion,
i, j := 0, 1
Like ordinar y var de clarat ions, short var iable declarat ions may be used for cal ls to functions
li ke os.Open that retur n two or more values:

-One subtle but important point: a short var iable declarat ion does not necessarily declare al l the
variables on its left-hand side. If some of themwere already declared in the same lexic al block, then the short var iable declarat ion acts like an assignment to those variables.
**A short var iable declarat ion must declare at least one new variable**, else code wont compile







