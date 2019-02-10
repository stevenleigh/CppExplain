# CppExplain

## Table of Contents
 *[Patterns](#Patterns)<br>
 **[Strategy](#Strategy)<br>
 *[Anti-Patterns](#Anti-Patterns)<br>


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
