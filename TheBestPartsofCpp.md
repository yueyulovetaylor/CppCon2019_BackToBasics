## The Best Parts of C++ (Opening Keynote) By Jason Turner

## 1. The C++ Standards
* Document specifies the standards of our compilers and programs.

## 2. `const`
* An object declared `const` or accessed via a `const` reference or `const` pointer cannot be modified.

## 3. Deterministic Object Lifetime and Destructor
* Constructor/destructor pairs (RAII) combined with scoped values give us determinism removes the need for things like `finally`.

## 4. Templates
* Have types and values filled in at compile time.

## 5. Algorithms and Standard Template Library (STL)

## 6. `std::array`
* `std::array<Type, Size> data`

## 7. List Initialization

## 8. Variadic Templates
* Allow template definitions to take an arbitrary number of arguments of any type. Since `C++11`.
* Example and explanation
```
template<typename... Params> 
  void printf(const std::string &str_format, Params... parameters);
```
* Roles of Ellipsis (...) Operator:
  * To the left of the name of a parameter, it declares a parameter pack.
  * To the right of a template or function call argument, it unpacks the parameter packs into separate arguments.
  
# 9. `constexpr` (since C++11)
* The constexpr specifier declares that it is possible to evaluate the value of the function or variable at compile time. 
