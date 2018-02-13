**Data Types**

- Go’s typ es fal l into four categories: basic ty pes, aggregate ty pes, reference types, and interface
ty pes. Basic typ es, the topic of this chapter, include numbers, str ings, and boole ans. Aggregate
types—ar rays (§4.1) and str ucts (§4.4)—form more complic ated dat a types by combining values
of several simpler ones. Reference typ es are a diverse group that includes pointers (§2.3.2),
slices (§4.2), maps (§4.3), functions (Chapter 5), and channels (C hapter 8), but what they have
in common is that they al l refer to program variables or state indirectly, so that the effec t of an
operat ion applie d to one reference is obs erved by all copies of that reference.

-Go’s numer ic data typ es include several sizes of integers, floating-p oint numbers, and complex
numbers. Each numer ic type deter mines the size and sig nedness of its values.

-Go provides both sig ned and unsig ned integer arithmetic. There are four distinct sizes of
sig ned integers—8, 16, 32, and 64 bits—repres ented by the typ es int8, int16, int32, and
int64, and corresponding unsig ned versions uint8, uint16, uint32, and uint64

-There are also two typ es called just int and uint that are the natural or most efficient size for
sig ned and unsig ned integers on a par tic ular platform;**Both thes e types have the same size, either 32 or 64 bits, but one must not make assumptions about which**; different compilers may make dif ferent choices even on identical hardware.

-The typ e **rune** is an synonym for int32 and conventionally indic ates that a value is a Unico de
co de point. The two names may be used interchangeably. Simi larly, the typ e **byte** is an synonym
for uint8, and emphasizes that the value is a pie ce of raw dat a rat her than a small
numeric quantity.

-Final ly, there is an unsig ned integer typ e **uintptr**, whose width is not specified but is sufficient
to hold all the bits of a pointer value. The uintptr type is used only for low-le vel
prog ramming,

-Regardless of their size, int, uint, and uintptr are dif ferent typ es from their explicitly size d
siblings. **Thus int is not the same typ e as int32, even if the natural size of integers is 32 bits**,
and an explicit conversion is required to use an int value where an int32 is needed, and vice
versa.

-There are only five levels of precedence for binary operators. Operators at the same level asso
ciate to the lef t, so parenthes es may be required for clarity, or to make the operators evaluate
in the intended order

-The behavior of % for negat ive numbers var ies across programming langu ages. In Go, the sig n of the remainder is
always the same as the sig n of the dividend, so -5%3 and -5%-3 are both 2.
The behavior of / dep ends on whether its operands are integers, so 5.0/4.0 is 1.25, but 5/4 is 1 because integer
division truncates the result toward zero.

-If the result of an arithmetic operat ion, whether signed or unsig ned, has more bits than can be
repres ented in the result typ e, it is said to **overflow**. The hig h-order bits that do not fit are
si lently discarded

-al l values of basic typ e—boole ans, numbers, and str ings—are comparable, meaning
that two values of the same typ e may be compared using the == and != operators. Fur thermore,
integers, floating-p oint numbers, and str ings are ordered by the comparison operators.

-There are also unar y addition and subtrac tion operators:
+ unar y positive (no effec t)
-unary negat ion

-Although Go provides unsig ned numbers and arithmetic, we tend to use the sig ned int form
even for quantities that can’t be negat ive, such as the lengt h of an array, thoug h uint mig ht
seem a more obvious choice. Indeed, the bui lt-in len function retur ns a sig ned int,
medals := []string{"gold", "silver", "bronze"}
for i := len(medals) 1;
i >= 0; i{
fmt.Println(medals[i]) // "bronze", "silver", "gold"
}
The alternat ive would be cal amitous. If len returned an unsig ned number, then i too would
be a uint, and the condition i >= 0 would always be true by definition. After the third iteration,
in which i == 0, the i-- statement would cause i to become not −1, but the maximum
uint value (for example, 264−1), and the evaluation of medals[i] would fai l at run time, or
panic (§5.9), by attempting to access an element outside the bounds of the slice.

-Rune literals are written as a charac ter within single quotes. The simplest example is an ASCII
charac ter like 'a', but it’s possible to write any Unico de co de point either direc tly or with
numeric escap es, as we will see shortly.
Runes are printed with %c, or with %q if quoting is desired:
ascii := 'a'
unicode := 'D'
newline := '\n'
fmt.Printf("%d %[1]c %[1]q\n", ascii) // "97 a 'a'"
fmt.Printf("%d %[1]c %[1]q\n", unicode) // "22269 D 'D'"
fmt.Printf("%d %[1]q\n", newline) // "10 '\n'"

-Go provides two sizes of floating-p oint numbers, float32 and float64.
A float32 provides approximately six decimal digits of precision, whereas a float64
provides about 15 dig its; float64 should be preferred for most pur pos es because float32
computations acc umulate error rapid ly unless one is quite caref ul, and the smallest positive
integer that cannot be exac tly represented as a float32 is not large

-In addition to a large collec tion of the usual mat hemat ical functions, the math package has
functions for creating and detec ting the speci al values defined by IEEE 754: the positive and
negat ive infinities, which represent numbers of excessive mag nitude and the result of division
by zero; and NaN (‘‘not a number’’), the result of such mat hemat ical ly dubious operat ions as
0/0 or Sqrt(1).

-The function math.IsNaN tests whether its argument is a not-a-number value, and math.NaN
returns such a value. It’s tempting to use NaN as a sentinel value in a numer ic computation,
but testing whether a specific computational result is equal to NaN is fraug ht with peril
because any **comparison with NaN always yields false:**

-Go provides two sizes of complex numbers, complex64 and complex128, whose components
are float32 and float64 respectively. The bui lt-in function complex creates a complex number
from its real and imaginary components, and the bui lt-in real and imag functions ext ract
thos e components


-A value of typ e **bool**, or boolean, has only two possible values, true and false. The conditions
in if and for statements are boole ans, and comparison operators li ke == and < produce
a boole an result. The unary operator ! is log ical negat ion, so !true is false, or, one mig ht
say, (!true==false)==true, although as a matter of sty le, we always simplif y redundant
boole an expressions like x==true to x.
Boolean values can be combined with the && (AND) and || (OR) operators, which have shortcircuit
behavior: if the answer is already deter mined by the value of the lef t operand, the rig ht
operand is not evaluated,

-There is no implicit conversion from a boole an value to a numer ic value like 0 or 1, or vice
versa. It’s necessary to use an explicit if, as in
i := 0
if b {
i = 1
}

**Strings**
-A str ing is an immutable sequence of bytes. Str ings may contain arbitrar y data, including
bytes with value 0, but usually they contain human-readable text. Text str ings are conventional
ly interpreted as UTF-8-enco ded sequences of Unico de co de points (runes),

-The bui lt-in len function retur ns the number of bytes (not runes) in a str ing, and the index
operat ion s[i] retr ieves the i-t h byte of str ing s, where 0 ¥ i < len(s).

-The **substring** operat ion s[i:j] yields a new str ing consisting of the bytes of the original str ing
starting at index i and continuing up to, but not including, the byte at index j. The result
contains j-i bytes. Either or both of the i and j operands may be omitted, in which cas e the defau lt values of 0
(t he start of the str ing) and len(s) (its end) are assumed

-Strings may be compared with comparison operators li ke == and <; the comparison is done
byte by byte, so the result is the natural lexicographic order ing.

-Since str ings are immutable, constr uctions that try to modif y a str ing’s dat a in place are not
al lowed:
s[0] = 'L' // compile error: cannot assign to s[0]
Immutability means that it is safe for two copies of a str ing to share the same underlying
memory, mak ing it cheap to copy str ings of any lengt h. Simi larly, a str ing s and a substring
li ke s[7:] may safely share the same dat a, so the substring operat ion is also cheap. No new
memory is allocated in either cas e.

-A **raw string literal** is written `...`, using backquotes instead of double quotes. Within a raw
string literal, no escap e sequences are processed; the contents are taken literal ly, including
backslashes and newlines, so a raw str ing literal may spread over several lines in the program
source. The only processing is that car r iage retur ns are deleted so that the value of the str ing
is the same on all platforms, including those that conventionally put car r iage retur ns in text
files.
Raw str ing literals are a convenient way to write regu lar expressions, which tend to have lots of
backslashes. They are also useful for HTML templates, JSON literals, command usage messages,
and the like, which often extend over multiple lines.

-Four standard packages are par tic ularly important for manipulat ing str ings: bytes, strings,
strconv, and unicode. The strings package provides many functions for searching, replacing,
comparing, tr imming, splitting, and joining str ings.The bytes package has similar functions for manipulat ing slices of bytes, of typ e []byte, which share some properties with strings.
The strconv package provides functions for converting boole an, integer, and floating-p oint
values to and from their str ing representations, and functions for quoting and unquoting
strings.
The unicode package provides functions like IsDigit, IsLetter, IsUpper, and IsLower for
classif ying runes. Each function takes a single rune argument and retur ns a boole an. Conversion
functions like ToUpper and ToLower convert a rune into the given cas e if it is a letter.

**Constants**
-Constants are expressions whose value is known to the compiler and whose evaluation is guaranteed
to occ ur at compile time, not at run time. The underlying typ e of every constant is a
basic typ e: boole an, string, or number.

-Many computations on constants can be completely evaluated at compile time, reducing the
work necessary at run time and enabling other compiler optimizat ions. Errors ordinar ily
detec ted at run time can be rep orted at compile time when their operands are constants, such
as integer division by zero, str ing indexing out of bounds, and any floating-p oint operat ion
that would result in a non-finite value.

-When a sequence of constants is declared as a group, the rig ht-hand side expression may be
omitted for all but the first of the group, implying that the previous expression and its typ e
should be used again. For example:
const (
a = 1
b
c = 2
d
)
fmt.Println(a, b, c, d) // "1 1 2 2"

-A const de clarat ion may use the **constant generator iota**, which is used to create a sequence
of related values without spelling out each one explicitly. In a const de clarat ion, the value of
iota begins at zero and increments by one for each item in the sequence.
Here’s an example from the time package, which defines named constants of typ e Weekday for
the days of the week, starting with zero for Sunday. Typ es of this kind are often cal le d **enumerations**,
or **enums** for short.
type Weekday int
const (
Sunday Weekday = iota
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
)
This declares Sunday to be 0, Monday to be 1, and so on.

**Arrays**
-An array is a fixed-lengt h sequence of zero or more elements of a par tic ular typ e. Because of
their fixed lengt h, ar rays are rarely used direc tly in Go. Slices, which can grow and shrink, are
much more versatile,

-In an array literal, if an ellipsis ‘‘...’’ appears in place of the lengt h, the array lengt h is determined
by the number of initializers.
q := [...]int{1, 2, 3}

-If an array’s element typ e is comparable then the array typ e is comparable too, so we may
direc tly compare two arrays of that typ e using the == operator, which rep orts whether all corresponding
elements are equal.

-When a function is cal le d, a copy of each argument value is assig ned to the corresponding
parameter variable, so the function receives a copy, not the original. Passing large arrays in
this way can be inefficient, and any changes that the function makes to array elements affect
only the copy, not the original. In this regard, Go treats arrays like any other typ e, but this
behavior is dif ferent from languages that implicitly pass arrays by reference.Of course, we can explicitly pass a pointer to an array so that any modifications the function makes to array elements will be visible to the cal ler.

**Slices**

-Slices represent var iable-lengt h sequences whose elements all have the same typ e. A slice typ e
is written []T, where the elements have typ e T; it looks like an array typ e without a size.

-Arrays and slices are intimately connected. A slice is a lig htweig ht dat a struc ture that gives
access to a subsequence (or perhaps all) of the elements of an array, which is known as the
slice’s underlying array. A slice has three components: a pointer, a lengt h, and a cap acity. The
pointer points to the first element of the array that is reachable through the slice, which is not
necessarily the array’s first element. The lengt h is the number of slice elements; it can’t exceed
the cap acity, which is usually the number of elements bet ween the start of the slice and the end
of the underlying array. The bui lt-in functions len and cap return thos e values.

-Multiple slices can share the same underlying array and may refer to overlapping par ts of that
ar ray

-Since a slice contains a pointer to an element of an array, passing a slice to a function per mits
the function to modif y the underlying array elements. In other words, copying a slice creates
an alias (§2.3.2) for the underlying array.

-s := []int{0, 1, 2, 3, 4, 5}
Notice how the expression that initializes the slice s dif fers from that for the array a. A slice
literal lo oks like an array literal, a sequence of values sep arated by commas and sur rounded by
braces, but the size is not given. This implicitly creates an array variable of the rig ht size and
yields a slice that points to it

-Unlike arrays, slices are not comparable, so we cannot use == to test whether two slices contain
the same elements.


-There are two reasons why deep equivalence is problemat ic. First, unlike array elements,
the elements of a slice are indirec t, mak ing it possible for a slice to contain its elf.
Although there are ways to deal with such cas es, none is simple, efficient, and most
importantly, obvious.
Second, because slice elements are indirec t, a fixed slice value may contain different elements
at different times as the contents of the underlying array are modified. Because a hash table
such as Go’s map typ e makes only shallow copies of its keys, it requires that equality for each
key remain the same throughout the lifet ime of the hash table. Deep equivalence would thus
make slices unsuitable for use as map keys. For reference typ es li ke pointers and channels, the
== operator tests reference identity, that is, whether the two entities refer to the same thing. An
analogous ‘‘shallow’’ equality test for slices could be useful, and it would solve the problem
with maps, but the inconsistent treatment of slices and arrays by the == operator would be
conf using. The safest choice is to disallow slice comparisons altogether.
The only legal slice comparison is against nil,

-The bui lt-in **append** function appends items to slices:
The bui lt-in append function may use a more sophisticated growth strateg y than appendInt’s
simplistic one. Usually we don’t know whether a given cal l to append wi l l cause a reallocat ion,
so we can’t assume that the original slice refers to the same array as the resulting slice, nor that
it refers to a dif ferent one. Simi larly, we must not assume that operat ions on elements of the
old slice will (or will not) be reflec ted in the new slice. As a result, it’s usual to assig n the result
of a cal l to append to the same slice variable whose value we passed to append:
runes = append(runes, r)

**Maps**

-The hash table is one of the most ingenious and versatile of all dat a struc tures. It is an
unordered collec tion of key/value pairs in which all the keys are distinct, and the value asso ciated
with a given key can be ret rie ved, updated, or removed using a constant number of key
comparisons on the average, no matter how large the hash table.
In Go, a map is a reference to a hash table, and a map typ e is written map[K]V, where K and V
are the typ es of its keys and values. All of the keys in a given map are of the same typ e, and all
of the values are of the same typ e, but the keys need not be of the same typ e as the values. The
key type K must be comparable using ==, so that the map can test whether a given key is equal
to one already within it.


