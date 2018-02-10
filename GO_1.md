
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


