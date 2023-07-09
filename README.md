# java8-interview-prep

## 1. Aggregation vs composition in java with examples
Aggregation:
 - "has-a" relationship
 -  one class has a reference to another class, but the objects have independent lifecycles
```
class Department {
    private List<Employee> employees;

    public Department() {
        employees = new ArrayList<>();
    }

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    // Other department methods
}

class Employee {
    // Employee class implementation
}
```
Here we are not creating an **employee** object but using it in **list** passed by **addEmployee method**

Composition :
- "has-a" relationship
-  death relationship
-  as stronger ownership

```
class Car {
    private Engine engine;

    public Car() {
        engine = new Engine();
    }

    // Other car methods
}

class Engine {
    // Engine class implementation
}
```

here we create Engine object in Car **constructor** hence If the car is destroyed, the engine will be destroyed as well.

## 2. instance vs static variables

Class consists of 
- instance variables/methods they are known as instance
   - --> as each instance get a **new copy** of each of them
- Static variables/methods --->
   -   Variables and methods can be created that are common to all objects and accessed without using a particular object by declaring them static.


## 3 map.computeIfAbsent(key, k -> new ArrayList<>()) vs getOrDefault(key, new ArrayList<>()) 
-  **map.computeIfAbsent(key, k -> new ArrayList<>())**
    - If the key is not present --> then create new ArrayList<>() and **modify map **with association
 ```
   map.computeIfAbsent(key, k -> new ArrayList<>());
   map.get(key).add(s);
  // here we have already created associated list in map hence no need to put list again
  ```
    - 
-  **map.getOrDefault(key, new ArrayList<>())**
    - If the key is not present --> then **returns** new ArrayList<>() and ** but not modify map ** with association
    -  hence involve 1 extra step to put list to map

```
  List<String> list = map.getOrDefault(key, new ArrayList<>());
  list.add(s);
  map.put(key, list);
// here we have return unassociated list  and hence need to associate it with map 
```





