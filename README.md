# CppExplain
The goal of these explanations is to provide basic understanding of common C++ ideas with which a developer could begin using them productively.


<!-- One windows use alt+255 to add a space at the beginning of a line -->
## Table of Contents
__[Concepts](#Concepts)<br>__
  [Pointers vs References](#Pointers-vs-References)<br>
  [Overload Resolution](#Overload-Resolution)<br>
__[Patterns](#Patterns)<br>__
  [Strategy](#Strategy)<br>
  [Facade](#Facade)<br>
__[Anti-Patterns](#Anti-Patterns)<br>__
  [Poltergeist](#Poltergeist)<br>

## Concepts
### Pointers vs References
__Q:__ What is the difference between a pointer and a reference and which one should be used?

__A:__ "Under the hood" they are both the same thing.  They will both be compiled to a value that stores a memory address.  In general a reference should be used whenever possible because it has more restricted behaviour by the compiler.  Eg. These are operations you can't do with a reference, but can do with a pointer:
 * Assign it to `nullptr`
 * Change what it points to
 * Pointer arithmetic

More details can be found here: https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable-in



### Overload Resolution
todo



## Patterns
### Strategy
The strategy pattern allows chosing between several algorithms at runtime.

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



### Facade
A facade is a simpler interface to another interface.  Best used when the other interface is more complex than necessary, or awkard to use.  Eg.

```cpp
class ApiIAmForcedToUse{
public:
  void getter_First_Name_Of_Owner(char* name);
  void getter_Name_Of_Owner_Last(char* name);
  void getter_Owners_Dogs_Name(char* name);
};

class NiceApiFacade{
public:
  std::string getOwnerFirstName(){
    char* name;
    _api.getter_First_Name_Of_Owner(name);
    return std::string(name);
  }
  
  std::string getOwnerLastName(){
    char* name;
    _api.getter_Name_Of_Owner_Last(name);
    return std::string(name);
  }
  
  // If we don't need owner's dog's name then no need to expose it
  
private:
  ApiIAmForcedToUse _api;
}

```

### Singleton
Enforce that only a single instance of an object is created.  Useful for example when developing with hardware.  If there is only one physical piece of hardware in the system then it can make sense to enforce that only a single object in software exists to represent it.
```cpp
class SteeringWheel{
public:
  static SteeringWheel& getInstance(){
    static SteeringWheel _instance;
    return _instance;
  }
  
  void setAngle(int newAngle){
    _angle = newAngle;
  }

private:
  // Declare constructors private so another copy cannot be made
  SteeringWheel(){};
  SteeringWheel(SteeringWheel const&) = delete;
  void operator=(SteeringWheel const&) = delete;
  
  int _angle;
}


int main(){
  SteeringWheel& steeringWheel = SteeringWheel::getInstance();  // Always returns the same instance.
  steeringWheel.setAngle(10);
}
```
__CAUTION:__ This is sometimes considered an anti-pattern mostly because it can lead to bugs in testing.  All tests will use the same singleton, so it is easy for one test to pollute another if the singleton's state is not reset.  Resetting the state can be done using placement new:
```cpp
static void reset(){
  new(_instance) SteeringWheel;
}
```

## Anti-Patterns
### Poltergeist
A poltergeist class is a class that does almost nothing besides forward calls to another class.  Eg.

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

