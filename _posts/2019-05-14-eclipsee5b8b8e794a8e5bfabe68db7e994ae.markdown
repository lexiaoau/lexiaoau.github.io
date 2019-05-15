---
author: lexiao
comments: true
date: 2019-05-14 01:35:13+00:00
layout: post
link: http://localhost/blog/?p=342
slug: eclipse%e5%b8%b8%e7%94%a8%e5%bf%ab%e6%8d%b7%e9%94%ae
title: eclipse常用快捷键
wordpress_id: 342
categories:
- java
---

### 1. Open Resource (ctrl+shift+r)




**** This one is probably the 
biggest time saver of all shortcuts. This shortcut allows you to open 
any file in your workspace by only typing the first letters of a file 
name or a mask, like applic*.xml. The only drawback of this shortcut is 
that it is not available in all perspectives.




### 2. Quick Outline (ctrl+o)




Do you want to view the methods of the current class or navigate to a
 specific method without having to scroll down the code or use the find 
function? _ctrl+o_ is the answer. It will list all methods and 
attributes of the current class and you can just start typing the name 
you want and then press enter to navigate directly to that method or 
attribute.




![ctrl+o Quick Outline](http://sites.google.com/site/summablogmedia/Home/ctrl%2Bo.gif)




### 3. Quick Switch Editor (ctrl+e)




This one will help you navigate through the open editors. You can use _ctrl+page down_ or _ctrl+page up_ to navigate to the next and previous tabs, but _ctrl+e_ will be more effective when you have a lot of files opened.




![ctrl+e Quick Switch Editor](http://sites.google.com/site/summablogmedia/Home/ctrl%2Be.gif)




### 4. Assign to local variable (ctrl+2, L)




**** During development I’m used to first write the 
method invocation, like Calendar.getInstance(), and then assign the 
return of the method to a local variable using this shortcut. It saves 
me the time to enter the class name, variable name and also the import 
declaration. The _ctrl+2, F_ is similar but will assign the return of a method to a field in the class.




### 5. Rename (alt+shift+r)




Renaming attributes and methods was a big deal years ago, using a 
combination of search and replace that frequently would lead to broken 
code. Every modern Java IDE today provides refactoring capabilities and 
Eclipse is not different. It is so easy to rename variables and methods 
names that once you get used to it you’ll be renaming it every time you 
think it is necessary or when a better name is recommended in code 
review sessions.  To use this shortcut just move the cursor to the 
attribute or method name and press _alt+shift+r_, enter the new 
name and press enter. Done! All references to this method in every class
 in the project will be renamed automatically. If you’re renaming an 
attribute of a class you can press _alt+shift+r_ twice, it will 
display the Refactor Dialog which allows you to automatically rename the
 gets and set methods also when refactoring an attribute.




### 6. Extract Local Variable (alt+shift+l) and Extract Method (alt+shift+m)




Refactoring also includes extracting variables and methods from 
blocks of code. These two shortcuts allows us to do exactly that. Do you
 want to create a constant from that String? Just select the text and 
hit _alt+shift+l_. If that same string is in other places on the 
same class it will automatically be replaced there. The Extract Method 
is also very helpful and clever enough to know (in most cases) the 
arguments that must be used when you want a section of your code to be 
exposed as a new method.  Breaking larger methods in smaller well 
defined methods is one of the best ways to reduce cyclomatic complexity 
and also to improve testability of your code.




### 7. shift+enter and ctrl+shift+enter




_Shift+enter_ adds a new blank line right above moving the 
cursor to the new line. The difference between a regular enter is that 
the cursor doesn’t need to be at the end of a line. The _ctrl+shift+enter_ does the same thing adding the line right before the current line.




### 8. Alt+arrow keys




This one is a big time saver too. It will move the current line or 
selection up or down. Very helpful when moving sections of code  in 
try/catch blocks.




### 9. ctrl+m




We all know how productive a big screen is and the ctrl+m shortcut will maximize the editor view.




### 10. Next Error (ctrl+.) and Quick Fix (ctrl+1)




_ ctrl+. _moves the cursor to the next error or warning in the current file. I use this almost always in conjunction with _ctrl+1_
 which is the suggestion shortcut. New versions of Eclipse are pretty 
clever on the suggestions, helping you sort out a lot of problems like 
missing arguments in methods, throw/catch exception, unimplemented 
methods, etc.
