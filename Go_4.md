**Interface**
-An interface typ e specifies a set of met hods that a concrete typ e must possess to be considered
an instance of that interface.

-A typ e satisfies an interface if it possesses all the methods the interface requires.

-for each named concrete typ e T, some of its met hods
have a receiver of typ e T itself whereas others require a *T pointer. Recal l also that it is legal to
call a *T method on an argument of typ e T so long as the argument is a variable; the compiler
implicitly takes its address. But this is mere syntactic sugar : a value of typ e T do es not possess
al l the met hods that a *T pointer does, and as a result it mig ht sat isf y fewer interfaces

-This may seem useless, but in fac t the typ e interface{}, which is
called the empty interface type, is indispensable. Because the **empty interface** typ e places no
demands on the typ es that sat isf y it, we can assig n any value to the empty interface.
we’ve been using the empty interface typ e since the ver y first
example in this book, because it is what allows functions like fmt.Println, or errorf in
Section 5.7, to accept arguments of any typ e.

var any interface{}
any = true
any = 12.34
any = "hello"
any = map[string]int{"one": 1}
any = new(bytes.Buffer)

-Interfaces make the code more flexible, scalable and it’s a way to achieve polymorphism in Golang. Instead of requiring a particular type, interfaces allow to specify that only some behaviour is needed


