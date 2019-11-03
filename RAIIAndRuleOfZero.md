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
