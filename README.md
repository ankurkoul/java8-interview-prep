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
