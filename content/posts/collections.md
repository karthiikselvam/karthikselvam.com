---
title: "Using Collections the right way"
description: "2023"
date: "2023-02-23"
tags:
- interview
---

### Iterating through a List

1. Iterating through a List using Index.

Iterating through a list is a basic operation on a collection, but over the years it’s gone through a few significant changes. We’ll begin with the old and evolve an example—enumerating a list of names—to the elegant style.
```java
final List<String> friends = Arrays.asList("Brian", "Nate", "Neal", "Raju", "Sara", "Scott");
```
Here’s the habitual, but not so desirable, way to iterate and print each of the elements
```java
for(int i = 0; i < friends.size(); i++) {
    System.out.println(friends.get(i));
}
```

2. Iterating through a List using Iterator.

```java
for(String name : friends) {
    System.out.println(name);
}
```
Under the hood this form of iteration uses the Iterator interface and calls into its hasNext() and next() methods.Both these versions are external iterators, which mix how we do it with what we’d like to achieve. We explicitly control the iteration with them, indicating where to start and where to end; the second version does that under the hood using the Iterator methods. With explicit control, the break and continue statements can also help manage the iteration’s flow of control.

3. Using forEach 
```java
friends.forEach(new Consumer<String>() {
    public void accept(final String name) {
         System.out.println(name);
}
});
```
we traded in the old for loop for the new internal iterator forEach(). As for the benefit, we went from specifying how to iterate to focusing on what we want to do for each element.

4. Using lambda expressions
```java
friends.forEach((final String name) -> System.out.println(name));
```
The underlying library takes control of how the lambda expressions are evaluated. It can decide to perform them lazily, in any order, and exploit parallelism as it sees fit.

The Java compiler also offers some lenience and can infer the types. Leaving out the type is convenient, requires less effort, and is less noisy. Here’s the previous code without the type information.

```java
friends.forEach((name) -> System.out.println(name));
```

### Transforming a List
Java’s String is immutable, so instances can’t be changed. We could create new strings in all caps and replace the appropriate elements in the collection. However, the original collection would be lost; also, if the original list is immutable, like it is when created with Arrays.asList(), then the list can’t change. Another downside is it would be hard to parallelize the computations.Creating a new list that has the elements in all caps is a better option.

```java
final List<String> uppercaseNames = new ArrayList<String>();
    for(String name : friends) {
        uppercaseNames.add(name.toUpperCase());
}
```

As a first step to move toward a functional style, we could use the internal iterator forEach() method.
```java
final List<String> uppercaseNames = new ArrayList<String>();
friends.forEach(name -> uppercaseNames.add(name.toUpperCase()));
System.out.println(uppercaseNames);
```

We used the internal iterator, but that still required the empty list and the effort to add elements to it.

Using Lambda Expressions : The map() method of a new Stream interface can help us avoid mutability and make the code concise

```java
friends.stream()
.map(name -> name.toUpperCase())
.forEach(name -> System.out.print(name + " "));
```

The map() method is quite useful to map or transform an input collection into a new output collection. This method will ensure that the same number of elements exists in the input and the output sequence. However, element types in the input don’t have to match the element types in the output collection.

### Finding Elements

The now-familiar elegant methods to traverse and transform collections will not directly help pick elements from a collection. The filter() method is designed for that purpose.

From a list of names, let’s pick the ones that start with the letter N. Since there may be zero matching names in the list, the result may be an empty list. Let’s first code it using the old approach.
```java
final List<String> startsWithN = new ArrayList<String>();
for(String name : friends) {
    if(name.startsWith("N")) {
        startsWithN.add(name);
    }
}
```

Let’s refactor this code to use the filter() method.
```java
final List<String> startsWithN = friends.stream()
                                .filter(name -> name.startsWith("N"))
                                .collect(Collectors.toList());
```

The filter() method expects a lambda expression that returns a boolean result. If the lambda expression returns a true, the element in context while executing that lambda expression is added to a result collection; it’s skipped otherwise. Finally the method returns a Stream with only elements for which the lambda expression yielded a true.

The filter() method returns an iterator just like the map() method does, but the similarity ends there. Whereas the map() method returns a collection of the same size as the input collection, the filter() method may not. It may yield a result collection with a number of elements ranging from zero to the maximum number of elements in the input collection. However, unlike map(), the elements in the result collection that filter() returned are a subset of the elements in the input collection.


