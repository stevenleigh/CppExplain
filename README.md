# CppExplain

## Patterns
### Poltergeist (Anti-Pattern)
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
_FIX:_  Remove the poltergeist class and use the forwarded class instead.
