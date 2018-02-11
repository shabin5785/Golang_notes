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

