# Const as a Promise 

## Constant Expression `constexpr`

 * Defines an expression that can be evaluated at compile time.
 * A `constexpr` object must be initialized with an constant expression. For example
 ```
 int n = 2;   // non-constant
 constexpr int val = n; // no; non-constant initialiation
 constexpr int val = 2; // yes; 
 ```
 * Prefer `constexpr` to `const` for defining symbolic constants.

## Structure of Declarations:
 * Declaration specifiers (type specifier and non-type specifier) & Declarator
 * `const` is a Type specifier
 * `const` to the immediate right of a * (such as `widget * const cpw`) turns the pointer into a constant pointer
 * `constexpr` => Non-type specifier
 
## Const as a Promise
### Qualification Conversion Definition
Given `CV1` and `CV2`, `CV2` is more cv-qualifier than `CV1` if `CV2` has **every qualifier** in `CV1` plus at least one more.

### `Iterator` as a use case
  * `iterator` and `const_iterator` (pointer to constant)
  * Example
  ```
  deque<int>::iterator i;
  deque<int>::const_iterator ci;
  i = ci;   // no, drops const from point to object
  ci = i;   // okay, adds const from point to object
  ```
  
## Use `const` in Parameter Declaration
  * Examples
  ```
  R foo(T const * p)    // (1) Meaningful constraint
  R foo(T * const p)    // (2) Useless, if not deceptive
  R foo(T const * const p)    // (3) Overkill
  ```
  * Conclusion: Declare a pointer parameter as **"pointer to const"** if the function should not alter the point to object
  * Top Level CV-Qualifiers suggestions: Avoid using `const` at top level of parameter declarations. 
    For example: It is meaningless to define `void *const memcpy(void *const d, void const *const s, size_t const n)`. Defining const to primitive types might not be useful, because we are basically pass these in by value. So in this case, what we actually want is `void * memcpy(void * d, void const * s, size_t  n)`
