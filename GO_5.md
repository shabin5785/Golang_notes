-Each package defines a distinct name space that enclos es its identifiers. Each name is associated
with a par tic ular package, letting us choose short, cle ar names for the typ es, functions,
and so on that we use most often, without creating conflic ts with other par ts of the program

Packages also provide encapsulation by controlling which names are visible or exp orted
outside the package. Restr icting the visibility of package members hides the helper functions
and typ es behind the package’s API, allowing the package maintainer to change the implementation
with confidence that no code outside the package will be affec ted. Restr icting visibility
also hides variables so that clients can access and update them only through exp orted functions
that preserve internal invar iants or enforce mutual exclusion in a concur rent program


