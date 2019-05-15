---
author: lexiao
comments: true
date: 2019-05-14 01:35:22+00:00
layout: post
link: http://localhost/blog/?p=345
slug: java%e6%b3%9b%e5%9e%8b%e7%bc%96%e7%a8%8b-generic-programming
title: java泛型编程 generic programming
wordpress_id: 345
categories:
- java
---

<font color="#0000ff" size="4">声明一个泛型的类：</font>
    
    <br>
    
    /**
     * Generic version of the Box class.
     * @param <T> the type of the value being boxed
     */
    public class Box<T> {
        // T stands for "Type"
        private T t;
    
        public void set(T t) { this.t = t; }
        public T get() { return t; }
    }
    
    <br>
    
    <font color="#0000ff" style="line-height: 25px;" size="4">调用一个泛型的类：</font>
    
    <br>
    
    Box<Integer> integerBox;
    
    Box<Integer> integerBox = new Box<Integer>();
    
    <br>
    
    <font color="#0000ff" style="line-height: 25px;" size="4">包含多种类型的泛型类：</font>
    
    <br>
    
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
    
    <br>
    
    <span style="white-space:pre;">	</span><font color="#3366ff">调用：</font>
    
    Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
    Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
    
    <br>
    
    <span style="line-height: 22px;">	</span><font color="#3366ff" style="line-height: 22px;">调用“泛型”函数：</font>
    
    <strong>public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2)</strong> {。。。。}
    
    boolean same = Util.<strong><Integer, String></strong>compare(p1, p2);
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
