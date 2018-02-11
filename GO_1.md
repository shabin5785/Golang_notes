
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

**Pointers**

-A pointer value is the address of a var iable. A pointer is thus the locat ion at which a value is
stored. Not every value has an address, but every var iable does. With a pointer, we can read
or update the value of a var iable indirectly, without using or even knowing the name of the
var iable, if indeed it has a name.

-If a var iable is declared var x int, the expression &x (‘‘address of x’’) yields a pointer to an
integer var iable, that is, a value of typ e *int, which is pronounced ‘‘pointer to int.’’ If this
value is cal le d p, we say ‘‘p points to x,’’ or equivalently ‘‘p contains the address of x.’’ The var iable
to which p points is written *p. The expression *p yields the value of that var iable, an
int, but since *p denotes a var iable, it may also appear on the lef t-hand side of an assig nment,
in which cas e the assig nment updates the var iable

x := 1
p := &x // p, of type *int, points to x
fmt.Println(*p) // "1"
*p = 2 // equivalent to x = 2
fmt.Println(x) // "2"

-The zero value for a pointer of any typ e is nil. The test p != nil is true if p points to a var iable.
Pointers are comparable; two pointers are equal if and only if they point to the same
var iable or both are nil.

-Each time we take the address of a var iable or copy a pointer, we create new aliases or ways to
identify the same var iable. For example, *p is an ali as for v. Pointer ali asing is useful because
it allows us to access a var iable without using its name, but this is a double-e dged sword: to
find all the statements that access a var iable, we have to know all its alias es. It’s not just pointers
that create ali ases; aliasing also occurs when we copy values of other reference typ es li ke
slices, maps, and channels, and even str ucts, arrays, and interfaces that contain these typ es

- flag type is used to read flags or command line args 

**new Function**

-Another way to create a var iable is to use the bui lt-in function new. The expression new(T)
creates an unnamed variable of typ e T, initializes it to the zero value of T, and retur ns its
address, which is a value of typ e *T.

p := new(int) // p, of type *int, points to an unnamed int variable
fmt.Println(*p) // "0"
*p = 2 // sets the unnamed int to 2
fmt.Println(*p) // "2"

-A var iable created with new is no dif ferent from an ordinar y lo cal variable whose address is
taken, except that there’s no need to invent (and declare) a dummy name, and we can use
new(T) in an expression. Thus new is only a syntactic convenience, not a fundamental notion:

-Each cal l to new returns a distinct var iable with a unique address:
p := new(int)
q := new(int)
fmt.Println(p == q) // "false"

**Lifetime of Variable**

-The lifetime of a var iable is the interval of time dur ing which it exists as the program exec utes.
The lifet ime of a package-le vel var iable is the entire execution of the program. By contrast,
lo cal variables have dynamic lifet imes: a new instance is created each time the declarat ion
statement is executed, and the var iable lives on until it becomes unreachable, at which point its
storage may be rec ycled. Function parameters and results are local variables too; they are created
each time their enclosing function is cal le d.

-Garb age collec tion is a tremendous help in writing correc t prog rams, but it does not relie ve
you of the burden of thin king about memor y. You don’t need to explicitly allocate and free
memory, but to write efficient programs you still need to be aware of the lifet ime of var iables.
For example, keeping unnecessary pointers to short-lived objec ts within long-lived objec ts,
especi ally global var iables, will prevent the garb age collec tor from reclaiming the short-lived
objec ts.

**Variable Assignment**
-Another form of assig nment, known as tuple assignment, allows several variables to be
assig ned at once. All of the rig ht-hand side expressions are evaluated before any of the var iables
are updated, mak ing this form most useful when some of the var iables appear on both
sides of the assig nment,
x, y = y, x
a[i], a[j] = a[j], a[i]
i, j, k = 2, 3, 5

- Certain expressions, such as a cal l to a function with multiple results, produce several values.
When such a call is used in an assignment statement, the left hand side must have as many
variables as the function has results.

- Assig nment statements are an explicit form of assig nment, but there are many places in a
prog ram where an assig nment occ urs **implicitly**: a function cal l implicitly assig ns the argument
values to the corresponding parameter variables; a return statement implicitly assig ns the
return operands to the corresponding result var iables;

**Variable Type**
-The typ e of a var iable or expression defines the charac ter istics of the values it may take on,
such as their size (number of bits or number of elements, perhaps), how they are represented
internal ly, the intrinsic operat ions that can be per formed on them, and the methods associated
with them.


-A type declaration defines a new named ty pe that has the same underlying type as an existing
type. The named typ e provides a way to sep arate dif ferent and perhaps incompatible uses of
the underlying typ e so that they can’t be mixed unintentionally.
type name underlyingtype

type Celsius float64
type Fahrenheit float64

AbsoluteZeroC Celsius = 273.15
FreezingC Celsius = 0

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }

This package defines two typ es, Celsius and Fahrenheit, for the two units of temperature.
Even thoug h both have the same underlying typ e, float64, they are not the same typ e, so they
cannot be compared or combined in arithmetic expressions. Distinguishing the typ es makes it
possible to avoid errors like inadver tently combining temperatures in the two dif ferent scales;

-For ever y type T, there is a corresponding conversion operat ion T(x) that converts the value x
to typ e T. A conversion from one type to another is allowed if both have the same underlying
type, or if both are unnamed pointer typ es that point to var iables of the same underlying typ e;
thes e conversions change the typ e but not the representation of the value. If x is assig nable to
T, a conversion is per mitted but is usually redundant

-The underlying typ e of a named typ e deter mines its str ucture and representation, and also the
set of intrinsic operat ions it supports, which are the same as if the underlying typ e had been
used direc tly. That means that arithmetic operators work the same for Celsius and Fahrenheit
as they do for float64

var c Celsius
var f Fahrenheit
fmt.Println(c == 0) // "true"
fmt.Println(f >= 0) // "true"
fmt.Println(c == f) // compile error: type mismatch
fmt.Println(c == Celsius(f)) // "true"!


-A named typ e may provide notational convenience if it helps avoid writing out complex typ es
over and over again.Named types also make it possible to define new behaviors for values of the typ e. These
behaviors are expressed as a set of functions associ ated with the typ e, cal le d the typ e’s methods.

**Packages and Files**

-Packages in Go ser ve the same pur pos es as librar ies or modules in other langu ages, supporting
modular ity, encapsulat ion, sep arate compilat ion, and reuse. Each package ser ves as a sep arate name space for its declarat ions. Within the image package,
for example, the identifier Decode refers to a dif ferent function than does the same identifier
in the unicode/utf16 package.

-Packages also let us hide informat ion by controlling which names are visible outside the package,
or exported. In Go, a simple rule governs which identifiers are exp orted and which are
not: exported identifiers start with an upper-case letter.

-Within a Go program, every package is identified by a unique str ing cal le d its import path.The langu age specification doesn’t define where these str ings come from or what they mean;
it’s up to the tools to interpret them. When using the go tool (Chapter 10), an import pat h
denotes a direc tor y containing one or more Go source files that toget her make up the package

-In addition to its import pat h, each package has a package name, which is the short (and not
necessarily unique) name that appears in its package de clarat ion. By convention, a package’s
name matches the last seg ment of its import pat h, making it easy to predic t that the package
name of gopl.io/ch2/tempconv is tempconv.

-The import declarat ion binds a short name to the imported package that may be used to refer
to its contents throughout the file. The import ab ove lets us refer to names within
gopl.io/ch2/tempconv by using a qualified identifier li ke tempconv.CToF. By defau lt, the
short name is the package name—tempconv in this cas e—but an import declarat ion may
specif y an alternat ive name to avoid a conflic t

-Package initializat ion beg ins by initializing package-le vel var iables in the order in which they
are declared, except that dep endencies are res olved first.If the package has multiple .go files, they are initialize d in the order in which the files are given to the compiler ; the go tool sorts .go files by name before invoking the compiler.

- Each var iable declared at package level starts life with the value of its initializer expression, if
any, but for some var iables, like tables of dat a, an initializer expression may not be the simplest
way to set its initial value. In that cas e, the **init function** mechanism may be simpler. Any
file may contain any number of functions whose declarat ion is just
func init() { /* ... */ }
Such init functions can’t be cal le d or reference d, but other wise they are normal functions.
Within each file, init functions are

-One package is initialize d at a time, in the order of imports in the program, dependencies first,
so a package p importing q can be sure that q is fully initialize d before p’s initializat ion beg ins.
Initializat ion proceeds from the bottom up; the main package is the last to be initialize d. In
this manner, all packages are fully initialize d before the applic ation’s main function beg ins.

**Variable Scope**
-Don’t confuse scope with lifet ime. The scope of a declarat ion is a reg ion of the program text;
it is a compile-time proper ty. The lifet ime of a var iable is the range of time dur ing execution
when the var iable can be refer red to by other par ts of the program; it is a run-t ime proper ty.

-A **syntactic block** is a sequence of statements enclos ed in braces like those that sur round the
body of a function or loop. A name declared inside a syntactic blo ck is not visible outside that
block. The blo ck enclos es its declarat ions and deter mines their scope. We can generalize this
notion of blo cks to include other groupings of declarat ions that are not explicitly sur rounded
by braces in the source code; we’ll cal l them all **lexical blocks**. There is a lexic al block for the
entire source code, calle d the **universe block**; for each package; for each file; for each for, if,
and switch statement; for each cas e in a switch or select statement; and, of course, for each
explicit syntactic blo ck.

-A declarat ion’s lexic al block deter mines its scope, which may be large or small. The declarations
of bui lt-in typ es, functions, and constants like int, len, and true are in the universe
block and can be referred to throughout the entire program. Declarat ions outside any function,
that is, at package level, can be refer red to from any file in the same package. Imported
packages, such as fmt in the tempconv example, are declared at the file level, so they can be
referred to from the same file, but not from another file in the same package without another
import. Many declarat ions, like that of the var iable c in the tempconv.CToF function, are
local, so they can be refer red to only from within the same function or perhaps just a par t of it.

-The scope of a control-flow lab el, as used by break, continue, and goto statements, is the
entire enclosing function.

-A program may contain multiple declarat ions of the same name so long as each declarat ion is
in a different lexic al block.

-When the compiler encounters a reference to a name, it looks for a declarat ion, starting with
the inner most enclosing lexic al block and working up to the universe block. If the compiler
finds no declarat ion, it rep orts an ‘‘unde clared name’’ er ror. If a name is declared in both an
outer blo ck and an inner blo ck, the inner declarat ion will be found first. In that cas e, the
inner declarat ion is said to **shadow or hide** the outer one, mak ing it inaccessible

-Within a function, lexical blo cks may be nested to arbitrar y depth, so one local declarat ion can
shadow another. Most blo cks are created by control-flow constr ucts like if statements and
for loops.

-not all lexic al blocks correspond to explicit brace-delimited sequences of
statements; some are merely implie d. The for lo op above creates two lexic al blocks: the
explicit blo ck for the loop body, and an implicit blo ck that additionally enclos es the var iables
de clared by the initializat ion clause, such as i. The scope of a var iable declared in the implicit
block is the condition, post-statement (i++), and body of the for statement








