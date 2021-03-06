# Best Practices for Objective Oriented Programming in C++

## Theory of OOP

  * **Definition**: Using polymorphism based runtime function dispatch using virtual functions;
  * Derived objects in OOP considered as independent libraries;
  * Example logging
    * **Base class**: defines library interface;
    * **Derived class**: provide one library implementation;

    ```
    // Base class
    struct Logger {
      virtual void LogMessage(char const* msg) = 0;
      virtual ~Logger() = default;
    };

    // Derived class
    struct ConsoleLogger : final Logger {
      void LogMessage(char const* msg) override {
        std::cout << msg << "\n";
      }
    };
    ```
  * Liscov Substitution
    * **Subtype Requirement**: Code behaved correctly for API defined by Type B object also work correctly for Type D object, D is a subtype of B.
    * **Liscov Substitution**: Each derived type is a logical subtype of base class type, providing intended semantics of the defined interface.
    * **Key Overriding Practices**: An override can ask for less and provide more, but can never require more or promise less.

## Design Guidelines
  ### Use OOP to model "is-a" relationships, not for code-reuse
  * Inheritance: Powerful hierarchies are built on well-defined abstractions.

  ### Make non-leaf class abstract
  * **Object Slicing in C++**: A derived class object can be assigned to a base class object, but the other way is not possible. Object slicing happens when a derived class object is assigned to a base class object, additional attributes of a derived class object are sliced off to form the base class object.
  * Example of object slicing
  ```
  class Base { int x, y; }; 
    
  class Derived : public Base { int z, w; }; 
    
  int main()  
  { 
      Derived d; 
      Base b = d; // Object Slicing,  z and w of d are sliced off 
  }
  ```
  * Rule of thumb: **Only dereferencing an OOP pointer/ref to access base class members.**
  * Example of best practice: 
    * Original design

      <img src="./img/ObjectiveOrientedProgramming/oop_img1.png" height=60% width=60%>

    * Improved Design

      <img src="./img/ObjectiveOrientedProgramming/oop_img2.png" height=60% width=60%>
  
  * Scott Myers steps:
    * Step 1: Make each class in the hierarchy either a base-only or a leaf-only
    * Step 2: Make bases abstract (via pure virtual functions) and protected assignment operators
    * Step 3: Make leaf classes concrete, public assignment operators and **final**
  
  ### Use the non-virtual interface (NVI) idiom
  * Example:
    ```
    class NVILogger {
    public:
      void logMessage(const char * msg) {
        /*Prepare*/
        DoLogMsg(msg);
        /*Cleanup*/
      }

      void ~NVILogger() = default;

    private:
      virtual void DoLogMsg(const char * msg) = 0;
    };
    ```
  * Client interface: This is the public non-virtual interface
  * Subclass interface: This is the private interface, which can have any combination virtual and non-virtual methods.

## Building (code-level) Guidelines

### Always make base class destructor virtual
* Counterexample:

  <img src="./img/ObjectiveOrientedProgramming/oop_img3.png" height=50% width=50%>
  <img src="./img/ObjectiveOrientedProgramming/oop_img4.png" height=50% width=50%>

### Using `override` keyword for overriden functions
* This will force compiler to verify overrides has the exact same signatures as interface defined in base.

### Do not mix overloading and overriding
* Function overloading rules when considering scope
  * 1. look for override in scope if found, collect all signatures and stop
  * 2. Not found, go to outer scope and repeat 1.
  * So, **overloading does not happen across scopes**
  * Example:

    <img src="./img/ObjectiveOrientedProgramming/oop_img5.png" height=50% width=50%>

## Do not specify default values on function overrides
  * Default parameter values are determined by compiler at compile time and inserted into paramter list at compile time.
  * Actual function invoked and determined at runtime. So passed default value will be statically determined as **default parameter in the base class's function declaration**.

## Do not call virtual functions in constructors and destructors

## Use dynamic rather than static casts for downcasting, but avoid casting by refactoring where possible
  * **Upcasting**: Casting a derived type pointer or reference to a pointer or reference up the hierarchy (to a base type).
    * Always safe
  * **Downcasting**: Casting a base type pointer or reference to a pointer or reference down the hierarchy (to a derived type).
    * Problematic: base type cannot be substituted for a derived type object
    * Solution: using conditional dynamic casting and do correct error handling.
    * Dynamic_cast failure: 
      * If the cast fails and new-type is a pointer type, it returns a null pointer of that type. 
      * If the cast fails and new-type is a reference type, it throws an exception that matches a handler of type `std::bad_cast`.