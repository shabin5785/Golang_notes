**Functions**

-func name(parameterlist) (resultlist)
{
body
}

If the function retur ns one unnamed result or no results at all, parenthes es are optional and usually omitted. Leaving off the result list entirely declares a function that does not retur n any value and is cal le d only for its effec ts

-Like parameters, results may be named. In that cas e, each name declares a local variable initialize
d to the zero value for its typ e.
A function that has a result list must end with a return statement unless execution cle arly
cannot reach the end of the function, perhaps because the function ends with a cal l to panic
or an infinite for lo op with no break.

-Two functions have the same typ e or
sig nature if they have the same sequence of parameter typ es and the same sequence of result
types. The names of parameters and results don’t affec -t the typ e, nor does whether or not they
were declared using the fac tored form.

-Every function cal l must provide an argument for each parameter, in the order in which the
parameters were declared. **Go has no concept of defau lt parameter values**, nor any way to
specif y arguments by name, so the names of parameters and results don’t matter to the cal ler
except as documentation.

-**Arguments are passed by value**, so the function receives a copy of each argument; modifications
to the copy do not affec t the cal ler. However, if the argument contains some kind of reference,
like a pointer, slice, map, function, or channel, then the cal ler may be affec ted by any
modifications the functionmakes to var iables indirectly referred to by the argument.

-**A function can retur n more than one result.**

- The result of cal ling a multi-value d function is a tuple of values. The cal ler of such a function
must explicitly assig n the values to var iables if any of them are to be used:
links, err := findLinks(url)

-In a function with named results, the operands of a retur n statement may be omitted. This is
called a **bare return.**

-A function for which fai lure is an exp ected behavior retur ns an additional result, conventional
ly the last one. If the fai lure has only one possible cause, the result is a boole an, usually
called ok, as in this example of a cache lookup that always succe e ds unless there was no entry
for that key :
value, ok := cache.Lookup(key)
if !ok {
// ...cache[key] does not exist...
}
More often, and especi ally for I/O, the fai lure may have a var iety of causes for which the cal ler
wi l l need an explanation. In such cas es, the typ e of the additional result is error.

-Usual ly when a function retur ns a non-ni l er ror, its other results are undefined and should be
ig nored. However, a few functions may retur n partial results in error cases

-Go programs use ordinar y control-flow mechanisms like if and return to
respond to errors. This sty le undeni ably demands that more attention be paid to error-handling
log ic, but that is precisely the point.

-When a function cal l returns an error, it’s the cal ler’s responsibility to check it and take
appropriate action

- Ways to handle errors
First, and most common, is to propagate the error, so that a fai lure in a subroutine becomes a
fai lure of the cal ling routine.Because er ror messages are frequently chained toget her, message str ings should not be capitalize d and newlines should be avoide d. The resulting errors may be long, but they wi l l be selfcontained when found by tools like grep.

For errors that represent transient or
unpredic table problems, it may make sense to retry the fai le d operat ion, possibly with a delay
between tries, and perhaps with a limit on the number of attempts or the time spent trying
before giving up entirely.

Third, if progress is impossible, the cal ler can print the error and stop the program gracef ully,
but this course of action should general ly be res erved for the main package of a program.

Fourth, in some cases, it’s sufficient just to log the error and then continue, perhaps with
reduce d functionality

And fifth and finally, in rare cas es we can safely ignore an error entirely:

- the io package guarantees that any read fai lure caused by an end-of-file condition
is always rep orted by a distinguished er ror, io.EOF, which is defined as
var EOF = errors.New("EOF")
The cal ler can detec t this condition using a simple comparison,

-Functions are **first-class values in Go**: like other values, function values have typ es, and they
may be assig ned to var iables or passed to or retur ned from functions. A function value may
be cal le d li ke any other function

-Named functions can be declared only at the package level, but we can use a function literal to
denote a function value within any expression. A function literal is written like a function
de clarat ion, but without a name following the func keyword. It is an expression, and its value
is cal le d an **anonymous function**.

strings.Map(func(r rune) rune { return r + 1 }, "HAL9000")

More importantly, functions defined in this way have access to the entire lexic al environment,
so the inner function can refer to var iables from the enclosing function,

-A **variadic function** is one that can be cal le d with var ying numbers of arguments. The most
fami liar examples are fmt.Printf and its variants. Printf requires one fixed argument at the
beginning, then accepts any number of subsequent arguments.
To declare a var iadic function, the typ e of the final parameter is preceded by an ellipsis, ‘‘...’’,
which indic ates that the functionmay be cal le d with any number of arguments of this typ e.

-Syntactic ally, a **defer statement** is an ordinar y function or met hod cal l prefixed by the
keyword defer. The function and argument expressions are evaluated when the statement is
exec uted, but the actual cal l is deferred until the function that contains the defer statement
has finished, whether normal ly, by executing a retur n statement or fal ling off the end, or
abnormal ly, by panicking. Any number of cal ls may be defer red; they are executed in the
reverse of the order in which they were defer red.

A defer statement is often used with paired operat ions like open and clos e, connect and disconnec
t, or lock and unlock to ensure that res ources are released in all cas es, no matter how
complex the control flow. The rig ht place for a defer statement that releases a res ource is
immediately after the res ource has been successf ully acquired.

-Go’s typ e system catches many mistakes at compile time, but others, like an out-of-b ounds
ar ray access or nil pointer dereference, require checks at run time. When the Go runtime
detec ts these mistakes, it **panics**

-During a typic al panic, normal exec ution stops, all defer red function cal ls in that goroutine are
exec uted, and the program crashes with a log message. This log message includes the panic
value, which is usually an error message of some sort, and, for each goroutine, a stack trace
showing the stack of function cal ls that were active at the time of the panic. This log message
often has enough informat ion to diagnos e the root cause of the problem without running the
prog ram again, so it should always be include d in a bug rep ort about a panicking program.

-Not al l panics come from the runtime. The bui lt-in panic function may be cal le d direc tly ; it
accepts any value as an argument. A panic is often the best thing to do when some ‘‘impossible’’
situation happens, for instance, execution reaches a cas e that logic ally can’t happen.

-Although Go’s panic mechanism res embles exceptions in other langu ages, the situat ions in
which panic is used are quite dif ferent. Since a panic causes the program to crash, it is general
ly used for grave errors, such as a log ical inconsistency in the program; diligent programmers
consider any crash to be proof of a bug in their code. In a robust program, ‘‘exp ected’’
er rors, the kind that arise from incorrec t input, misconfigurat ion, or fai ling I/O, should be
handled gracef ully; they are best dealt with using error values.

-If the bui lt-in recover function is cal le d within a defer red function and the function containing
the defer statement is panicking, recover ends the cur rent state of panic and retur ns the
panic value. The function that was panicking does not continue where it lef t off but retur ns
normal ly. If recover is cal le d at any other time, it has no effec t and retur ns nil.


**Methods**

-an object is simply a value or var iable that has **methods**, and a method is a function
asso ciated with a par tic ular typ e. An object-oriented prog ram is one that uses met hods to
express the proper ties and operat ions of each dat a struc ture so that clients need not access the
objec t’s representation direc tly.

-A met hod is declared with a var iant of the ordinar y function declarat ion in which an ext ra
parameter appears before the function name. The parameter attaches the function to the typ e
of that parameter.

type Point struct{ X, Y float64 }
// traditional function
func Distance(p, q Point) float64 {
	return math.Hypot(q.Xp.X, q.Yp.Y)
}
// same thing, but as a method of the Point type
func (p Point) Distance(q Point) float64 {
	return math.Hypot(q.Xp.X, q.Yp.Y)
}

-The ext ra parameter p is cal le d the met hod’s receiver, a legac y from early object-oriented langu
ages that des cribed cal ling a met hod as ‘‘sending a message to an object.’’
In Go, we don’t use a speci al name like this or self for the receiver; we choose receiver
names just as we would for any other parameter. Since the receiver name will be frequently
used, it’s a good ide a to choose something short and to be consistent across met hods. A common
choice is the first letter of the typ e name, like p for Point

-In a met hod cal l, the receiver argument appears before the method name. This paral lels the
de clarat ion, in which the receiver parameter appears before the method name.

p := Point{1, 2}
q := Point{4, 6}
fmt.Println(Distance(p, q)) // "5", function call
fmt.Println(p.Distance(q)) // "5", method call

-There’s no conflic t between the two declarat ions of functions cal le d Distance ab ove. The first
de clares a package-le vel function. The second de clares a met hod
of the typ e Point, so its name is Point.Distance.

-The expression p.Distance is cal le d a selector, because it selec ts the appropriate Distance
method for the receiver p of typ e Point. Selec tors are also used to selec t fields of str uct typ es,
as in p.X. Since met hods and fields inhabit the same name space, declaring a met hod X on the
struc t type Point would be ambiguous and the compiler will rej e ct it.
So we cannot define a function name X if there is a variable named X inside a struct

-type Path []Point
func (path Path) Distance() float64 {}

Path is a named slice typ e, not a str uct typ e li ke Point, yet we can still define methods for it.
In allowing met hods to be associ ated with any typ e, Go is unlike many other object-oriented
languages. It is often convenient to define additional behaviors for simple typ es such as numbers,
str ings, slices, maps, and sometimes even functions. Met hods may be declared on any
named type defined in the same package, so long as its underlying typ e is neither a pointer nor
an interface.

-the compiler deter mines which function to cal l based on both the met hod name and the typ e of the receiver

-func (p *Point) ScaleBy(factor float64) {
	p.X *= factor
	p.Y *= factor
}

This is a method with a pointer receiver

-In a realistic program, convention dic tates that if any met hod of Point has a pointer receiver,
then all methods of Point should have a pointer receiver, even ones that don’t str ictly need it.

-Named types (Point) and pointers to them (*Point) are the only typ es that may appear in a
receiver declarat ion. Fur thermore, to avoid ambiguities, met hod declarat ions are not per mitted
on named typ es that are themselves pointer typ es:

type P *int
func (P) f() { /* ... */ } // compile error: invalid receiver type

-The (*Point).ScaleBy method can be cal le d by providing a *Point receiver, like this:
	r := &Point{1, 2}
	r.ScaleBy(2)
	fmt.Println(*r) // "{2, 4}"
or this:
	p := Point{1, 2}
	pptr := &p
	pptr.ScaleBy(2)
	fmt.Println(p) // "{2, 4}"
or this:
	p := Point{1, 2}
	(&p).ScaleBy(2)
	fmt.Println(p) // "{2, 4}"
    
-But the last two cas es are ungain ly. Fortunately, the langu age helps us here. If the receiver p is
a variable of typ e Point but the method requires a *Point receiver, we can use this shorthand:
p.ScaleBy(2)
and the compiler will per form an implicit &p on the var iable. This works only for var iables,
including str uct fields like p.X and array or slice elements like perim[0]. We cannot cal l a
*Point method on a non-addressable Point receiver, because there’s no way to obtain the
address of a temporar y value.

Point{1, 2}.ScaleBy(2) // compile error: can't take address of Point literal

But we can call a Point method like Point.Distance with a *Point receiver, because there is
a way to obtain the value from the address: just load the value pointed to by the receiver. The
compiler inserts an implicit * operat ion for us. Thes e two function cal ls are equivalent:
pptr.Distance(q)
(*pptr).Distance(q)

-**In every valid met hod call expression, exactly one of these three statements is true**.

Either the receiver argument has the same typ e as the receiver parameter, for example both
have typ e T or both have typ e *T:
Point{1, 2}.Distance(q) // Point
pptr.ScaleBy(2) // *Point
Or the receiver argument is a variable of typ e T and the receiver parameter has typ e *T. The
compiler implicitly takes the address of the var iable:
p.ScaleBy(2) // implicit (&p)
Or the receiver argument has typ e *T and the receiver parameter has typ e T. The compiler
implicitly dereferences the receiver, in other words, loads the value:
pptr.Distance(q) // implicit (*pptr)

-Nil Is a Valid Receiver Value
Just as some functions allow nil pointers as arguments, so do some methods for their receiver,
especi ally if nil is a meaningf ul- zero value of the typ e, as with maps and slices


-type Point struct{ X, Y float64 }
type ColoredPoint struct {
Point
Color color.RGBA
}

We could have defined ColoredPoint as a str uct of three fields, but instead we embedded a
Point to provide the X and Y fields

-embedding lets us take a  shortcut to defining a ColoredPoint that contains all the fields of Point, plus some
more. If we want, we can selec t the fields of ColoredPoint that were contr ibuted by the embedde d Point without mentioning Point:

cp.X = 1
fmt.Println(cp.Point.X) // "1"
cp.Point.Y = 2
fmt.Println(cp.Y) // "2"

-A simi lar mechanism applies to the methods of Point. We can cal l methods of the embedde d
Point field using a receiver of typ e ColoredPoint, even thoug h ColoredPoint has no
de clared methods

-The met hods of Point have been promoted to ColoredPoint. In this way, embedding allows
complex typ es with many met hods to be bui lt up by the composition of several fields,
providing a few met hods.

-Usual ly we selec t and cal l a met hod in the same expression, as in p.Distance(), but it’s possible
to sep arate these two operat ions. The selec tor p.Distance yields a method value, a function
that binds a met hod (Point.Distance) to a specific receiver value p. This function can
then be invoked without a receiver value; it needs only the non-receiver arguments

p := Point{1, 2}
q := Point{4, 6}
distanceFromP := p.Distance // method value
fmt.Println(distanceFromP(q)) // "5"
var origin Point // {0, 0}
fmt.Println(distanceFromP(origin)) // "2.23606797749979", ;5
scaleP := p.ScaleBy // method value
scaleP(2) // p becomes (2, 4)
scaleP(3) // then (6, 12)
scaleP(10) // then (60, 120)

-**Method values** are useful when a package’s API cal ls for a function value, and the client’s
desired behavior for that function is to cal l a met hod on a specific receiver.

-Related to the method value is the **method expression**. When cal ling a met hod, as opposed to
an ordinar y function, we must supply the receiver in a speci al way using the selec tor syntax. A
method expression, written T.f or (*T).f where T is a typ e, yields a function value with a regular
first parameter tak ing the place of the receiver, so it can be cal le d in the usual way.

p := Point{1, 2}
q := Point{4, 6}
distance := Point.Distance // method expression
fmt.Println(distance(p, q)) // "5"
fmt.Printf("%T\n", distance) // "func(Point, Point) float64"
scale := (*Point).ScaleBy
scale(&p, 2)
fmt.Println(p) // "{2 4}"
fmt.Printf("%T\n", scale) // "func(*Point, float64)"

Method expressions can be helpful when you need a value to represent a choice among several
methods belonging to the same typ e so that you can cal l the chosen met hod with many dif ferent receivers



