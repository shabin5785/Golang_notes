
-Go is a compiled langu age. The Go toolchain converts a source program and the things it 
depends on into instruc tions in the nat ive machine language of a computer. These tools are
accessed through a single command cal le d go that has a number of subcommands. The simplest
of these subcommands is run, which compiles the source code from one or more source
files whose names end in .go, lin ks it with librar ies, then runs the resulting executable file.

-If the program is more than a one-shot exp eriment, it’s likely that you would want to compile
it once and save the compiled result for later use. That is done with go build: