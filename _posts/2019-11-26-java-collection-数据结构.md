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


- [共通部分](#%e5%85%b1%e9%80%9a%e9%83%a8%e5%88%86)
  - [遍历](#%e9%81%8d%e5%8e%86)
  - [转换为 ArrayList](#%e8%bd%ac%e6%8d%a2%e4%b8%ba-arraylist)
  - [通用函数](#%e9%80%9a%e7%94%a8%e5%87%bd%e6%95%b0)
- [Set](#set)
  - [Set 共通部分](#set-%e5%85%b1%e9%80%9a%e9%83%a8%e5%88%86)
- [List](#list)
  - [List 共通部分](#list-%e5%85%b1%e9%80%9a%e9%83%a8%e5%88%86)
    - [子列表视图 subList(int fromIndex, int toIndex)](#%e5%ad%90%e5%88%97%e8%a1%a8%e8%a7%86%e5%9b%be-sublistint-fromindex-int-toindex)
    - [List 算法(操作)](#list-%e7%ae%97%e6%b3%95%e6%93%8d%e4%bd%9c)
- [Queue 队列](#queue-%e9%98%9f%e5%88%97)
  - [Queue 共通部分](#queue-%e5%85%b1%e9%80%9a%e9%83%a8%e5%88%86)
  - [priority queues 部分](#priority-queues-%e9%83%a8%e5%88%86)
  - [FIFO Queue](#fifo-queue)
- [Deque 双端队列](#deque-%e5%8f%8c%e7%ab%af%e9%98%9f%e5%88%97)
- [Map](#map)
- [Map 共通部分](#map-%e5%85%b1%e9%80%9a%e9%83%a8%e5%88%86)


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

# List

## List 共通部分

```java

//// add -- to the end
List<Type> list3 = new ArrayList<Type>(list1);
list3.addAll(list2);


List<String> list = people.stream()
.map(Person::getName)
.collect(Collectors.toList());

//// 通过序号 get/set
public static <E> void swap(List<E> a, int i, int j) {
    E tmp = a.get(i);
    a.set(i, a.get(j));
    a.set(j, tmp);
}


//// 使用  ListIterator 进行遍历，除了 next 还可以 previous ,
///// 其它操作包括  add/remove/set/get/
for (ListIterator<Type> it = list.listIterator(list.size()); it.hasPrevious(); ) {
    Type t = it.previous();
    ...
}




```

### 子列表视图     subList(int fromIndex, int toIndex)

### List  算法(操作)

 - sort — sorts a List using a merge sort algorithm, which provides a fast, stable sort. (A stable sort is one that does not reorder equal elements.)
 - shuffle — randomly permutes the elements in a List.
 - reverse — reverses the order of the elements in a List.
 - rotate — rotates all the elements in a List by a specified distance.
 - swap — swaps the elements at specified positions in a List.
 - replaceAll — replaces all occurrences of one specified value with another.
 - fill — overwrites every element in a List with the specified value.
 - copy — copies the source List into the destination List.
 - binarySearch — searches for an element in an ordered List using the binary search algorithm.
 - indexOfSubList — returns the index of the first sublist of one List that is equal to another.
 - lastIndexOfSubList — returns the index of the last sublist of one List that is equal to another.


# Queue 队列

## Queue 共通部分

<table>
<tbody><tr>
<th>Type of Operation</th>
<th>Throws exception</th>
<th>Returns special value</th>
</tr>
<tr>
<td>Insert</td>
<td><code>add(e)</code></td>
<td><code>offer(e)</code></td>
</tr>
<tr>
<td>Remove</td>
<td><code>remove()</code></td>
<td><code>poll()</code></td>
</tr>
<tr>
<td>Examine</td>
<td><code>element()</code></td>
<td><code>peek()</code></td>
</tr>
</tbody>
</table>

- Queue 可以限制queue长度，称为bounded
- Queue 一般不允许加 **null** ， 但是    **LinkedList** 是例外。


## priority queues 部分

## FIFO Queue



# Deque 双端队列

- 可以在队列的两端 add/remove 元素。


<table summary="Basic methods in the Deque interface" border="1" cellpadding="3" cellspacing="1">
<caption id="deque-methods"><strong>Deque Methods</strong></caption>
  <tbody><tr>
    <th>Type of Operation</th>
    <th>First Element (Beginning of the <code>Deque</code> instance)</th>
    <th>Last Element (End of the <code>Deque</code> instance)</th> 
    </tr>
  <tr>
    <td><b>Insert</b></td>
    <td><code>addFirst(e)</code><br><code>offerFirst(e)</code></td>
    <td><code>addLast(e)</code><br><code>offerLast(e)</code></td>
     </tr>
  <tr>
    <td><b>Remove</b></td>
    <td><code>removeFirst()</code><br><code>pollFirst()</code></td>
    <td><code>removeLast()</code><br><code>pollLast()</code></td>
  </tr>
  <tr>
    <td><b>Examine</b></td>
    <td><code>getFirst()</code><br><code>peekFirst()</code></td>
    <td><code>getLast()</code><br><code>peekLast()</code></td>
  </tr>
 </tbody></table>


# Map 


# Map 共通部分

- 基本操作 (put/get/remove/containsKey/containsValue/size/empty),
- 批量操作 (putAll/clear)
- collection views (keySet/entrySet/values)


3 种 Map 实现

HashMap, TreeMap, and LinkedHashMap. 

```java

///// 遍历 Map
for (Map.Entry<KeyType, ValType> e : m.entrySet())
    System.out.println(e.getKey() + ": " + e.getValue());



```




























