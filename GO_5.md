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




