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



