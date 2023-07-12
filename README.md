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


## 5 lambdas:

A lambda expression can be understood as a concise representation of an anonymous function that can be passed around

- Anonymous— We say anonymous because it doesn’t have an explicit name like a method would normally have: less to write and think about!
- Function— We say function because a lambda isn’t associated with a particular class like a method is.
     -  But like a method, a lambda has a list of parameters, a body, a return type, and a possible list of exceptions that can be thrown.
- Passed around— A lambda expression can be passed as argument to a method or stored in a variable.
- Concise— You don’t need to write a lot of boilerplate like you do for anonymous classes.

 lambda expression is composed of parameters, an arrow, and a body.
 (a,b) -> a.compareTo(b)
 
 few  valid example: 
1.  () -> {}      //This lambda has no parameters and returns void. It’s similar to a method with an empty body: public void run() { }.

2.  () -> "Raoul"

3.  () -> {return "Mario";}   //This lambda has no parameters and returns a String (using an explicit return statement).

## 6  functional interface:

functional interface is an interface that specifies exactly one abstract method 
An interface is still a functional interface if it has many **default methods** as long as it specifies only one abstract method.
- like comparator, comparable,runnable,callable

```
public interface Adder{
    int add(int a, int b);
}
public interface SmartAdder extends Adder{
    int add(double a, double b);
}
public interface Nothing{
}

Answer:
Only Adder is a functional interface.SmartAdder isn’t a functional interface because it specifies two abstract methods called add (one is inherited from Adder).
Nothing isn’t a functional interface because it declares no abstract method at all.
```
- Lambda expressions let you provide the implementation of the abstract method of a functional interface directly inline
- and treat the whole expression as an instance of a functional interface 


```
Functional interface         |         Function descriptor|         Primitive specializations

Predicate<T>                          	T -> boolean	                         IntPredicate, LongPredicate, DoublePredicate
Consumer<T>	                           T -> void	                           	IntConsumer, LongConsumer, DoubleConsumer
Function<T, R>		                       T -> R	                               IntFunction<R>, IntToDoubleFunction, IntToLongFunction, LongFunction<R>, LongToDoubleFunction, LongToIntFunction, 
                                                                             DoubleFunction<R>, ToIntFunction<T>, ToDoubleFunction<T>, ToLongFunction<T>
Supplier<T>	                           () -> T	                              BooleanSupplier, IntSupplier, LongSupplier, DoubleSupplier



UnaryOperator<T>	 	                    T -> T	                              IntUnaryOperator, LongUnaryOperator, DoubleUnaryOperator
BinaryOperator<T>	 	                   (T, T) -> T 	                       	IntBinaryOperator, LongBinaryOperator, DoubleBinaryOperator
BiPredicate<L, R>	 	                   (L, R) -> boolean	 
BiConsumer<T, U>	 	                    (T, U) -> void	 	                    ObjIntConsumer<T>, ObjLongConsumer<T>, ObjDoubleConsumer<T>
BiFunction<T, U, R>	 	                 (T, U) -> R 	                    	ToIntBiFunction<T, U>, ToLongBiFunction<T, U>, ToDoubleBiFunction<T, U>

```
## 7  Different functional interface:
- based on function descriptor
  -    signature of the abstract method
- Predicate --->  boolean test(T e) ---> SAM with name test and boolean return
   -  ```
           @FunctionalInterface
           public interface Predicate<T>{
               boolean test(T t);
           }
      ```
- consumer --->  void accept(T e) ---> SAM with name accept take generic type T  and void return
-  ```
       @FunctionalInterface
           public interface Consumer<T>{
               void accept(T t);
           }
   
       public static <T> void myPrint(List<T> list, Consumer<T>c){
          for(T e: list){
            c.accept(e);
          }
         }
   
       myPrint(list,(Integer i)-> System.out.println(i);
   ```
    - you need to access an object of type T and perform some operations on it.
    - For example, you can use it to create a method forEach, which takes a list of Integers and applies an operation on each element of that list
      
- Function --->  R apply(T e) ---> SAM with name apply take generic type T and R return
- ```
           @FunctionalInterface
           public interface Function<T,R>{
               R apply(T t);
           }

   
        public static <T,R> List<R> map(List<T> list, Function<T,R> f){
            List<R> res=new ArrayList<>();
          for(T e: list){
            res.add(f.apply(e));
          }
           return res;
         }
          
       List<Integer> res=  map(myList, (String s)->s.length())
  ```
  - You might use this interface when you need to define a lambda that maps information from an input object to an output
  - (for example, extracting the weight of an apple or mapping a string to its length).
 

-  Supplier<T> ---> has a SAM called **get** representing a function descriptor
-  ``` () -> T ```
-   Ex : Callable<T> also has a SAM called call representing a function descriptor () -> T.

5.  BiFunction<T, U, R> has a SAM called apply representing a function descriptor  ``` (T, U) -> R ```
 ## 7  Primitive specializations
 
 - specialized functional interfaces 
-  every Java type is either a reference type or a primitive type
-  But generic parameters (for example, the T in Consumer<T>) can be bound only to reference types.
-  primitive to corresponding Reference conversion aka boxing and reverse is unboxing
-  boxed values use more memory as store in heap
-  Hence Java 8 brings a specialized version of the functional interfaces
-  ex: DoublePredicate,  IntConsumer,  LongFunction , LongBinaryOperator
  
## 8:  Examples of lambdas with functional interfaces

```
Use case                       |             Example of lambda            |                      Matching functional interface

A boolean expression	                    (List<String> list) -> list.isEmpty()	                    Predicate<List<String>>


Creating objects                         () -> new Apple(10)	                                     Supplier<Apple>


Consuming from an object	                (Apple a) -> System.out.println(a.getWeight())           	Consumer<Apple>


Select/extract from an object	           (String s) -> s.length()                                 	Function<String, Integer> or ToIntFunction<String>


Combine two values	                      (int a, int b) -> a * b                                  	IntBinaryOperator


Compare two objects	                    (Apple a1, Apple a2) ->{
                                               a1.getWeight().compareTo (a2.getWeight())          	Comparator<Apple> or BiFunction<Apple, Apple, Integer> or ToIntBiFunction<Apple, Apple>
                                             }

```

Note that none of the functional interfaces allow for a checked exception to be thrown

## 9: capturing lambdas.
- Lambda expressions are also allowed to use free variables (variables that aren’t the parameters and defined in an outer scope)
- allowed to use ** instance variables and static variables** without restrictions.
-  But **local variables** must be final or effectively final
       -  reason: local variables live in stack if lambda were used in a thread,
       -  then the thread using the lambda could try to access the variable after the thread that allocated the variable had deallocated it hence thread unsafe
       -  instance variables are fine because they live on the heap, which is shared across threads
       -  Hence, Java implements access to a free local variable as access to a copy of it rather than access to the original variable 
-  lambda expressions can capture local variables that are assigned to them only once
- ```
         int portNumber = 1337;
         Runnable r = () -> System.out.println(portNumber);
         portNumber = 1337;
  
         //Compiler Error : local variable must be final or effectively final
         reason because variable portNumber is assigned to twice
  
      ```

## 10: method references:
- shorthand for lambdas calling only a specific method.
- ```

                Lambda                                                                                  Method reference equivalent
                
                (Apple a) -> a.getWeight()	                                                   Apple::getWeight
  
                () -> Thread.currentThread().dumpStack()	                                     Thread.currentThread()::dumpStack
  
                (str, i) -> str.substring(i)	                                                 String::substring
  
                (String s) -> System.out.println(s)                                           System.out::println
  
  ```

![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/221f14f6-5f18-42df-9439-13a62aa1c434)

##11: Constructor references

- ClassName::new

![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/9023ba88-ffa5-4b4e-838f-d24ffdb5ddea)

![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/d79b617c-314d-4f4e-8a45-62f336dfe325)

```
          public interface TriFunction<T, U, V, R>{
                 R apply(T t, U u, V v);
        }


       TriFunction<Integer, Integer, Integer, Color> colorFactory = Color::new;
```


## 12: Composing Predicates
- Predicate interface includes three methods that let you reuse an existing Predicate to create more complicated ones:
-  negate, and, and or.
-  negate:![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/78e36c5f-094a-4212-a070-87f0bbd847dd)
-  and: ![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/5954ee23-cb34-4a71-9c4e-a2beaa9a33a3)
-  and/or: ![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/b0e428b1-3bf0-4de5-9e3b-dc1e02cdacfb)


## 13: Composing Functions
- The Function interface comes with two default methods for this,
   - andThen and
   - compose,
- For example, given a function f that increments a number (x -> x + 1)
- and another function g that multiples a number by 2,
-  you can combine them to create a function h that first increments a number and then multiplies the result by 2
- ![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/1c9ca8c6-7ecc-44c4-a555-47f3232501f3)

- ![image](https://github.com/ankurkoul/java8-interview-prep/assets/7008603/fa8dbf43-53e2-4d29-8a0b-3f0d5d582c10)




 


