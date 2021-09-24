# Static Binding(early binding) in C++

Static Binding or Early Binding is nothing but most popular compile time polymorphic technique method overloading.

**Definition of Static Binding**: Defining multiple methods with the same name but difference in the number of arguments or the data type of arguments or ordering of arguments and resolving this method calls among multiple methods at ***compilation*** itself is called `Static Binding` or `Early binding` or `Method overloading`.

#### Ambiguity
___
If you define methods with the same signature **(same name, number of arguments, the order of argument, the data type of arguments)exactly same,** then it causes of ambiguity, the compiler is unable to find what method is to be executed.

#### Ordering of arguments
___
```c++
void fun(int lhs, char rhs) { printf("void fun(int lhs, char rhs)\n"); }
void fun(char lhs, int rhs) { printf("void fun(char lhs, int rhs)\n"); }
```

#### Impact of return type
___
    ```c++
    int fun(int lhs) { printf("int fun(int lhs)"); return lhs; }
    void fun() { printf("void fun()"); }
    char fun(char lhs) { printf("char fun(char lhs)"); return lhs; }
    ```
    
    There is no impact on return type in method overloading, so you can't overload a method like this:
    
    ```c++
    void fun(char);
    int fun(char);
    ```