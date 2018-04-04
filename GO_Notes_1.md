- Go has two types of packages. main and reusbale. 
- main is used to create teh executable package. Only package named main will be converted to executable on go build coommand. Only requirement for main package is that it should have a function named main

- carefully decide if we want the fn to be a receiver type or pass argument to it. Based on design and the logical analysis.

- to create test the file has to be named as _test.go

- go test runs all the tests 

- all test functions are passed an a default pointer argument "testing" by go

- we have take care of cleaning up resources created during testing

- better start all testing functions names with "Test"

- using descriptive names as test fn names can be pretty useful for debugging

- while creating struct objects, we can pass just the values and relay upon go to figure out the fields based on the order. THis is quite confusing and so better use the key value notation of creating structs

- remember to put comma in embedded structs in golang. Even after the internal structs ending brace, if we are declarig it multiline. May be needed for all multi line declaration as well. 

- while embedding struct we can just put embedded struct name within the struct, it becomes that name of embedded struct and field name will be the same as the embedded struct name, Kind of shortcut for declaring

- remember that go can implicitly find our argument type and pass the reference or just the value. We can call the fn using the same way, but based on receiver go will change the way how its called. This can be confusing, so better explicitly state it.

- struct, maps, channels, pointers and functions: all of them are a reference to a memory location of an underlying basic data strcuture. So when they are passed to a fn, by value, the memory referenc is copied. So passing by value for these and change will affect the values of these functions.

- all other types, when we pass value, we infact pass the value of the contents. structs and maps are like this, because they are made from underlying structures. pointer is obvious, so as functions. Channels ?

- by values : int, float, string, bool and struct. ( value types).. need pointers for these

- all keys in map shld be of same type. Values shld be of same type. But key and values need not to be same type

- go needs commas after every entry inside objects.. even for last entry

- structs can have value of diff types, while map shld have of same type.
- struct needs to know all keys at complie time, while for map we can add it on run time.
- maps have all keys indexed, while structs its not
- map is a reference type while struct is a reference type
- map is generally used to represent closed related props while struct is to group togetehr a set of diverse properties
- map we can dynamically remove and add keys. strcuts doesnt allow this.

- go will not complain if we dont use the receiver variable in our code. Now if we are not using it within the functin, we can remove the variable and just keep the type. It will work like a normal defined receiver. 


- in go there is no method overloading.

- interfaces are not generic types. GO doesnt support generics.
- interfaces are implicit in GO
- interface is a contract to help us.

- interface can have other interace within it. So the enclosing interface will have all embedded interfaces within. to satisfy the enclosing one, we need to satisfy all interfaces within it.

- if we have a interface as type of a field in struct, it means that, that field can have any value that satsifies that interface. We can create custom types that satisfies that interface and use it.


