# CppExplain

## Table of Contents
__[Concepts](#Concepts)<br>__
  [Pointers-vs-References](#Pointers-vs-References)<br>
__[Patterns](#Patterns)<br>__
  [Strategy](#Strategy)<br>
__[Anti-Patterns](#Anti-Patterns)<br>__
  [Poltergeist](#Poltergeist)<br>


## Concepts
### Pointers-vs-References
__Q:__ What is the differnce between a pointer and a reference and which one should be used?

__A:__ "Under the hood" they are both the same thing.  They will both be compiled to a value that stores a memory address.  In general a reference should be used whenever possible because it has more restricted behaviour.  Eg. These are operations you can do with a pointer, but not a reference:
 * Assign it to `nullptr`
 * Change what it points to
 * Pointer arithmetic

More details can be found here: https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable-in

## Patterns
### Strategy
Choose between several algorithms at runtime.

Here is an example of using the strategy pattern to select between various sorting algorithms at runtime:
```cpp
class ISort{
  vitual void sort(vector<int>& toSort) = 0;
};

class SortQuick: public ISort{
public:
  void sort(vector<int>& toSort) override{
    // sort using quicksort ...
  }
};

class SortBubble: public ISort{
public:
 void sort(vector<int>& toSort) override{
    // sort using bubble sort ...
  }
};

class SortDistributed: public ISort{
public:
 void sort(vector<int>& toSort) override{
    // use a distrubted sort ...
  }
};

enum class SortingStrategies{
  Quick,
  Bubble,
  Distributed
};

int main(){
  vector<int> toSort;
  SortingStrategies strategy;
  
  cout << "Which sorting strategy should we use?";
  cin >> strategy;
  
  switch(strategy){
    case SortingStrategies::Quick:
      SortQuick sorter;
      sorter.sort(toSort);
      break;
    case SortingStrategies::Bubble:
      SortBubble sorter;
      sorter.sort(toSort);
      break;
    case SortingStrategies::Distributed:
      SortDistributed sorter;
      sorter.sort(toSort);
      break;
  }
}

```

---

## Anti-Patterns
### Poltergeist
A class that does almost nothing besides forward calls to another class.  Eg.

```cpp
class Useful{
public:
  int funcA(){
    //some useful computation
    return val;
  }
  char funcB(){
    //some useful computation
    return val;
  }
  void funcC(){
    //some useful computation
    return val;
  }
};

// Class Poltergeist doesn't really have any responsibility.  It is all forwarded to class Useful.
class Poltergeist{
public:
  int funcA(){
    return _foo.funcA();
  }
  
  char funcB(){
    return _foo.funcB();
  }
  
  void funcC(){
    _foo.funcC();
  }
  
private:
  Foo _foo;
};
```
__HOW TO FIX:__  Simply remove the poltergeist class and use the forwarded class instead.

---
