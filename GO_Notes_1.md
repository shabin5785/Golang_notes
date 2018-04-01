- Go has two types of packages. main and reusbale. 
- main is used to create teh executable package. Only package named main will be converted to executable on go build coommand. Only requirement for main package is that it should have a function named main

- carefully decide if we want the fn to be a receiver type or pass argument to it. Based on design and the logical analysis.

- to create test the file has to be named as _test.go

- go test runs all the tests 

- all test functions are passed an a default pointer argument "testing" by go

- we have take care of cleaning up resources created during testing

- better start all testing functions names with "Test"

- using descriptive names as test fn names can be pretty useful for debugging
