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

-
