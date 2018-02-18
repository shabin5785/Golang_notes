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

f() // call f(); wait for it to return
go f() // create a new goroutine that calls f(); don't wait

The main function then retur ns. When this happens, all goroutines are abruptly ter minated
and the program exits. Other than by retur ning from main or exiting the program, there is no
prog rammat ic way for one goroutine to stop another, but as we will see later, there are ways to
communic ate with a goroutine to request that it stop its elf.

- when main routine exitsx all other go routines running is stopped or exited abruptly.

**Channels**

-If goroutines are the activities of a concur rent Go program, channels are the connections
between them. A channel is a communic ation mechanism that lets one goroutine send values
to another goroutine. Each channel is a conduit for values of a par tic ular typ e, cal le d the
channel’s element type. The typ e of a channel whose elements have typ e int is written
chan int.

To create a channel, we use the bui lt-in make function:
ch := make(chan int) // ch has type 'chan int'

-As with maps, a channel is a reference to the dat a struc ture created by make. When we copy a
channel or pass one as an argument to a function, we are copying a reference, so cal ler and
callee refer to the same dat a struc ture. As with other reference typ es, the zero value of a channel
is nil.

-Two channels of the same typ e may be compared using ==. The comparison is true if both are
references to the same channel dat a struc ture. A channel may also be compared to nil.

-A channel has two principal operat ions, send and receive, collec tively known as
communications. A send statement transmits a value from one goroutine, through the channel,
to another goroutine executing a corresponding receive expression. Both operat ions are
wr itten using the <-operator. In a send statement, the <-separates the channel and value operands.
In a receive expression, <-precedes the channel operand. A receive expression whose
result is not used is a valid statement.

ch <-x// a send statement
x = <-ch// a receive expression in an assignment statement
<-ch// a receive statement; result is discarded

-Channels support a third operat ion, close, which sets a flag indic ating that no more values will
ever be sent on this channel; subsequent attempts to send will panic. Receive operat ions on a clos ed channel yield the values that have been sent until no more values are lef t; any receive operat ions thereafter complete immediately and yield the zero value of the channel’s element type. To clos e a channel, we cal l the bui lt-in close function:
close(ch)

A channel created with a simple cal l to make is cal le d an unbuffered channel, but make accepts
an optional second argument, an integer cal le d the channel’s capacity. If the cap acity is nonzero,
make creates a buffered channel.
ch = make(chan int) // unbuffered channel
ch = make(chan int, 0) // unbuffered channel
ch = make(chan int, 3) // buffered channel with capacity 3

-A send operat ion on an **unbuffered channel** blo cks the sending goroutine until another
goroutine executes a corresponding receive on the same channel, at which point the value is
transmitted and both goroutines may continue. Conversely, if the receive operat ion was
attempted first, the receiving goroutine is blo cked until another goroutine per forms a send on
the same channel

Communic ation over an unbuf fered channel causes the sending and receiving goroutines to
synchroniz e. Because of this, unbuffered channels are sometimes cal le d **synchronous** channels.
When a value is sent on an unbuf fered channel, the receipt of the value happens before the
reawakening of the sending goroutine.

Messages sent over channels have two important aspects. Each message has a value, but
sometimes the fac t of communic ation and the moment at which it occ urs are just as
important. We cal l messages events when we wish to stress this aspect. When the event carries
no additional informat ion, that is, its sole pur pos e is synchronizat ion, we’ll emphasize this
by using a channel whose element typ e is struct{}, thoug h it’s common to use a channel of
bool or int for the same pur pos e since done <1
is shorter than done <-struct{}{}.

-Channels can be used to connect goroutines toget her so that the output of one is the input to
another. This is cal le d a **pipeline**.

-After a channel has been clos ed, any fur ther send operat ions on it will panic. After the clos ed
channel has been drained, that is, after the last sent element has been received, all subsequent
receive operat ions will proceed without blo cking but will yield a zero value.

-There is no way to test direc tly whether a channel has been clos ed, but there is a var iant of the
receive operat ion that produces two results: the received channel element, plus a boole an
value, conventionally cal le d ok, which is true for a successf ul receive and false for a receive
on a clos ed and drained channel

-You needn’t clos e ever y channel when you’ve finished with it. It’s only necessary to clos e a
channel when it is important to tell the receiving goroutines that all dat a have been sent. A
channel that the garb age collec tor deter mines to be unreachable will have its res ources
reclaimed whether or not it is clos ed. (

-func counter(out chan int)
func squarer(out, in chan int)
func printer(in chan int)
The squarer function, sitting in the midd le of the pipeline, takes two parameters, the input
channel and the output channel. Both have the same typ e, but their intended uses are
opposite: in is only to be received from, and out is only to be sent to. The names in and out
convey this intention, but still, nothing prevents squarer from sending to in or receiving
from out.
This arrangement is typic al. When a channel is supplie d as a function parameter, it is nearly
always with the intent that it be used exclusively for sending or exclusively for receiving.
To document this intent and prevent misuse, the Go typ e system provides unidirectional channel
typ es that expos e only one or the other of the send and receive operat ions. The typ e
chan<-int, a send-only channel of int, allows sends but not receives. Conversely, the typ e
<-chan int, a receive-only channel of int, allows receives but not sends. (The position of the
<-arrow relative to the chan keyword is a mnemonic.) Violations of this discipline are
detec ted at compile time.

-A **buffered channel** has a queue of elements. The queue’s maximum size is deter mined when it
is created, by the cap acity argument to make.
ch = make(chan string, 3)

A send operat ion on a buf fered channel inserts an element at the back of the queue, and a
receive operat ion removes an element from the front. If the channel is full, the send operat ion
blocks its goroutine until space is made available by another goroutine’s receive. Conversely, if
the channel is empty, a receive operat ion blo cks until a value is sent by another goroutine

-In this example, the send and receive operat ions were all per formed by the same goroutine,
but in real programs they are usually executed by dif ferent goroutines. Novices are sometimes
tempted to use buf fered channels within a single goroutine as a queue, lured by their ple asingly
simple syntax, but this is a mistake. Channels are deeply connected to goroutine
scheduling, and without another goroutine receiving from the channel, a sender—and perhaps
the whole program—risks becoming blo cked forever. If all you need is a simple queue, make
one using a slice.

-**The choice between unbuffered and buffered channels,** and the choice of a buf fered channel’s
capacity, may both af fec t the correc tness of a program. Unbuf fered channels give stronger
sy nchronizat ion guarantees because ever y send operat ion is synchronize d with its corresponding
receive; with buffered channels, these operat ions are decoupled. Also, when we
know an upper bound on the number of values that will be sent on a channel, it’s not unusual
to create a buf fered channel of that size and per form al l the sends before the first value is
received. Fai lure to allocate sufficient buf fer cap acity would cause the program to deadlock.

-Channel buf fer ing may also affec t prog ram performance

-Consider a function that works correc tly in a sequential program. That function is concurrency-
s afe if it continues to work correc tly even when cal le d concurrently, that is, from two or
more goroutines with no additional synchronizat ion. A type is
concurrency-s afe if all its accessible met hods and operat ions are concur rency-s afe.

-We can make a program concurrency-s afe without mak ing every concrete typ e in that
prog ram concurrency-s afe. Indeed, concur rency-s afe typ es are the exception rat her than the
rule, so you should access a var iable concurrently only if the documentation for its typ e says
that this is safe. We avoid concurrent access to most var iables either by confining them to a
single goroutine or by maintaining a hig her-le vel invar iant of mutual exclusion
In contrast, exported package-le vel functions are general ly exp ected to be concur rency-s afe.
Since package-le vel var iables cannot be confined to a single goroutine, functions that modif y
them must enforce mutual exclusion

-we can use a channel of cap acity 1 to ensure that at most one goroutine accesses a shared var iable at a time.
A semaphore that counts only to 1 is cal le d a **binary semaphore**.
This pattern of mutual exclusion is so useful that it is supported direc tly by the Mutex type
from the sync package. Its Lock method acquires the token (cal le d a lock) and its Unlock
method releases it:


-Each time a goroutine accesses the var iables of the ban k (just balance here), it must cal l the
mutex’s Lock method to acquire an exclusive lock. If some other goroutine has acquired the
lo ck, this operat ion will blo ck until the other goroutine cal ls Unlock and the lock becomes
avai lable again. The mutex guards the shared var iables. By convention, the var iables guarde d
by a mutex are declared immediately after the declarat ion of the mutex its elf. If you deviate
from this, be sure to document it.
The reg ion of code bet ween Lock and Unlock in which a goroutine is free to read and modif y
the shared var iables is cal le d a cr itical section. The lock holder’s cal l to Unlock happens before
any other goroutine can acquire the lock for itself. It is essential that the goroutine release the
lo ck once it is finished, on all paths through the function, including error pat hs.

-Each function acquires a mutex
lo ck at the beg inning and releases it at the end, thereby ensuring that the shared var iables are
not accessed concur rently. This arrangement of functions, mutex lock, and var iables is cal le d
a **monitor**.

-but because mutex locks are not re-entrant—it’s not possible to lock a mutex that’s already locked—this
le ads to a deadlock where nothing can proceed,

-There is a good reason Go’s mutexes are not re-entrant. The pur pos e of a mutex is to ensure
that certain invar iants of the shared var iables are maintained at critical points dur ing program
exec ution. One of the invar iants is ‘‘no goroutine is accessing the shared var iables,’’ but there
may be additional invar iants specific to the dat a struc tures that the mutex guards. When a
goroutine acquires a mutex lock, it may assume that the invar iants hold. Whi le it holds the
lo ck, it may update the shared var iables so that the invar iants are temporar ily violated.
However, when it releases the lock, it must guarantee that order has been restored and the
invar iants hold once again. Although a re-entrant mutex would ensure that no other
goroutines are accessing the shared var iables, it cannot protec t the additional invar iants of
thos e var iables.

-Within a single goroutine, the effec ts of each statement are guaranteed to occ ur in the order of
exec ution; goroutines are sequentially consistent. But in the abs ence of explicit synchronization
using a channel or mutex, there is no guarantee that events are seen in the same order by
al l goroutines.

-Even with the greatest of care, it’s all too easy to make concur rency mistakes. Fortunately, the
Go runtime and toolchain are equipped with a sophisticated and easy-to-use dynamic analysis
tool, the **race detector**. Just add the race flag to your go build, go run, or go test command.

**Goroutines and Threads**

-Each OS thread has a fixed-size blo ck of memor y (often as large as 2MB) for its stack, the work
area where it saves the local variables of function cal ls that are in progress or temporar ily
suspende d while another function is cal le d. In contrast, a goroutine starts life with a small stack, typic ally 2KB. A goroutine’s stack, like the stack of an OS thread, holds the local variables of active and suspended function cal ls, but un li ke an OS thread, a goroutine’s stack is not fixed; it grows and shr inks as needed. The size
limit for a goroutine stack may be as much as 1GB, orders of mag nitude larger than a typic al
fixed-size thread stack, thoug h of course few goroutines use that much.

-OS threads are scheduled by the OS ker nel.Every few milliseconds, a hardware timer interrupts
the processor, which causes a ker nel function cal le d the scheduler to be invoked. This
function suspends the cur rently executing thread and saves its reg isters in memor y, looks over
the list of threads and decides which one should run next, restores that thread’s reg isters from
memory, then resumes the execution of that thread. Because OS threads are scheduled by the
kernel, passing control from one thread to another requires a full context switch, that is, sav ing
the state of one user thread to memor y, restoring the state of another, and updat ing the
scheduler’s dat a struc tures. This operat ion is slow, due to its poor locality and the number of
memory accesses required, and has historically only gotten worse as the number of CPU cycles
required to access memor y has increased.
The Go runtime contains its own scheduler that uses a technique known as m:n scheduling,
because it multiplexes (or schedules) m goroutines on n OS threads. The job of the Go
scheduler is analogous to that of the ker nel scheduler, but it is concer ned only with the
goroutines of a single Go program.











-but because mutex locks are not re-entrant—it’s not possible to lock a mutex that’s already locked—this
le ads to a deadlock where nothing can proceed,

-There is a good reason Go’s mutexes are not re-entrant. The pur pos e of a mutex is to ensure
that certain invar iants of the shared var iables are maintained at critical points dur ing program
exec ution. One of the invar iants is ‘‘no goroutine is accessing the shared var iables,’’ but there
may be additional invar iants specific to the dat a struc tures that the mutex guards. When a
goroutine acquires a mutex lock, it may assume that the invar iants hold. Whi le it holds the
lo ck, it may update the shared var iables so that the invar iants are temporar ily violated.
However, when it releases the lock, it must guarantee that order has been restored and the
invar iants hold once again. Although a re-entrant mutex would ensure that no other
goroutines are accessing the shared var iables, it cannot protec t the additional invar iants of
thos e var iables.

-Within a single goroutine, the effec ts of each statement are guaranteed to occ ur in the order of
exec ution; goroutines are sequentially consistent. But in the abs ence of explicit synchronization
using a channel or mutex, there is no guarantee that events are seen in the same order by
al l goroutines.

-Even with the greatest of care, it’s all too easy to make concur rency mistakes. Fortunately, the
Go runtime and toolchain are equipped with a sophisticated and easy-to-use dynamic analysis
tool, the race detector.



