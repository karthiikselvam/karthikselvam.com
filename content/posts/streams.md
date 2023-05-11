---
title: "Streams"
description: "2023"
date: "2023-01-25"
tags:
- java
---


In this article, we will solve intervals-related problems that are commonly encountered in interviews.

**1. Given a list of integers, find the sum of all even numbers.**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
int sumOfEvens = numbers.stream()
                        .filter(n -> n % 2 == 0)
                        .mapToInt(Integer::intValue)
                        .sum();

System.out.println(sumOfEvens); 
```
---

**2. Given a list of strings, return a list of strings that start with a given letter.**
```java
List<String> myList = new ArrayList<>();
	      myList.add("pqr");
	      myList.add("stu");
	      myList.add("vwx");
	      myList.add("yza");
	      myList.add("bcd");
	      myList.add("efg");
	      myList.add("vwxy");
List<String> result = myList.stream()
	    		  			.filter(x -> x.startsWith("v"))
	    		  			.collect(Collectors.toList());
System.out.println(result);
```
---

**3. Given a list of integers, return a list of their squares.**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> result = numbers.stream().map(x -> x * x).collect(Collectors.toList());
System.out.print(result);
```
---
**4. Given a list of strings, return a list of the lengths of those strings.**
```java
List<String> myList = new ArrayList<>();
		myList.add("p");
		myList.add("st");
		myList.add("vwx");
		myList.add("ayza");
		myList.add("bcead");
		myList.add("efgass");
		myList.add("vwxyaefw");

List<Integer> result = myList.stream().map(x -> x.length()).collect(Collectors.toList());
System.out.print(result);
```
---
**5. Given a list of integers, find the maximum value.**
```java
List<Integer> numbers = Arrays.asList(1, 2, 10, 88, 101, 3, 7, 999);
Integer result = numbers.stream().max(Integer::compare).get();
System.out.println(result);
```