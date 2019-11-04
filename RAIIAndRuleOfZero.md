# RAII and the Rule of Zero

## Example of `Double Delete (Free)`
```
class NaiveVector {
size_t size_;
int* ptr_
public:
  NaiveVector();
  void push_back(int v);
  ~NaiveVector() { delete [] ptr_; }
};
{
  NaiveVector v;
  v.push_back(1);
  {
    NaiveVector w = v;
  }
  std::cout << v[0] << "\n";    
    // Undefined behavior!! Heap memory of v[0] 
    // is already deleted in the block above
}
```
Solution: Introducing **Copy Constructor**

## Initialization is not assignment
* **Initialization** -- `NaiveVector w = v;`; **Assignment** an existing object -- `NaiveVector w; w = v;`
* Assignment will also have `Double Delete (Free)` problem, so introducing **Copy Assignment**
* Example of **Copy Assignment**
```
NaiveVector::NaiveVector(const NaiveVector& rhs) {
  ptr_ = new int[rhs.size_];
  size_ = rhs.size_;
  std::copy(rhs.ptr_, rhs.ptr_ + rhs.size_, ptr_);
}
```

## The Rule of Three
* **Destructor**: Free the resource;
* **Copy Constructor**
* **Copy Assignment**: We need **Copy and Swap** here!!!!
* Example of **Copy Assignment** using **Copy and Swap**
```
NaiveVector& NaiveVector::operator=(const NaiveVector& rhs) {
  NaiveVector copy(rhs);
  copy.swap(*this);
  return *this;
}
```

## Rule of Zero
* Let the compiler implicitly generate a default destructor
* Let the compiler generate the copy constructor
* Let the compiler generate the copy assignment operator
* **Prefer Rule of Zero when possible**

## rvalue reference
* `int&` **lvalue reference** to an int
* `int&&` **rvalue reference** to an int
* lvalue cannot bind to rvalue, and rvalue cannot bind lvalue; however, a `const` lvalue reference can apply to a rvalue. For example

```
void f(int&);       f(i); //okay    f(42); //error
void g(int&&);      g(i); //error   g(42); //okay
void h(const int&); h(i); //okay    h(42); //error
```

## The Rule of Five
* **Destructor**
* **Copy constructor**
* **Move constructor**
* **Copy assignment operator**
* **Move assignment operator**

## The Rule of Four (and a half)
* **Destructor**
* **Copy constructor**
* **Move constructor**
* **By-value assignment operator**
* 1/2 (**swap** function)

## The Rule of Four (and a half) Example
```
class Vec {
  // Copy constructor: copy the resource
  Vec(const Vec& rhs) {
    ptr_ = new int[rhs.size_];
    size_ = rhs.size_;
    std::copy(rhs.ptr_, rhs.ptr_ + rhs.size_, ptr_);
  }
  
  // Move constructor: transfer ownership
  Vec(Vec&& rhs) nonexcept {
    ptr_ = std::exchange(rhs.ptr_, nullptr);
    size_ = std::exchange(rhs.size_, 0);
  }
  
  // Destructor: free the resource
  ~Vec() { delete [] ptr_; }
  
  // By-value assignment operator
  Vec& operator=(Vec copy) {
    copy.swap(*this);
    return *this;
  }
  
  // Swap: swap ownership
  void swap(Vec& rhs) noexcept {
    using namespace std::swap;
    swap(ptr_, rhs.ptr_);
    swap(size_, rhs.size_);
  }
  
  friend void swap(Vec& a, Vec& b) noexcept {
    a.swap(b);
  }
};
```
