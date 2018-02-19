-Each package defines a distinct name space that enclos es its identifiers. Each name is associated
with a par tic ular package, letting us choose short, cle ar names for the typ es, functions,
and so on that we use most often, without creating conflic ts with other par ts of the program

Packages also provide encapsulation by controlling which names are visible or exp orted
outside the package. Restr icting the visibility of package members hides the helper functions
and typ es behind the package’s API, allowing the package maintainer to change the implementation
with confidence that no code outside the package will be affec ted. Restr icting visibility
also hides variables so that clients can access and update them only through exp orted functions
that preserve internal invar iants or enforce mutual exclusion in a concur rent program

-Go compilat ion is notably faster than most other compile d languages, even
when bui lding from scratch. There are three main reasons for the compiler’s speed. First, all
imports must be explicitly listed at the beg inning of each source file, so the compiler does not
have to read and process an entire file to deter mine its dependencies. Second, the dep endencies
of a package form a direc ted acyclic graph, and because there are no cycles, packages can
be compiled sep arately and perhaps in paral lel. Final ly, the object file for a compiled Go package
records exp ort informat ion not just for the package its elf, but for its dep endencies too.
When compiling a package, the compiler must read one object file for each import but need
not look beyond thes e files.

-For packages you intend to share or publish, import pat hs should be globally unique. To avoid
conflic ts, the import pat hs of all packages other than those from the standard librar y should
start with the Internet domain name of the organizat ion that owns or hosts the package; this
also makes it possible to find packages.

-A package de clarat ion is required at the start of every Go source file. Its main pur pos e is to
deter mine the defau lt identifier for that package (called the package name) when it is imported
by another package.Conventionally, the package name is the last seg ment of the import pat h, and as a result, two
packages may have the same name even thoug h their import pat hs necessarily dif fer. For
example, the packages whose import pat hs are math/rand and crypto/rand both have the
name rand.

-AGo source file may contain zero or more import de clarat ions immediately after the package
de clarat ion and before the first non-import declarat ion. Each import declarat ion may specif y
the import pat h of a single package, or multiple packages in a parenthesize d list.

If we need to import two packages whose names are the same, like math/rand and
crypto/rand, into a third package, the import declarat ion must specif y an alternat ive name
for at least one of them to avoid a conflic t. This is cal le d a renaming import

import (
"crypto/rand"
mrand "math/rand" // alternative name mrand avoids conflict
)

-The alternat ive name affec ts only the importing file. Other files, even ones in the same package,
may import the package using its default name, or a dif ferent name.
A renaming import may be useful even when there is no conflic t. If the name of the imported
package is unwieldy, as is sometimes the cas e for automat ical ly generated code, an abbreviated
name may be more convenient. The same short name should be used consistently to avoid
conf usion. Choosing an alternat ive name can help avoid conflic ts with common local variable
names.

-It is an error to import a package into a file but not refer to the name it defines within that file.
However, on occ asion we must import a package merely for the side effects of doing so: evaluat
ion of the initializer expressions of its package-le vel var iables and execution of its init functions

To suppress the ‘‘unused import’’ er ror we would other wise encounter, we must
use a renaming import in which the alternat ive name is _, the blank identifier
**This is known as a blank import**. It is most often used to implement a compile-t ime
mechanism whereby the main program can enable optional features by blank-importing additional
packages.

-When creating a package, keep its name short, but not so short as to be cryptic. The most
frequently used packages in the standard librar y are named bufio, bytes, flag, fmt, http, io,
json, os, sort, sync, and time.
Be des criptive and unambiguous where possible.

-go tool, which is used for downlo ading, quer ying,
formatting, building, testing, and instal ling packages of Go code

-The go tool combines the features of a diverse set of tools into one command set. It is a package
manager (analogous to apt or rpm) that answers quer ies ab out its inventory of packages,
computes their dependencies, and downlo ads them from remote version-control systems. It is
a bui ld system that computes file dependencies and invokes compilers, assemblers, and lin kers,
although it is intentionally less complete than the standard Unix make. And it is a test
dr iver, as we will

-The only configurat ion most users ever need is the GOPATH environment var iable, which specifies
the root of the workspace. When switching to a dif ferent workspace, users update the
value of GOPATH.
GOPATH has three subdirec tories. The src subdirec tor y holds source code. Each package
resides in a direc tor y whos e name relative to $GOPATH/src is the package’s import pat h, such
as gopl.io/ch1/helloworld. Obs erve that a single GOPATH workspace contains multiple version-
control rep ositories beneath src, such as gopl.io or golang.org. The pkg subdirec tor y
is where the bui ld tools store compiled packages, and the bin subdirec tor y holds executable
prog rams like helloworld

A second environment var iable, GOROOT, specifies the root direc tor y of the Go distr ibution,
which provides all the packages of the standard librar y. The direc tor y struc ture beneath
GOROOT resembles that of GOPATH, so, for example, the source files of the fmt package reside in
the $GOROOT/src/fmt direc tor y. Users never need to set GOROOT since, by defau lt, the go tool


-When using the go tool, a package’s import pat h indic ates not only where to find it in the local
workspace, but where to find it on the Internet so that go get can ret rie ve and update it.
The go get command can down load a single package or an entire subtree or rep ository using
the ... notation, as in the previous sec tion. The tool also computes and downlo ads all the
dep endencies of the initial packages, which is why the golang.org/x/net/html package
appeared in the workspace in the previous example.
Once go get has downlo ade d the packages, it bui lds them and then installs the librar ies and
commands.

-golint tool, which checks for common sty le problems in Go source code.

-The go get command has support for popular code-hosting sites like GitHub, Bitbucket, and
Launchpad and can make the appropriate requests to their version-control systems. For less
well-k nown sites, you may have to indic ate which version-control protocol to use in the
import pat h, such as Git or Mercurial.

-The direc tories that go get creates are true clients of the remote repository, not just copies of
the files, so you can use version-control commands to see a dif f of local edits you’ve made or to
update to a dif ferent revision

-If you specif y the -u
flag , go get wi l l ensure that all packages it visits, including dep endencies,
are updated to their latest version before being bui lt and instal le d. Without that flag , packages
that already exist local ly will not be updated.

-The go build command compiles each argument package. If the package is a librar y, the
result is discarde d; this merely checks that the package is free of compile errors. If the package
is named main, go build invokes the lin ker to create an executable in the cur rent direc tor y;
the name of the executable is taken from the last seg ment of the package’s import pat h. Since each direc tor y contains one package, each executable program, or command in Unix terminolog
y, requires its own direc tor y

Packages may be specified by their import pat hs, as we saw above, or by a relative direc tor y
name, which must start with a . or .. segment even if this would not ordinar ily be required.
If no argument is provide d, the cur rent direc tor y is assumed.Packages may also be specified as a list of file names, thoug h this tends to be used only for
smal l prog rams and one-off exp eriments. If the package name is main, the executable name
comes from the bas ename of the first .go file.

-The go install command is ver y simi lar to go build, except that it saves the compiled code
for each package and command instead of throwing it away. Compiled packages are saved
beneath the $GOPATH/pkg direc tor y corresponding to the src direc tor y in which the source
resides, and command executables are saved in the $GOPATH/bin direc tor y.
Thereafter, go build and go install do not run the compiler for those packages and commands if they have not changed, mak ing subsequent bui lds much faster. For convenience, go build -i installs the packages that are dep endencies
of the bui ld target.

-Go doc comments are always complete sentences, and the first sentence is usually a summar y
that starts with the name being declared. Function parameters and other identifiers are mentioned
without quotation or markup



