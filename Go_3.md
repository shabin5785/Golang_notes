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



