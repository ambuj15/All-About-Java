```java
Actual Vs Formal Parameters:
Actual Parameters are Values that are passes to a fnction when it is invoked.
Formal Parameters are the variables defined by the function tha receives values when function is called.

	public static void main(String args[]){
	add(1,2); //1,2 are actual Parameters
	}
	static void add(int a , int b){ //a,b are Formal parameters
	System.out.println(a+b);
	}

```

# Generics in java
In a nutshell, generics enable types (classes and interfaces) to be parameters when defining classes, interfaces and methods. 
**Much like the more familiar formal parameters used in method declarations, type parameters provide a way for you to re-use the same code with different inputs.
The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types.

## Benefits of Generics :
Code that uses generics has many benefits over non-generic code:
1. Stronger type checks at compile time.
2. Elimination of casts.
```java
//The following code snippet without generics requires casting:
	List list = new ArrayList();
	list.add("hello");
	String s = (String) list.get(0);
	
//When re-written to use generics, the code does not require casting:
	List<String> list = new ArrayList<String>();
	list.add("hello");
	String s = list.get(0);   // no cast
```

3. Enabling programmers to implement generic algorithms:-
By using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.

4. To provide better **Type Safety.

## Generic Types
A generic type is a generic class or interface that is parameterized over types. 
Eg:-
A Simple Box Class
```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```
** Generic Version
```java
public class Box <T> {
	private T t;
	
	public void ser (T t) { this.t = t; }
	publilc T get(){ return this.t; }
```

### Invoking and Instantiating a Generic Type
To reference the generic Box class from within your code, you must perform a generic type invocation, which replaces T with some concrete value, such as Integer:
Box<Integer> integerBox;
It simply declares that integerBox will hold a reference to a "Box of Integer", which is how Box<Integer> is read.

To instantiate this class, use the new keyword, as usual, but place <Integer> between the class name and the parenthesis:
Box<Integer> integerBox = new Box<Integer>();

Java 7 or later above code can be re-written as, as long as the complieer can determine/infer:-
Box<Integer> integerBox = new Box<>();

** Note Generics can only work with Objects and does not support primitive types.

## Multiple Type Parameters
A generic class can have multiple type parameters. 
```java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}
//The following statements create two instantiations of the OrderedPair class:

Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
						//OR
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
```

## Parameterized Types
You can also substitute a type parameter (i.e., K or V) with a parameterized type (i.e., List<String>). For example, using the OrderedPair<K, V> example:
```java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
```

## Raw Type
A raw type is the name of a generic class or interface without any type arguments. For example, given the generic Box class:
```java
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}
```
To create a parameterized type of Box<T>, you supply an actual type argument for the formal type parameter T:

```java
Box<Integer> intBox = new Box<>();
```
If the actual type argument is omitted, you create a raw type of Box<T>:
```java
Box rawBox = new Box();
```
Therefore, Box is the raw type of the generic type Box<T>. However, a non-generic class or interface type is not a raw type.
Raw types show up in legacy code because lots of API classes (such as the Collections classes) were not generic prior to JDK 5.0. When using raw types, you essentially get pre-generics behavior — a Box gives you Objects. For backward compatibility, assigning a parameterized type to its raw type is allowed:
```java 
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK
But if you assign a raw type to a parameterized type, you get a warning:

Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
You also get a warning if you use a raw type to invoke generic methods defined in the corresponding generic type:

Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)

The warning shows that raw types bypass generic type checks, deferring the catch of unsafe code to runtime. Therefore, you should avoid using raw types.
```

## Unchecked Error Messages
When mixing **legacy code with generic code**, you may encounter warning messages similar to the following:
```java
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```
This can happen when using an older API that operates on raw types:-
```java
public class WarningDemo {
    public static void main(String[] args){
        Box<Integer> bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}
```

The term "unchecked" means that the compiler does not have enough type information to perform all type checks necessary to ensure type safety.
The "unchecked" warning is disabled, by default, though the compiler gives a hint. To see all "unchecked" warnings, recompile with -Xlint:unchecked.
 The @SuppressWarnings("unchecked") annotation suppresses unchecked warnings
 
### Generic Methods:
Generic methods are methods that introduce their own type parameters. This is similar to declaring a generic type, but the type parameter's scope is limited to the method where it is declared.
Eg:- The Util class includes a generic method, compare, which compares two Pair objects:
```java 
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}

Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
//		OR
boolean same = Util.compare(p1, p2);
```
The type has been explicitly provided. Generally, this can be left out and the compiler will infer the type that is needed:
This feature, known as **type inference**, allows you to invoke a generic method as an ordinary method, without specifying a type between angle brackets

## Generics, Inheritance, and Subtypes
It is possible to assign an object of one type to an object of another type provided that the types are compatible. For example, you can assign an Integer to an Object, since Object is one of Integer's supertypes:
```java
Object someObject = new Object();
Integer someInteger = new Integer(10);
someObject = someInteger;   // OK
```
In object-oriented terminology, this is called an "is a" relationship. Since an Integer is a kind of Object, the assignment is allowed. But Integer is also a kind of Number, 
The same is also true with generics. You can perform a generic type invocation, passing Number as its type argument, and any subsequent invocation of add will be allowed if the argument is compatible with Number:
```java
Box<Number> box = new Box<Number>();
box.add(new Integer(10));   // OK
box.add(new Double(10.1));  // OK
```
Now consider the following method:
```
public void boxTest(Box<Number> n) { /* ... */ }
```
What type of argument does it accept? By looking at its signature, you can see that it accepts a single argument whose type is Box<Number>.
But what does that mean? Are you allowed to pass in Box<Integer> or Box<Double>, as you might expect? The answer is "no", because Box<Integer> and Box<Double> are not subtypes of Box<Number>.

![alt text](https://github.com/Ashu-hub/All-About-Java/blob/master/images/generics-subtypeRelationship.gif)

					Box<Integer> is not a subtype of Box<Number> even though Integer is a subtype of Number.

```
Note: Given two concrete types A and B (for example, Number and Integer), MyClass<A> has no relationship to MyClass<B>, regardless of whether or not A and B are related. The common parent of MyClass<A> and MyClass<B> is Object.
```
## Generic Classes and Subtyping
You can subtype a generic class or interface by extending or implementing it. The relationship between the type parameters of one class or interface and the type parameters of another are determined by the extends and implements clauses.

![alt text](https://github.com/Ashu-hub/All-About-Java/blob/master/images/generics-sampleHierarchy.gif)

Now imagine we want to define our own list interface, PayloadList, that associates an optional value of generic type P with each element. Its declaration might look like:
```
interface PayloadList<E,P> extends List<E> {
  void setPayload(int index, P val);
  ...
}
//The following parameterizations of PayloadList are subtypes of List<String>:

PayloadList<String,String>
PayloadList<String,Integer>
PayloadList<String,Exception>
```
![alt text](https://github.com/Ashu-hub/All-About-Java/blob/master/images/generics-payloadListHierarchy.gif)

# Wildcards
In generic code, the question mark (?), called the wildcard, represents an unknown type.
The wildcard can be used in a variety of situations: as the type of a parameter, field, or local variable; sometimes as a return type 
e wildcard is never used as a type argument for a generic method invocation, a generic class instance creation, or a supertype.
	
## Upper Bounded Wildcards
You can use an upper bounded wildcard to relax the restrictions on a variable. For example, say you want to write a method that works on List<Integer>, List<Double>, and List<Number>; you can achieve this by using an upper bounded wildcard.
To declare an upper-bounded wildcard, use the wildcard character ('?'), followed by the extends keyword, followed by its upper bound. Note that, in this context, extends is used in a general sense to mean either "extends" (as in classes) or "implements" (as in interfaces).
To write the method that works on lists of Number and the subtypes of Number, such as Integer, Double, and Float, you would specify List<? extends Number>. The term List<Number> is more restrictive than List<? extends Number>
because the former matches a list of type Number only, whereas the latter matches a list of type Number or any of its subclasses.	
To declare an upper-bounded wildcard, use the wildcard character (‘?’), followed by the extends keyword, followed by its upper bound.
```
public static void add(List<? extends Number> list)
```
## Unbounded Wildcards
The unbounded wildcard type is specified using the wildcard character (?), for example, List<?>. This is called a *list of unknown type.*
There are two scenarios where an unbounded wildcard is a useful approach:-
1. If you are writing a method that can be implemented using functionality provided in the Object class.
2. When the code is using methods in the generic class that don't depend on the type parameter. For example, List.size or List.clear. In fact, Class<?> is so often used because most of the methods in Class<T> do not depend on T.

Consider the following method, printList:
```
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}

//The goal of printList is to print a list of any type, but it fails to achieve that goal — it prints only a list of Object instances; it cannot print List<Integer>, List<String>, List<Double>, and so on, because they are not subtypes of List<Object>. To write a generic printList method, use List<?>:

public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}
```
## Lower Bounded Wildcards
A lower bounded wildcard restricts the unknown type to be a specific type or a super type of that type.
 Syntax: Collectiontype <? super A>
 
	Note: You can specify an upper bound for a wildcard, or you can specify a lower bound, but you cannot specify both.
eg:-
```java
import java.util.Arrays; 
import java.util.List; 
  
class WildcardDemo 
{ 
    public static void main(String[] args) 
    { 
        //Lower Bounded Integer List 
        List<Integer> list1= Arrays.asList(4,5,6,7); 
          
        //Integer list object is being passed 
        printOnlyIntegerClassorSuperClass(list1); 
  
        //Number list 
        List<Number> list2= Arrays.asList(4,5,6,7); 
          
        //Integer list object is being passed 
        printOnlyIntegerClassorSuperClass(list2); 
    } 
  
    public static void printOnlyIntegerClassorSuperClass(List<? super Integer> list) 
    { 
        System.out.println(list); 
    } 
} 
```