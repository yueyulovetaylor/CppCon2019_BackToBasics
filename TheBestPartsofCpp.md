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
  
## 9. `constexpr` (since C++11)
* The constexpr specifier declares that it is possible to evaluate the value of the function or variable at compile time. 

## 10. `auto`
* Automatic deduction of value types.

## 11. Return type deduction for normal functions
* Example: 
```
template<class T, class U>
auto add(T t, U u) { return t + u; }
```

## Usage of `Callable` (`operator()`) Example: printing a map
```
(TODO)
```

## 12. Lambda
* Allowed us to create unnamed function objects which may or may not have captures.
* Examples(TODO)

## 13. Generic and Variadic Lambdas
* Examples (TODO)

## 14. Range based for loop
* Iterate all elements in a container. Works with anything with `begin()` and `end()`, and C style arrays.
* Examples (TODO)

## 15. Structure bindings 
(TODO)

## 16. Concepts

## 17. `std::string_view`

## 18. Text formatting

## How to print a map in `C++20`?

## 19. Ranges

## 20. Class Template Argument Deduction

## 21. RValue references

## 22. Guaranteed Copy Elision

## 23. Defaulted and Deleted Functions
