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
