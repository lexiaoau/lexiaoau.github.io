---
author: lexiao
comments: true
date: 2019-08-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: java-collection-数据结构
wordpress_id: 440
tags: summary
categories:
- java
---


<h1>Java 数据结构</h1>


# 共通部分

## 遍历

- 遍历方法 1 ----  使用 stream （JDK 8 推荐 ）

```java

myShapesCollection.stream()
.filter(e -> e.getColor() == Color.RED)
.forEach(e -> System.out.println(e.getName()));

////////////////////////////////////////////////////////////////////

myShapesCollection.parallelStream()
.filter(e -> e.getColor() == Color.RED)
.forEach(e -> System.out.println(e.getName()));

////////////////////////////////////////////////////////////////////

String joined = elements.stream()
    .map(Object::toString)
    .collect(Collectors.joining(", "));

////////////////////////////////////////////////////////////////////

int total = employees.stream()
.collect(Collectors.summingInt(Employee::getSalary)));


```

- 遍历方法 2 ----  使用 for-each 


```java

for (Object o : collection)
    System.out.println(o);


```

- 遍历方法 3 ----  使用 Iterators


```java

static void filter(Collection<?> c) {
    for (Iterator<?> it = c.iterator(); it.hasNext(); )
        if (!cond(it.next()))
            it.remove();
}


```






## 转换为 ArrayList

## 通用函数

-  int size(), 
-  boolean isEmpty(), 
-  boolean contains(Object element), 
-  boolean add(E element), 
-  boolean remove(Object element),  
-  Iterator<E> iterator().
- boolean containsAll(Collection<?> c), 
- boolean addAll(Collection<? extends E> c), 
- boolean removeAll(Collection<?> c), 
- boolean retainAll(Collection<?> c),  
- void clear().
- Object[] toArray()  
- <T> T[] toArray(T[] a)
- (JDK 8 )  Stream<E> stream()  
- (JDK 8 )  Stream<E> parallelStream()


# Set 

## Set 共通部分

```java

//// 把其它collection 转换为 无重复的 set

Collection<Type> noDups = new HashSet<Type>(c);

c.stream()
.collect(Collectors.toSet()); // no duplicates                 /// jdk 8 以上



```















































