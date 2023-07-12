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
-  **map.getOrDefault(key, new ArrayList<>())**
    - If the key is not present --> then **returns** new ArrayList<>() and ** but not modify map ** with association
    -  hence involve 1 extra step to put list to map
```
  List<String> list = map.getOrDefault(key, new ArrayList<>());
  list.add(s);
  map.put(key, list);
// here we have return unassociated list  and hence need to associate it with map 
```

## 3. Behavior parameterization

**Behavior parameterization is a software development pattern that lets you handle frequent requirement changes.**

In a nutshell, it means taking a block of code and making it available without executing it. 
This block of code can be called later by other parts of your programs, which means that you can defer the execution of that block of code. 

- It lets you define a block of code that represents a behavior and then pass it around.
- You can decide to run that block of code when a certain event happens (for example, a click on a button) or at certain points in an algorithm (for example, a predicate such as “only apples havinf 
 color red ” in the filtering algorithm or the customized comparison operation in sorting).
- In general, using this concept you can write code that’s more flexible and reusable.

```
Attempt 1:
public static List<Apple> filterApplesByColor(List<Apple> inventory,
                                             String color) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple: inventory){
        if ( apple.getColor().equals(color) ) {
            result.add(apple);
        }
    }
    return result;
}
```

improve code based on strategy design pattern
- it lets you define a family of algorithms,
- encapsulate each algorithm (called a strategy),
- and select an algorithm at run-time
```
//------------------------------------------------------------------------------------------
Attempt 2: achieve using class

public interface ApplePredicate{
    boolean test (Apple apple);
}
class AppleColorPredicate implements ApplePredicate{
       boolean test (Apple apple){
        return apple.getColor().equals("red");
    }
}

public static List<Apple> filterApplesByColor(List<Apple> inventory,
                                             AppleColorPredicate p) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple: inventory){
       if(p.test(apple)){
            result.add(apple);
        }
    }
    return result;
}

//------------------------------------------------------------------------------------------
Attempt 3: achieve using anonymous class 

public interface ApplePredicate{
    boolean test (Apple apple);
}


public static List<Apple> filterApplesByColor(List<Apple> inventory,  ApplePredicate p) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple: inventory){
       if(p.test(apple)){
            result.add(apple);
        }
    }
    return result;
}

List<Apple> res=filterApplesByColor( myList, new ApplePredicate(){
                                                          boolean test (Apple apple){
                                                                 return apple.getColor().equals("red");
                                                             }
                                              }
                                    )



//------------------------------------------------------------------------------------------
Attempt 4: achieve using lambda

List<Apple> res=filterApplesByColor( myList, app-> app.getColor().equals("red"));

//------------------------------------------------------------------------------------------
Attempt 5: making it generic:

public interface Predicate{
    boolean test (T t);
}

public static <T> List<T> filter(List<T> inventory,  Predicate p) {
    List<T> result = new ArrayList<>();
    for (T e: inventory){
       if(p.test(e)){
            result.add(e);
        }
    }
    return result;
}

List<Apple> redApples = filter(inventory, (Apple apple) -> "red".equals(apple.getColor()));
```

- Behavior parameterization is the ability for a method to take multiple different behaviors as parameters and use them internally to accomplish different behaviors.
- Behavior parameterization lets you make your code more adaptive to changing requirements and saves on engineering efforts in the future.
- Passing code is a way to give new behaviors as arguments to a method. But it’s verbose prior to Java 8.
- Anonymous classes helped a bit before Java 8 to get rid of the verbosity associated with declaring multiple concrete classes for an interface that are needed only once.
The Java API contains many methods that can be parameterized with different behaviors, which include sorting, threads, and GUI handling.




