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
    * **Subtype Requirement**: 

## Design Guidelines

## Building (code-level) Guidelines