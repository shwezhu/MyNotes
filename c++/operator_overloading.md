# Operator Overloading

#### Assignment operator
___
```c++
// copy assignment
T& operator=(const T& other)
{
    // Guard self assignment
    if (this == &other)
        return *this;
    // do something...
    return *this;
}
```

#### Function call operator
___

When a user-defined class overloads the function call operator, operator(), it becomes a [*FunctionObject*](https://en.cppreference.com/w/cpp/named_req/FunctionObject) type.

```c++
// An object of this type represents a linear function of one variable a*x + b.
class Linear {
public:
    inline double operator()(double x) const { return a*x + b; }
private:
    double a;
    double b;
};

int main() {
    Linear f{2, 1}; // Represents function 2x + 1.
    Linear g{-1, 0}; // Represents function -x.
    
    // f and g are objects that can be used like a function.
    double f_0 = f(0);
    double f_1 = f(1);
    double g_0 = g(0);
}
```

#### Comparison operators
___
```c++
// Pseudocode
bool operator==(const MyComplex<T> &lhs) const;

template<typename T>
inline bool MyComplex<T>::operator==(const MyComplex<T> &lhs) const {
    if(this->real==lhs.real && this->imag==lhs.imag)
        return true;
    else
        return false;
}
```

#### Add operators
___
```c++
MyComplex operator+(const MyComplex<T>& rhs) const;

template<typename T>
inline MyComplex<T> MyComplex<T>::operator+(const MyComplex<T>& rhs) const{
    return MyComplex<T>(this->real + rhs.real, this->imag + rhs.imag);
}
```

#### Increment and decrement
___
When the postfix increment or decrement operator appears in an expression, the corresponding user-defined function (operator++ or operator--) is called with an integer argument `0`. Typically, it is implemented as T operator++(int) or T operator--(int), where the argument is ignored. The postfix increment and decrement  operators are usually implemented in terms of the prefix versions:

```c++
struct X
{
    // prefix increment
    X& operator++()
    {
        // actual increment takes place here
        return *this; // return new value by reference
    }
 
    // postfix increment
    X operator++(int)
    {
        X old = *this; // copy old value
        operator++();  // prefix increment
        return old;    // return old value
    }
 
    // prefix decrement
    X& operator--()
    {
        // actual decrement takes place here
        return *this; // return new value by reference
    }
 
    // postfix decrement
    X operator--(int)
    {
        X old = *this; // copy old value
        operator--();  // prefix decrement
        return old;    // return old value
    }
};
```

#### Array subscript operator
___
User-defined classes that provide array-like access that allows both reading and writing typically define two overloads for operator[]: const and non-const variants:

```c++
struct T
{
    value_t& operator[](std::size_t idx)       { return mVector[idx]; }
    const value_t& operator[](std::size_t idx) const { return mVector[idx]; }
};
```

If the value type is known to be a **built-in type**, the const variant should **return by value**.

To provide multidimensional array access semantics, e.g. to implement a 3D array access a[i] [j] [k] = x;, operator[] has to return a reference to a 2D plane, which has to have  its own operator[] which returns a reference to a 1D row, which has to  have operator[] which returns a reference to the element. To avoid this  complexity, some libraries opt for overloading operator() instead, so that 3D access expressions have the Fortran-like syntax a(i, j, k) = x;

