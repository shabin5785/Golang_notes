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

-No particular implementation is enforced. It’s enough that type defines methods with desired names and signatures (input and output parameters) to say it implements (satisfies) an interface:

type I interface {
    M() string
}
type T struct {
    name string
}
func (t T) M() string {
    return t.name
}
func Hello(i I) {
    fmt.Printf("Hi, my name is %s\n", i.M())
}
func main() {
    Hello(T{name: "Michał"}) // "Hi, my name is Michał"
}

Type T satisfies interface I defined in the first snippet. Values of type T can be f.ex. passed to any function accepting I as a parameter

-Prominent feature of Golang is that interfaces are implemented implicitly. Programmer doesn’t have to specify that type T implements interface I. That work is done by the Go compiler (never send a human to do a machine’s job). Really nice implication of this behaviours is the possibility to define interface which can be automatically implemented by the types already written (without any changes).

-Flexibility provided by interfaces comes from the fact that single type can implement many interfaces

type I1 interface {
    M1()
}
type I2 interface {
    M2()
}
type T struct{}
func (T) M1() { fmt.Println("T.M1") }
func (T) M2() { fmt.Println("T.M2") }
func f1(i I1) { i.M1() }
func f2(i I2) { i.M2() }
func main() {
    t := T{}
    f1(t) // "T.M1"
    f2(t) // "T.M2"
}


-In Go we’ve two concepts related to interfaces:

Interface — set of methods required to implement such interface. It’s defined using keyword interface
Interface type — variable of interface type which can hold any value implementing particular interface

-**Declaration of the interface** type specifies methods belonging to it. Method is defined by its name and signature — input and result parameters:

type I interface {
    m1()
    m2(int)
    m3(int) int
    m4() int
}

Besides methods it’s allowed to embedded other interfaces — either defined in the same package or imported — using qualified name. It adds all methods from embedded interface

If I embeds an interface J which in turn embeds other interface K, then all methods from K will be also added to I
Circular embedding of interfaces is disallowed and will be detected while compilation

-Variable of interface type I can hold any value implementing I

-Variables have type known at compilation phase. It’s specified while declaration, never changes and is known as static type (or just type). Variables of interface type also have static type which is an interface itself. They additionally have dynamic type so the type of assigned value
Variables have type known at compilation phase. It’s specified while declaration, never changes and is known as static type (or just type). Variables of interface type also have static type which is an interface itself. They additionally have dynamic type so the type of assigned value

-Package reflect can be used to get dynamic type of interface
Also fmt package is able to do that with format verb %T:

type I interface {
    M()
}
type T struct {}
func (T) M() {}
func main() {
    var t *T
    if t == nil {
        fmt.Println("t is nil")
    } else {
        fmt.Println("t is not nil")
    }
    var i I = t
    if i == nil {
        fmt.Println("i is nil")
    } else {
        fmt.Println("i is not nil")
    }
}

t is nil
i is not nil

 Interface type value consists of two components:

dynamic type
dynamic value

 Dynamic value is the actual value assigned. In discussed snippet after assignment var i I = t, dynamic value of i is nil but dynamic type is *T. Function call fmt.Printf("%T\n", i) after this assignment would print *main.T. Interface type value is nil iff both dynamic value and dynamic type are nil. The effect is that even if interface type value holds a nil pointer then such interface value is not nil.
 
 -Method set of interface doesn’t have to contain at least one member. It can be completely empty
 Empty interface is automatically satisfied by any type — so value of any type can be assigned to such interface type variable. The behaviour of dynamic or static types apply to empty interfaces in the same way as to non-empty interfaces. Prominent use of empty interface exists in variadic function fmt.Println.
 
 -Every type which implements all interface’s method automatically satisfies such interface. We don’t need in these types use any other keywords like f.ex. implements in Java to say that type implements an interface. It’s automatically detected by Go compiler and it’s really powerful feature of the language 
 
 -Interface type value gives access ONLY to methods of its interface type. It hides all details about the exact value like if it’s struct, array, scalar etc
 
-There are times when value needs to be converted to a different type. Conversion is checked at compilation-time

-Conversion can be done only if Go compiler is able to check its correctness. Scenarios where it isn’t verifiable at compile-time are as follows:

interface type → concrete type (source code)
 interface type → interface type, where the method set of the right side isn’t a subset of the method set from the type on the left 
 
 
 -The syntax of type assertion expression is as follows:

v.(T)
where v is of interface type and T is either abstract or concrete type.


**GoRoutines**

-Go enables two sty les of concur rent programming. One is goroutines and
channels, which support communicating sequential processes or CSP, a model of concur rency
in which values are passed bet ween indep endent activities (goroutines) but var iables are for
the most par t confined to a single activity. Second is traditional
model of shared memory multithreading, which will be fami liar if you’ve used threads in
other mainstream langu ages.

-In Go, each concur rently executing activity is cal le d a **goroutine**.
If you have used operat ing system threads or threads in other langu ages, then you can assume
for now that a goroutine is simi lar to a thread, and you’ll be able to write correc t prog rams.
The dif ferences bet ween threads and goroutines are essential ly quantitative, not qualitative,

-When a program starts, its only goroutine is the one that cal ls the main function, so we cal l it
the main goroutine. New goroutines are created by the go statement. Syntactic ally, a go statement
is an ordinar y function or met hod cal l prefixed by the key word go. A go statement
causes the function to be cal le d in a newly created goroutine. The go statement its elf completes
immediately:

