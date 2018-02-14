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




