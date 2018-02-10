
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
