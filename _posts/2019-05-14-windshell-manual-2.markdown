---
author: lexiao
comments: true
date: 2019-05-14 01:35:57+00:00
layout: post
link: http://localhost/blog/?p=361
slug: windshell-manual-2
title: Windshell Manual  -- 2
wordpress_id: 361
categories:
- 杂而无多
---

### _7.3  The Shell C-Expression Interpreter_

    

The C-expression interpreter is the  most common command interface to the Tornado shell. This interpreter can  evaluate almost any C expression interactively in the context of the  attached target. This includes the ability to use variables and  functions whose names are defined in the symbol table. Any command you  type is interpreted as a C expression. The shell evaluates that  expression and, if the expression so specifies, assigns the result to a  variable.

#### _7.3.1  Data Types_

    

The most significant difference  between the shell C-expression interpreter and a C compiler lies in the  way that they handle data types. The shell does not accept any C  declaration statements, and no data-type information is available in the  symbol table. Instead, an expression's type is determined by the types  of its terms.

    

Unless you use explicit type-casting, the shell makes the following assumptions about data types:

    

  * In an assignment statement, the type of the left hand side is determined by the type of the right hand side.

    

  * If floating-point numbers and integers both appear in an arithmetic expression, the resulting type is a floating-point number. 

    

  * Data types are assigned to various elements as shown in Table 7-9. 

    

Table 7-9: **Shell C Interpreter Data-Type Assumptions**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Element**

</td>
<td >

**Data Type**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

variable

</td>
<td >

**int**

</td></tr> <tr valign="top" >
<td >

variable used as floating-point

</td>
<td >

**double**

</td></tr> <tr valign="top" >
<td >

return value of subroutine

</td>
<td >

**int**

</td></tr> <tr valign="top" >
<td >

constant with no decimal point

</td>
<td >

**int**/**long**

</td></tr> <tr valign="top" >
<td >

constant with decimal point

</td>
<td >

**double**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

A constant or variable can be  treated as a different type than what the shell assumes by explicitly  specifying the type with the syntax of C type-casting. Functions that  return values other than integers require a slightly different  type-casting; see _Function Calls_. Table 7-10 shows the various data types available in the shell C interpreter, with examples of how they can be set and referenced. 

    

Table 7-10: **Data Types in the Shell C Interpreter**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Type**

</td>
<td >

**Bytes**

</td>
<td >

**Set Variable**

</td>
<td >

**Display Variable**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td rowspan="2" >

**int  **

</td>
<td rowspan="2" >

4** **

</td>
<td rowspan="2" >
    
    <a rel="nofollow"></a>x = 99      

</td>
<td rowspan="2" >
    
    <a rel="nofollow"></a>x  (int) x      

</td></tr> <tr valign="top" ></tr> <tr valign="top" >
<td rowspan="2" >

**long** ** **

</td>
<td rowspan="2" >

4** **

</td>
<td rowspan="2" >
    
    <a rel="nofollow"></a>x = 33  x = (long)33      

</td>
<td rowspan="2" >
    
    <a rel="nofollow"></a>x  (long) x      

</td></tr> <tr valign="top" ></tr> <tr valign="top" >
<td >

**short** ** **

</td>
<td >

2** **

</td>
<td >
    
    <a rel="nofollow"></a>x = (short)20      

</td>
<td >
    
    <a rel="nofollow"></a>(short) x      

</td></tr> <tr valign="top" >
<td rowspan="3" >

**char** ** **

</td>
<td rowspan="3" >

1** **

</td>
<td rowspan="3" >
    
    <a rel="nofollow"></a>x = 'A'  x = (char)65  x = (char)0x41      

</td>
<td rowspan="3" >
    
    <a rel="nofollow"></a>(char) x      

</td></tr> <tr valign="top" ></tr> <tr valign="top" ></tr> <tr valign="top" >
<td rowspan="4" >

**double** ** **

</td>
<td rowspan="4" >

8** **

</td>
<td rowspan="4" >
    
    <a rel="nofollow"></a>x = 11.2  x = (double)11.2      

</td>
<td rowspan="4" >
    
    <a rel="nofollow"></a>(double) x      

</td></tr> <tr valign="top" ></tr> <tr valign="top" ></tr> <tr valign="top" ></tr> <tr valign="top" >
<td >

**float** ** **

</td>
<td >

4** **

</td>
<td >
    
    <a rel="nofollow"></a>x = (float)5.42      

</td>
<td >
    
    <a rel="nofollow"></a>(float) x      

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

Strings, or character arrays, are  not treated as separate types in the shell C interpreter. To declare a  string, set a variable to a string value.**3** For example:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>ss = "shoe bee doo"</b></font>

    

The variable **ss** is a pointer to the string _shoe bee doo_. To display **ss**, enter:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d ss</b></font>

    

The **d( )** command displays memory where **ss** is pointing.**4** You can also use **printf( )** to display strings.

    

The shell places no type restrictions on the application of operators. For example, the shell expression:

    
    
    <a rel="nofollow"></a> *(70000 + 3 * 16)

    

evaluates to the 4-byte integer value at memory location 70048.

#### _7.3.2  Lines and Statements_

    

The shell parses and evaluates its  input one line at a time. A line may consist of a single shell statement  or several shell statements separated by semicolons. A semicolon is not  required on a line containing only a single statement. A statement  cannot continue on multiple lines. 

    

Shell statements are either C  expressions or assignment statements. Either kind of shell statement may  call WindSh commands or target routines.

#### _7.3.3  Expressions_

    

Shell expressions consist of literals, symbolic data references, function calls, and the usual C operators.

#### _Literals_

    

The shell interprets the literals in Table 7-11 in the same way as the C compiler, with one addition: the shell also allows hex numbers to be preceded by **$** instead of **0x**. 

    

Table 7-11: **Literals in the Shell C Interpreter**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Literal**

</td>
<td >

**Example**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

decimal numbers

</td>
<td >

143967 

</td></tr> <tr valign="top" >
<td >

octal numbers

</td>
<td >

017734 

</td></tr> <tr valign="top" >
<td >

hex numbers

</td>
<td >

0xf3ba or $f3ba 

</td></tr> <tr valign="top" >
<td >

floating point numbers

</td>
<td >

666.666 

</td></tr> <tr valign="top" >
<td >

character constants

</td>
<td >

'x' and '$' 

</td></tr> <tr valign="top" >
<td >

string constants

</td>
<td >

"hello world!!!" 

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

#### _Variable References_

    

Shell expressions may contain  references to variables whose names have been entered in the system  symbol table. Unless a particular type is specified with a variable  reference, the variable's value in an expression is the 4-byte value at  the memory address obtained from the symbol table. It is an error if an  identifier in an expression is not found in the symbol table, except in  the case of assignment statements discussed below.

    

Some C compilers prefix user-defined identifiers with an underbar, so that **myVar** is actually in the symbol table as **_myVar**.  In this case, the identifier can be entered either way to the  shell--the shell searches the symbol table for a match either with or  without a prefixed underbar.

    

You can also access data in memory  that does not have a symbolic name in the symbol table, as long as you  know its address. To do this, apply the C indirection operator "*" to a  constant. For example, ***0x10000** refers to the 4-byte integer value at memory address 10000 hex.

#### _Operators_

    

The shell interprets the operators in Table 7-12 in the same way as the C compiler. 

    

Table 7-12: **Operators in the Shell C Interpreter**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Operator Type**

</td>
<td colspan="6" >

**Operators**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

arithmetic

</td>
<td >

+ 

</td>
<td >

- 

</td>
<td >

* 

</td>
<td >

/ 

</td>
<td colspan="2" >

unary - 

</td></tr> <tr valign="top" >
<td >

relational

</td>
<td >

== 

</td>
<td >

!= 

</td>
<td >

<

</td>
<td >

>

</td>
<td >

<= 

</td>
<td >

>= 

</td></tr> <tr valign="top" >
<td >

shift

</td>
<td >

<<

</td>
<td >

>>

</td>
<td >

</td>
<td >

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

logical

</td>
<td >

|| 

</td>
<td >

&&

</td>
<td >

! 

</td>
<td >

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

bitwise

</td>
<td >

| 

</td>
<td >

&

</td>
<td >

~ 

</td>
<td >

^ 

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

address and indirection

</td>
<td >

&

</td>
<td >

* 

</td>
<td >

</td>
<td >

</td>
<td >

</td>
<td >

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

The shell assigns the same  precedence to the operators as the C compiler. However, unlike the C  compiler, the shell always evaluates both sub-expressions of the logical  binary operators || and &&.

#### _Function Calls_

    

Shell expressions may contain calls  to C functions (or C-compatible functions) whose names have been entered  in the system symbol table; they may also contain function calls to  WindSh commands that execute on the host.

    

The shell executes such function  calls in tasks spawned for the purpose, with the specified arguments and  default task parameters; if the task parameters make a difference, you  can call **taskSpawn( )** instead of calling  functions from the shell directly. The value of a function call is the  4-byte integer value returned by the function. The shell assumes that  all functions return integers. If a function returns a value other than  an integer, the shell must know the data type being returned before the  function is invoked. This requires a slightly unusual syntax because you  must cast the function, not its return value. For example:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>floatVar = ( float ()) funcThatReturnsAFloat (x,y)</b></font>

    

The shell can pass up to ten  arguments to a function. In fact, the shell always passes exactly ten  arguments to every function called, passing values of zero for any  arguments not specified. This is harmless because the C function-call  protocol handles passing of variable numbers of arguments. However, it  allows you to omit trailing arguments of value zero from function calls  in shell expressions.

    

Function calls can be nested. That is, a function call can be an argument to another function call. In the following example, **myFunc( )** takes two arguments: the return value from **yourFunc( )** and **myVal**. The shell displays the value of the overall expression, which in this case is the value returned from **myFunc( )**.

    
    
    <a rel="nofollow"></a>myFunc (yourFunc (yourVal), myVal);

    

Shell expressions can also contain  references to function addresses instead of function invocations. As in  C, this is indicated by the absence of parentheses after the function  name. Thus the following expression evaluates to the result returned by  the function **myFunc2( )** plus 4:

    
    
    <a rel="nofollow"></a>4 + myFunc2 ( )

    

However, the following expression evaluates to the address of **myFunc2( )** plus 4:

    
    
    <a rel="nofollow"></a>4 + myFunc2

    

An important exception to this  occurs when the function name is the very first item encountered in a  statement. This is discussed in _Arguments to Commands_.

    

Shell expressions can also contain  calls to functions that do not have a symbolic name in the symbol table,  but whose addresses are known to you. To do this, simply supply the  address in place of the function name. Thus the following expression  calls a parameterless function whose entry point is at address 10000  hex:

    
    
    <a rel="nofollow"></a>0x10000 ()

    

You can assign the address of a  function to a variable and then dereference the variable to call the  function as in the following example:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>aaa=printf  </b></font>-> (* aaa)("The clock speed is %d\n" ,sysClckRateGet())

#### _Subroutines as Commands_

    

Both VxWorks and the Tornado shell  itself provide routines that are meant to be called from the shell  interactively. You can think of these routines as _commands_, rather than as _subroutines_,  even though they can also be called with the same syntax as C  subroutines (and those that run on the target are in fact subroutines).  All the commands discussed in this chapter fall in this category. When  you see the word _command_, you can read _subroutine_, or vice versa, since their meaning here is identical.

#### _Arguments to Commands_

    

In practice, most statements input  to the shell are function calls, often to invoke VxWorks facilities. To  simplify this use of the shell, an important exception is allowed to the  standard expression syntax required by C. When a function name is the  very first item encountered in a shell statement, the parentheses  surrounding the function's arguments may be omitted. Thus the following  shell statements are synonymous:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>rename ("oldname", "newname")</b></font>

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>rename "oldname", "newname"</b></font>

    

as are:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>evtBufferAddress ( )</b></font>

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>evtBufferAddress</b></font>

    

However, note that if you wish to  assign the result to a variable, the function call cannot be the first  item in the shell statement--thus, the syntactic exception above does  not apply. The following captures the address, not the return value, of **evtBufferAddress( )**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>value = evtBufferAddress</b></font> 

#### _Task References_

    

Most VxWorks routines that take an  argument representing a task require a task ID. However, when invoking  routines interactively, specifying a task ID can be cumbersome since the  ID is an arbitrary and possibly lengthy number.

    

To accommodate interactive use,  shell expressions can reference a task by either task ID or task name.  The shell attempts to resolve a task argument to a task ID as follows:  if no match is found in the symbol table for a task argument, the shell  searches for the argument in the list of active tasks. When it finds a  match, it substitutes the task name with its matching task ID. In symbol  lookup, symbol names take precedence over task names. 

    

By convention, task names are prefixed with a _u_ for tasks started from the Tornado shell, and with a _t_ for VxWorks tasks started from the target itself. In addition, tasks started from a shell are prefixed by _s1_, _s2_,  and so on to indicate which shell they were started from. This avoids  name conflicts with entries in the symbol table. The names of system  tasks and the default task names assigned when tasks are spawned use  this convention. For example, tasks spawned with the shell command **sp( )** in the first shell opened are given names such as **s1u0** and **s1u1**. Tasks spawned with the second shell opened have names such as **s2u0** and **s2u1**. You are urged to adopt a similar convention for tasks named in your applications.

#### _7.3.4  The "Current" Task and Address_

    

A number of commands--**c( )**, **s( )**, **ti( )**--take a task parameter that can be omitted. If omitted, the _current task_ is used. The **l( )** and **d( )** commands use the _current address_ if no address is specified. The current task and address are set when:

    

  * A task hits a breakpoint or an exception trap.  The current address is the address of the instruction that caused the  break or exception.

    

  * A task is single-stepped. The current address is the address of the _next_ instruction to be executed.

    

  * Any of the commands that use the current task or  address are executed with a specific task parameter. The current  address will be the address of the byte _following_ the last byte that was displayed or disassembled.

#### _7.3.5  Assignments_

    

The shell C interpreter accepts assignment statements in the form:

    
    
    <a rel="nofollow"></a><i>addressExpression</i> = <i>expr<wbr>ession</i> 

    

The left side of an expression must evaluate to an addressable entity; that is, a legal C value. 

#### _Typing and Assignment_

    

The data type of the left side is  determined by the type of the right side. If the right side does not  contain any floating-point constants or noninteger type-casts, then the  type of the left side will be an integer. The value of the right side of  the assignment is put at the address provided by the left side. For  example, the following assignment sets the 4-byte integer variable **x** to 0x1000:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>x = 0x1000</b></font>

    

The following assignment sets the 4-byte integer value at memory address 0x1000 to the current value of **x**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>*0x1000 = x</b></font>

    

The following compound assignment adds 300 to the 4-byte integer variable **x**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>x += 300</b></font>

    

The following adds 300 to the 4-byte integer at address 0x1000:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>*0x1000 += 300</b></font>

    

The compound assignment operator -=, as well as the increment and decrement operators ++ and --, are also available.

#### _Automatic Creation of New Variables_

    

New variables can be created  automatically by assigning a value to an undefined identifier (one not  already in the symbol table) with an assignment statement.

    

When the shell encounters such an  assignment, it allocates space for the variable and enters the new  identifier in the symbol table along with the address of the newly  allocated variable. The new variable is set to the value and type of the  right-side expression of the assignment statement. The shell prints a  message indicating that a new variable has been allocated and assigned  the specified value.

    

For example, if the identifier **fd** is not currently in the symbol table, the following statement creates a new variable named **fd** and assigns to it the result of the function call:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>fd = open ("file", 0)</b></font> 

#### _7.3.6  Comments_

    

The shell allows two kinds of  comments. First, comments of the form /* */ can be included anywhere on a  shell input line. These comments are simply discarded, and the rest of  the input line evaluated as usual. Second, any line whose first nonblank  character is # is ignored completely. Comments are particularly useful  for Tornado shell scripts. See the section _Scripts: Redirecting Shell I/O_ below.

#### _7.3.7  Strings_

    

When the shell encounters a string  literal ("") in an expression, it allocates space for the string  including the null-byte string terminator. The value of the literal is  the address of the string in the newly allocated storage. For instance,  the following expression allocates 12 bytes from the target-agent memory  pool, enters the string in those 12 bytes (including the null  terminator), and assigns the address of the string to **x**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>x = "hello there"</b></font>

    

Furthermore, even when a string  literal is not assigned to a symbol, memory is still permanently  allocated for it. For example, the following uses 12 bytes of memory  that are never freed:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>printf ("hello there")</b></font>

    

If strings were only temporarily  allocated, and a string literal were passed to a routine being spawned  as a task, then by the time the task executed and attempted to access  the string, the shell would have already released--possibly even  reused--the temporary storage where the string was held.

    

This memory, like other memory used  by the Tornado tools, comes from the target-agent memory pool; it does  not reduce the amount of memory available for application execution (the  VxWorks memory pool). The amount of target memory allocated for each of  the two memory pools is defined at configuration time; see _Scaling the Target Agent_. 

    

After extended development sessions  in Tornado shells, the cumulative memory used for strings may be  noticeable. If this becomes a problem, restart your target server.

#### _7.3.8  Strings and Path Names_

    

In VxWorks, the directory and file  segments of path names (for target-resident files and devices) are  separated with the slash character (**/**). This presents no difficulty when subroutines require a path-name argument, because the **/** character has no special meaning in C strings. 

    

However, you can also refer from the  shell to files that reside on your Windows host. For host path names,  you can use either a slash for consistency with the VxWorks convention,  or a backslash (**\**) for consistency with the Windows convention.

    

Because the backslash character is  an escape character in C strings, you must double any backslashes that  you use in path names as strings. This applies only to path names in C  strings. No special syntax is required for path names that are  interpreted directly by the shell.

    

The shell's **ld( )** command (_System Modification and Debugging_) can be used with all of these variations of path names. The following **ld( )** invocations are all correct and equivalent:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>ld < c:\fred\tests\zap.o</b></font>

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>ld < c:/fred/tests/zap.o</b></font>

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>ld 1,0,"c:\\fred\\tests\\zap.o"</b></font>

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>ld 1,0,"c:/fred/tests/zap.o"</b></font>

#### _7.3.9  Ambiguity of Arrays and Pointers_

    

In a C expression, a nonsubscripted  reference to an array has a special meaning, namely the address of the  first element of the array. The shell, to be compatible, should use the  address obtained from the symbol table as the value of such a reference,  rather than the contents of memory at that address. Unfortunately, the  information that the identifier is an array, like all data type  information, is not available after compilation. For example, if a  module contains the following:

    
    
    <a rel="nofollow"></a>char string [ ] = "hello";

    

you might be tempted to enter a shell expression like:

    
    
    <a rel="nofollow"></a>?    -> <font color="#00a000"><b>printf (string)</b></font>

    

While this would be correct in C, the shell will pass the first 4 bytes of the string itself to **printf( )**,  instead of the address of the string. To correct this, the shell  expression must explicitly take the address of the identifier:

    
    
    <a rel="nofollow"></a>?    -> <font color="#00a000"><b>printf (&string)</b></font>

    

To make matters worse, in C if the identifier had been declared a character pointer instead of a character array:

    
    
    <a rel="nofollow"></a>char *string = "hello";

    

then to a compiler ? would be  correct and ? would be wrong! This is especially confusing since C  allows pointers to be subscripted exactly like arrays, so that the value  of **string[0]** would be "h" in either of the above declarations.

    

The moral of the story is that array  references and pointer references in shell expressions are different  from their C counterparts. In particular, array references require an  explicit application of the address operator &.

#### _7.3.10  Pointer Arithmetic_

    

While the C language treats pointer  arithmetic specially, the shell C interpreter does not, because it  treats all non-type-cast variables as 4-byte integers.

    

In the shell, pointer arithmetic is  no different than integer arithmetic. Pointer arithmetic is valid, but  it does not take into account the size of the data pointed to. Consider  the following example:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>*(myPtr + 4) = 5</b></font>

    

Assume that the value of **myPtr** is 0x1000. In C, if **myPtr** is a pointer to a type **char**, this would put the value 5 in the byte at address at 0x1004. If **myPtr**  is a pointer to a 4-byte integer, the 4-byte value 0x00000005 would go  into bytes 0x1010-0x1013. The shell, on the other hand, treats variables  as integers, and therefore would put the 4-byte value 0x00000005 in  bytes 0x1004-0x1007.

#### _7.3.11  C Interpreter Limitations_

    

Powerful though it is, the C  interpreter in the shell is not a complete interpreter for the C  language. The following C features are _not_ present in the Tornado shell:

** **
  * **Control Structures**

    

The shell interprets only C _expressions_ (and comments). The shell does not support C control structures such as **if**, **goto**, and **switch** statements, or **do**, **while**, and **for**  loops. Control structures are rarely needed during shell interaction.  If you do come across a situation that requires a control structure, you  can use the Tcl interface to the shell instead of using its C  interpreter directly; see _7.7 Tcl: Shell Interpretation_.

** **
  * **Compound or Derived Types**

    

No compound types (**struct** or **union** types) or derived types (**typedef**)  are recognized in the shell C interpreter. You can use CrossWind  instead of the shell for interactive debugging, when you need to examine  compound or derived types. 

** **
  * **Macros**

    

No C preprocessor macros (or any  other preprocessor facilities) are available in the shell. CrossWind  does not support preprocessor macros either, but indirect workarounds  are available using either the shell or CrossWind. For constant macros,  you can define variables in the shell with similar names to the macros.  To avoid intrusion into the application symbol table, you can use  CrossWind instead; in this case, use CrossWind convenience variables  with names corresponding to the desired macros. In either case, you can  automate the effort of defining any variables you need repeatedly, by  using an initialization script.

    

For the first two problems (control  structures, or display and manipulation of types that are not supported  in the shell), you might also consider writing auxiliary subroutines to  provide these services during development; you can call such subroutines  at will from the shell, and later omit them from your final  application.

#### _7.3.12  C-Interpreter Primitives_

    

Table 7-13 lists all the primitives (commands) built into WindSh. (For discussion of these primitives by function, see _7.2.4 Invoking Built-In Shell Routines_.)  Because the shell tries to find a primitive first before attempting to  call a target subroutine, it is best to avoid these names in the target  code. If you do have a name conflict, however, you can force the shell  to call a target routine instead of an identically-named primitive by  prefacing the subroutine call with the character @. (See _Resolving Name Conflicts between Host and Target_.) 

    

Table 7-13: **List of WindSh Commands **

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**agentModeShow**( ) 

**b**( ) 

**bd**( ) 

**bdall**( ) 

**bh**( ) 

**bootChange**( ) 

**browse**( ) 

**c**( ) 

**cd**( ) 

</td>
<td >

**ipstatShow**( ) 

**iStrict**( ) 

**l**( ) 

**ld**( ) 

**lkAddr**( ) 

**lkup**( ) 

**ls**( ) 

**m**( ) 

**memPartShow**( ) 

</td>
<td >

**smMemPartShow**( ) 

**smMemShow**( ) 

**so**( ) 

**sp**( ) 

**sps**( ) 

**sysResume**( ) 

**sysStatusShow**( ) 

**sysSuspend**( ) 

**taskCreateHookShow**( ) 

</td></tr> <tr valign="top" >
<td >

**checkStack**( ) 

**classShow**( ) 

**cplusCtors**( ) 

**cplusDtors**( ) 

**cplusStratShow**( ) 

**cplusXtorSet**( ) 

**cret**( ) 

**d**( ) 

**devs**( ) 

**h**( ) 

**help**( ) 

**hostShow**( ) 

**i**( ) 

**icmpstatShow**( ) 

**ifShow**( ) 

**inetstatShow**( ) 

**intVecShow**( ) 

**iosDevShow**( ) 

**iosDrvShow**( )

**iosFdShow**( ) 

</td>
<td >

**memShow**( ) 

**moduleIdFigure**( ) 

**moduleShow**( ) 

**mqPxShow**( ) 

**mRegs**( ) 

**msgQShow**( ) 

**period**( ) 

**printErrno**( ) 

**printLogo**( ) 

**pwd**( ) 

**quit**( ) 

**reboot**( ) 

**repeat**( ) 

**routestatShow**( ) 

**s**( ) 

**semPxShow**( ) 

**semShow**( ) 

**shellHistory**( ) 

**shellPromptSet**( ) 

**show**( ) 

</td>
<td >

**taskDeleteHookShow**( ) 

**taskIdDefault**( ) 

**taskIdFigure**( ) 

**taskRegsShow**( ) 

**taskShow**( ) 

**taskSwitchHookShow**( ) 

**taskWaitShow**( ) 

**tcpstatShow**( ) 

**td**( ) 

**tftpInfoShow**( ) 

**ti**( ) 

**tr**( ) 

**ts**( ) 

**tt**( ) 

**tw**( ) 

**udpstatShow**( ) 

**unld**( ) 

**version**( ) 

**w**( ) 

**wdShow**( ) 

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

#### _7.3.13  Terminal-Control Characters_

    

The terminal-control characters are  slightly different when WindSh runs as a console-based application and  when it runs within Tornado.

    

Table 7-14  lists special terminal characters frequently used for shell control in  both situations. For more information on the use of these characters,  see _7.5 Shell Line Editing_ and _7.2.8 Interrupting a Shell Command_.

    

Table 7-14: **Shell Special Characters**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Tornado Value**

</td>
<td >

**Console Values**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**BACKSPACE** (**CTRL+H**) 

</td>
<td >

**BACKSPACE** (**CTRL+H**)

</td>
<td >

Delete a character.

</td></tr> <tr valign="top" >
<td >

**CTRL+U**

</td>
<td >

**CTRL+U**

</td>
<td >

Delete an entire line.

</td></tr> <tr valign="top" >
<td >

**CTRL+BREAK**

</td>
<td >

**CTRL+BREAK**

</td>
<td >

Interrupt a function call.

</td></tr> <tr valign="top" >
<td >

n/a 

</td>
<td >

**CTRL+S**

</td>
<td >

Temporarily suspend output (UNIX only).

</td></tr> <tr valign="top" >
<td >

n/a 

</td>
<td >

**CTRL+Q**

</td>
<td >

Resume output (UNIX only).

</td></tr> <tr valign="top" >
<td >

**ESC**

</td>
<td >

**ESC**

</td>
<td >

Toggle between input mode and edit mode.

</td></tr> <tr valign="top" >
<td >

**CTRL+C**

</td>
<td >

n/a 

</td>
<td >

Copy selection to the clipboard.

</td></tr> <tr valign="top" >
<td >

**CTRL+V**

</td>
<td >

n/a 

</td>
<td >

Paste text from the clipboard.

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

#### _7.3.14  Redirection in the C Interpreter_

    

The shell provides a _redirection_  mechanism for momentarily reassigning the standard input and standard  output file descriptors just for the duration of the parse and  evaluation of an input line. The redirection is indicated by the <  and > symbols followed by file names, at the very end of an input  line. No other syntactic elements may follow the redirection  specifications. The redirections are in effect for all subroutine calls  on the line.

    

For example, the following input line sets standard input to the file named **input** and standard output to the file named **output** during the execution of **copy( )**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>copy < input > output</b></font>

    

If the file to which standard output is redirected does not exist, it is created.

#### _Ambiguity Between Redirection and C Operators_

    

There is an ambiguity between redirection specifications and the relational operators _less than_ and _greater than_.  The shell always assumes that an ambiguous use of < or >  specifies a redirection rather than a relational operation. Thus the  ambiguous input line:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>x > y</b></font>

    

writes the value of the variable **x** to the stream named **y**, rather than comparing the value of variable **x** to the value of variable **y**.  However, you can use a semicolon to remove the ambiguity explicitly,  because the shell requires that the redirection specification be the  last element on a line. Thus the following input lines are unambiguous:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>x; > y</b></font>

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>x > y;</b></font>

    

The first line prints the value of the variable **x** to the output stream **y**. The second line prints on standard output the value of the expression "**x** greater than **y**."

#### _The Nature of Redirection_

    

The redirection mechanism of the  Tornado shell is fundamentally different from that of the Windows  command shell, although the syntax and terminology are similar.

    

In the Tornado shell, redirecting  input or output affects only a command executed from the shell. In  particular, this redirection is not inherited by any tasks started while  output is redirected.

    

For example, you might be tempted to  specify redirection streams when spawning a routine as a task,  intending to send the output of **printf( )** calls  in the new task to an output stream, while leaving the shell's I/O  directed at the virtual console. This stratagem does not work. For  example, the shell input line:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>taskSpawn (...myFunc...) > output</b></font>

    

momentarily redirects the shell  standard output during the brief execution of the spawn routine, but  does not affect the I/O of the resulting task.

    

To redirect the input or output streams of a particular task, call **ioTaskStdSet( )** once the task exists.

#### _Scripts: Redirecting Shell I/O_

    

A special case of I/O redirection  concerns the I/O of the shell itself; that is, redirection of the  streams the shell's input is read from, and its output is written to.  The syntax for this is simply the usual redirection specification, on a  line that contains no other expressions. 

    

The typical use of this mechanism is to have the shell read and execute lines from a file. For example, the input lines:

    
    
    <a rel="nofollow"></a>?    -> <font color="#00a000"><b><startup</b></font>

    
    
    <a rel="nofollow"></a>?    -> <font color="#00a000"><b>< c:\fred\startup</b></font>

    

cause the shell to read and execute the commands in the file **startup**, either on the current working directory as in ? or explicitly on the complete path name in ?. If your working directory is **\fred**, commands ? and ? are equivalent.

    

Such command files are called _scripts_.  Scripts are processed exactly like input from an interactive terminal.  After reaching the end of the script file, the shell returns to  processing I/O from the original streams.

    

During execution of a script, the  shell displays each command as well as any output from that command. You  can change this by invoking the shell with the **-q** option; see the **windsh** reference entry (online or in _. _).

    

An easy way to create a shell script is from a list of commands you have just executed in the shell. The history command **h( )** prints a list of the last 20 shell commands. The following creates a file **c:\tmp\script** with the current shell history:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>h > c:\tmp\script</b></font>

    

The command numbers must be deleted from this file before using it a shell script. 

    

Scripts can also be nested. That is, scripts can contain shell input redirections that cause the shell to process other scripts.

    

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**CAUTION: **Input  and output redirection must refer to files on a host file system. If  you have a local file system on your target, files that reside there are  available to target-resident subroutines, but not to the shell or to  other Tornado tools (unless you export them from the target using NFS,  and mount them on your host).

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**CAUTION: **You should set the WindSh environment variable **SH_GET_TASK_IO**  to off before you use redirection of input from scripts or before you  copy and paste blocks of commands to the shell command line. Otherwise  commands might be taken as input for a command that precedes them, and  lost. 

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

#### _C-Interpreter Startup Scripts_

    

Tornado shell scripts can be  especially useful for setting up your working environment. You can run a  startup script through the shell C interpreter**5** by specifying its name with the **-s** command-line option to **windsh**. For example:

    
    
    <a rel="nofollow"></a>C:\> <font color="#00a000"><b>windsh -s c:\fred\startup</b></font>

    

Like the **autoexec.bat**  file, startup scripts can be used for setting system parameters to  personal preferences: defining variables, specifying the target's  working directory, and so forth. They can also be useful for tailoring  the configuration of your system without having to rebuild VxWorks. For  example:

    

  * creating additional devices
    

  * loading and starting up application modules
    

  * adding a complete set of network host names and routes
    

  * setting NFS parameters and mounting NFS partitions

    

For additional information on initialization scripts, see _7.7 Tcl: Shell Interpretation_. 

  
  


### _7.4  C++ Interpretation_

    

Tornado supports both C and C++ as development languages; see _VxWorks Programmer's Guide: C++ Development_  for information about C++ development. Because C and C++ expressions  are so similar, the WindSh C-expression interpreter supports many C++  expressions. The facilities explained in _7.3 The Shell C-Expression Interpreter_  are all available regardless of whether your source language is C or  C++. In addition, there are a few special facilities for C++ extensions.  This section describes those extensions.

    

However, WindSh is not a complete  interpreter for C++ expressions. In particular, the shell has no  information about user-defined types; there is no support for the **::**  operator; constructors, destructors, and operator functions cannot be  called directly from the shell; and member functions cannot be called  with the **.** or **->** operators. 

    

To exercise C++ facilities that are  missing from the C-expression interpreter, you can compile and download  routines that encapsulate the special C++ syntax. Fortunately, the  Tornado dynamic linker makes this relatively painless.

#### _7.4.1  Overloaded Function Names_

    

If you have several C++ functions  with the same name, distinguished by their argument lists, call any of  them as usual with the name they share. When the shell detects the fact  that several functions exist with the specified name, it lists them in  an interactive dialogue, printing the matching functions' signatures so  that you can recall the different versions and make a choice among them.

    

You make your choice by entering the  number of the desired function. If you make an invalid choice, the list  is repeated and you are prompted to choose again. If you enter 0  (zero), the shell stops evaluating the current command and prints a  message like the following, with _xxx_ replaced by the function name you entered:

    
    
    <a rel="nofollow"></a>undefined symbol: <i>xxx</i>

    

This can be useful, for example, if  you misspelled the function name and you want to abandon the interactive  dialogue. However, because WindSh is an interpreter, portions of the  expression may already have executed (perhaps with side effects) before  you abandon execution in this way.

    

The following example shows how the  support for overloaded names works. In this example, there are four  versions of a function called **xmin( )**. Each version of **xmin( )** returns at least two arguments, but each version takes arguments of different types.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>l xmin  </b></font>"xmin" is overloaded - Please select:      1: _xmin(double,double)      2: _xmin(long,long)      3: _xmin(int,int)      4: _xmin(float,float)  Enter <number> to select, anything else to stop: 1                  _xmin(double,double):  3fe710 4e56 0000        LINK    .W      A6,#0  3fe714 f22e 5400 0008   FMOVE   .D      (0x8,A6),F0  3fe71a f22e 5438 0010   FCMP    .D      (0x10,A6),F0  3fe720 f295 0008        FB      .W      #0x8f22e  3fe724 f22e 5400 0010   FMOVE   .D      (0x10,A6),F0  3fe72a f227 7400        FMOVE   .D      F0,-(A7)  3fe72e 201f             MOVE    .L      (A7)+,D0  3fe730 221f             MOVE    .L      (A7)+,D1  3fe732 6000 0002        BRA             0x003fe736  3fe736 4e5e             UNLK            A6  value = 4187960 = 0x3fe738 = _xmin(double,double) + 0x28

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>l xmin  </b></font>"xmin" is overloaded - Please select:      1: _xmin(double,double)      2: _xmin(long,long)      3: _xmin(int,int)      4: _xmin(float,float)  Enter <number> to select, anything else to stop: 3                  _xmin(int,int):  3fe73a 4e56 0000        LINK    .W      A6,#0  3fe73e 202e 0008        MOVE    .L      (0x8,A6),D0  3fe742 b0ae 000c        CMP     .L      (0xc,A6),D0  3fe746 6f04             BLE             0x003fe74c  3fe748 202e 000c        MOVE    .L      (0xc,A6),D0  3fe74c 6000 0002        BRA             0x003fe750  3fe750 4e5e             UNLK            A6  3fe752 4e75             RTS                  _xmin(long,long):  3fe7544e560000          LINK    .W      A6,#0  3fe758202e0008          MOVE    .L      (0x8,A6),D0  value = 4187996 = 0x3fe75c = _xmin(long,long) + 0x8

    

In this example, the disassembler is called to list the instructions for **xmin( )**, then the version that computes the minimum of two **double** values is selected. Next, the disassembler is invoked again, this time selecting the version that computes the minimum of two **int** values. Note that a different routine is disassembled in each case.

#### _7.4.2  Automatic Name Demangling _

    

Many shell debugging and system information functions display addresses symbolically (for example, the **l( )**  routine). This might be confusing for C++, because compilers encode a  function's class membership (if any) and the type and number of the  function's arguments in the function's linkage name. The encoding is  meant to be efficient for development tools, but not necessarily  convenient for human comprehension. This technique is commonly known as _name mangling_ and can be a source of frustration when the mangled names are exposed to the developer.

    

To avoid this confusion, the  debugging and system information routines in WindSh print C++ function  names in a demangled representation. Whenever the shell prints an  address symbolically, it checks whether the name has been mangled. If it  has, the name is demangled (complete with the function's class name, if  any, and the type of each of the function's arguments) and printed.

    

The following example shows the demangled output when **lkup( )** displays the addresses of the **xmin( )** functions mentioned in _7.4.1 Overloaded Function Names_.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>lkup "xmin"  </b></font>_xmin(double,double)    0x003fe710 text         (templex.out)  _xmin(long,long)        0x003fe754 text         (templex.out)  _xmin(int,int)          0x003fe73a text         (templex.out)  _xmin(float,float)      0x003fe6ee text         (templex.out)  value = 0 = 0x0 

  
  


### _7.5  Shell Line Editing_

    

The WindSh front end provides a  history mechanism similar to the UNIX Korn-shell history facility,  including a built-in line editor (with keystrokes similar to the UNIX  editor **vi**) that allows you to scroll, search, and  edit previously typed commands. Line editing is available regardless of  which interpreter you are using (C or Tcl**6**  ), and the command history spans both interpreters--you can switch from  one to the other and back, and scroll through the history of both  modes.

    

The ESC key switches the shell from normal input mode to _edit mode_. The history and editing commands in Table 7-15 are available in edit mode.

    

Some line-editing commands switch the line editor to insert mode until an ESC is typed (as in **vi**) or until an ENTER gives the line to one of the shell interpreters. ENTER always gives the line as input to the current shell interpreter, from either input or edit mode.

    

In input mode, the shell history command **h( )** (described in _System Information_)  displays up to 20 of the most recent commands typed to the shell; older  commands are lost as new ones are entered. You can change the number of  commands kept in history by running **h( )** with a numeric argument. To locate a line entered previously, press ESC followed by one of the search commands listed in Table 7-15; you can then edit and execute the line with one of the commands from Table 7-15. 

    

Table 7-15: **Shell Line-Editing Commands **

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td colspan="3" >

**Basic Control**

</td></tr> <tr valign="top" >
<td >

**h** [_size_] 

</td>
<td >

</td>
<td >

Display shell history if no argument; otherwise set history buffer to _size_.

</td></tr> <tr valign="top" >
<td >

**ESC**

</td>
<td >

</td>
<td >

Switch to line-editing mode from regular input mode.

</td></tr> <tr valign="top" >
<td >

**ENTER**

</td>
<td >

</td>
<td >

Give line to shell and leave edit mode.

</td></tr> <tr valign="top" >
<td >

**CTRL+D**

</td>
<td >

</td>
<td >

Complete symbol or  path name (edit mode), display synopsis of current symbol (symbol must  be complete, followed by a space), or end shell session (if the command  line is empty).

</td></tr> <tr valign="top" >
<td >

[**tab**]

</td>
<td >

</td>
<td >

Complete symbol or path name (edit mode).

</td></tr> <tr valign="top" >
<td >

**CTRL+H**

</td>
<td >

</td>
<td >

Delete a character (backspace).

</td></tr> <tr valign="top" >
<td >

**CTRL+U**

</td>
<td >

</td>
<td >

Delete entire line (edit mode).

</td></tr> <tr valign="top" >
<td >

**CTRL+L**

</td>
<td >

</td>
<td >

Redraw line (works in edit mode).

</td></tr> <tr valign="top" >
<td colspan="2" >

**CTRL+S** and **CTRL+Q**

</td>
<td >

Suspend output, and resume output.

</td></tr> <tr valign="top" >
<td >

**CTRL+W**

</td>
<td >

</td>
<td >

Display HTML reference page for a routine. 

</td></tr> <tr valign="top" >
<td colspan="3" >

**Movement and Search Commands**

</td></tr> <tr valign="top" >
<td >

_n_**G**

</td>
<td >

</td>
<td >

Go to command number _n_.**1**

</td></tr> <tr valign="top" >
<td >

/_s_ or ?_s_

</td>
<td >

</td>
<td >

Search for string _s_ backward in history, or forward.

</td></tr> <tr valign="top" >
<td >

**n**

</td>
<td >

</td>
<td >

Repeat last search.

</td></tr> <tr valign="top" >
<td >

_n_**k** or n**-**

</td>
<td >

</td>
<td >

Get _n_th previous shell command.*

</td></tr> <tr valign="top" >
<td >

_n_**j** or n**+**

</td>
<td >

</td>
<td >

Get _n_th next shell command.*

</td></tr> <tr valign="top" >
<td >

_n_**h**

</td>
<td >

</td>
<td >

Go left _n_ characters (also **CTRL+H**).*

</td></tr> <tr valign="top" >
<td >

_n_**l** or **SPACE**

</td>
<td >

</td>
<td >

Go right _n_ characters.*

</td></tr> <tr valign="top" >
<td >

_n_**w** or n**W**

</td>
<td >

</td>
<td >

Go _n_ words forward, or _n_ large words. ***2**

</td></tr> <tr valign="top" >
<td >

_n_**e** or n**E**

</td>
<td >

</td>
<td >

Go to end of the _n_th next word, or _n_th next large word. *

</td></tr> <tr valign="top" >
<td >

_n_**b** or n**B**

</td>
<td >

</td>
<td >

Go back _n_ words, or _n_ large words.*

</td></tr> <tr valign="top" >
<td >

**$**

</td>
<td >

</td>
<td >

Go to end of line.

</td></tr> <tr valign="top" >
<td >

**0** or ^ 

</td>
<td >

</td>
<td >

Go to beginning of line, or first nonblank character.

</td></tr> <tr valign="top" >
<td >

**f**_c_ or **F**_c_

</td>
<td >

</td>
<td >

Find character _c_, searching forward, or backward.

</td></tr> <tr valign="top" >
<td colspan="3" >

**Insert and Change Commands**

</td></tr> <tr valign="top" >
<td >

**a** or **A**

</td>
<td >

...**ESC**

</td>
<td >

Append, or append at end of line (**ESC** ends input).

</td></tr> <tr valign="top" >
<td >

**i** or **I**

</td>
<td >

...**ESC**

</td>
<td >

Insert, or insert at beginning of line (**ESC** ends input).

</td></tr> <tr valign="top" >
<td >

_n_**s**

</td>
<td >

...**ESC**

</td>
<td >

Change _n_ characters (**ESC** ends input).*

</td></tr> <tr valign="top" >
<td >

_n_**c** **SPACE**

</td>
<td >

...**ESC**

</td>
<td >

Change _n_ characters (**ESC** ends input).*

</td></tr> <tr valign="top" >
<td >

**cw**

</td>
<td >

...**ESC**

</td>
<td >

Change word (**ESC** ends input).

</td></tr> <tr valign="top" >
<td >

**cc** or **S**

</td>
<td >

...**ESC**

</td>
<td >

Change entire line (**ESC** ends input).

</td></tr> <tr valign="top" >
<td >

**c$** or **C**

</td>
<td >

...**ESC**

</td>
<td >

Change from cursor to end of line (**ESC** ends input).

</td></tr> <tr valign="top" >
<td >

**c0**

</td>
<td >

...**ESC**

</td>
<td >

Change from cursor to beginning of line (**ESC** ends input).

</td></tr> <tr valign="top" >
<td >

**R**

</td>
<td >

...**ESC**

</td>
<td >

Type over characters (**ESC** ends input).

</td></tr> <tr valign="top" >
<td >

_n_**r**_c_

</td>
<td >

</td>
<td >

Replace the following _n_ characters with _c_.*

</td></tr> <tr valign="top" >
<td >

**~**

</td>
<td >

</td>
<td >

Toggle case, lower to upper or vice versa.

</td></tr> <tr valign="top" >
<td colspan="3" >

**Delete Commands**

</td></tr> <tr valign="top" >
<td >

_n_**x**

</td>
<td >

</td>
<td >

Delete _n_ characters starting at cursor.*

</td></tr> <tr valign="top" >
<td >

_n_**X**

</td>
<td >

</td>
<td >

Delete _n_ character to left of cursor.*

</td></tr> <tr valign="top" >
<td >

**dw**

</td>
<td >

</td>
<td >

Delete word.

</td></tr> <tr valign="top" >
<td >

**dd**

</td>
<td >

</td>
<td >

Delete entire line (also **CTRL+U**).

</td></tr> <tr valign="top" >
<td >

**d$** or **D**

</td>
<td >

</td>
<td >

Delete from cursor to end of line.

</td></tr> <tr valign="top" >
<td >

**d0**

</td>
<td >

</td>
<td >

Delete from cursor to beginning of line.

</td></tr> <tr valign="top" >
<td colspan="3" >

**Put and Undo Commands**

</td></tr> <tr valign="top" >
<td >

**p** or **P**

</td>
<td >

</td>
<td >

Put last deletion after cursor, or in front of cursor.

</td></tr> <tr valign="top" >
<td >

**u**

</td>
<td >

</td>
<td >

Undo last command.

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >

1: The default value for _n_ is 1.

2: _words_ are separated by blanks or punctuation; _large words_ are separated by blanks only.

</td></tr></table>

  
  


### _7.6  Object Module Load Path_

    

In order to download an object  module dynamically to the target, both WindSh and the target server must  be able to locate the file. If path naming conventions are different  between WindSh and the target server, the two systems may both have  access to the file, but mounted with different path names. This  situation arises often in environments where UNIX and Windows systems  are networked together, because the path naming convention is different:  the UNIX **/usr/fred/applic.o** may well correspond to the Windows **n:\fred\applic.o**. If you encounter this problem, check to be sure the **LD_SEND_MODULES** variable of **shConfig** is set to "on" or use the **LD_PATH** facility to tell the target server about the path known to the shell.

    

Example 7-3: **Loading a Module: Alternate Path Names**

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>ld < /usr/david/project/test/test.o  </b></font>Loading /usr/david/project/test/test.o  WTX Error 0x2 (no such file or directory)  value = -1 = 0xffffffff  -> <font color="#00a000"><b>?shConfig LD_PATH "/usr/david/project/test;C:\project\test"  </b></font>-> <font color="#00a000"><b>ld < test.o  </b></font>Loading C:\project\test\test.o  value = 17427840 = 0x109ed80

    

For more information on using **LD_PATH **and other **shConfig** facilities, see _WindSh Environment Variables_. 

    

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**CAUTION: **If you call **ld( )** with an explicit argument list, any instances of the backslash character in Windows paths must be doubled: **"****n:\\fred\\applic.o****"**. If you supply the module name with the redirection symbol instead, no double backslashes are necessary.

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

Certain WindSh commands and browser  utilities imply dynamic downloads of auxiliary target-resident code.  These subroutines fail in situations where the shell and target-server  view of the file system is incompatible. To get around this problem,  download the required routines explicitly from the host where the target  server is running (or configure the routines statically into the  VxWorks image). Once the supporting routines are on the target, any host  can use the corresponding shell and browser utilities. Table 7-16 lists the affected utilities. The object modules are in **wind\target\lib\obj**_cputype_**gnuvx**. 

    

Table 7-16: **Shell and Browser Utilities with Target-Resident Components**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Utility**

</td>
<td >

**Supporting Module**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**repeat( )**

</td>
<td >

**repeatHost.o**

</td></tr> <tr valign="top" >
<td >

**period( )**

</td>
<td >

**periodHost.o**

</td></tr> <tr valign="top" >
<td >

**tt( )**

</td>
<td >

**trcLib.o**,   
**ttHostLib.o**

</td></tr> <tr valign="top" >
<td >

Browser spy panel

</td>
<td >

**spyLib.o**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

  
  


### _7.7  Tcl: Shell Interpretation_

    

The shell has a Tcl interpreter  interface as well as the C interpreter interface. This section  illustrates some uses of the shell Tcl interpreter. If you are not  familiar with Tcl, we suggest you skip this section and return to it  after you have gotten acquainted with Tcl. (For an outline of Tcl, see _C. Tcl_.) In the interim, you can do a great deal of development work with the shell C interpreter alone. 

    

To toggle between the Tcl interpreter and the C interpreter in the shell, type the single character** ****?**. The shell prompt changes to remind you of the interpreter state: the prompt **->** indicates the C interpreter is listening, and the prompt **tcl>** indicates the Tcl interpreter is listening.**7**  For example, in the following interaction we use the C interpreter to  define a variable in the symbol table, then switch into the Tcl  interpreter to define a similar Tcl variable in the shell itself, and  finally switch back to the C interpreter:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>hello="hi there"  </b></font>new symbol "hello" added to symbol table.  hello = 0x3616e8: value = 3544824 = 0x3616f8 = hello + 0x10  -> <font color="#00a000"><b>?  </b></font>tcl> <font color="#00a000"><b>set hello {hi there}  </b></font>hi there  tcl> <font color="#00a000"><b>?  </b></font>-> 

    

If you start **windsh** from the Windows command line, you can also use the option **-Tclmode** (or **-T**) to start with the Tcl interpreter rather than the C interpreter.

    

Using the shell's Tcl interface  allows you to extend the shell with your own procedures, and also  provides a set of control structures which you can use interactively.  The Tcl interpreter also acts as a host shell, giving you access to  Windows command-line utilities on your development host.

#### _7.7.1  Tcl: Controlling the Target_

    

In the Tcl interpreter, you can  create custom commands, or use Tcl control structures for repetitive  tasks, while using the building blocks that allow the C interpreter and  the WindSh commands to control the target remotely. These building  blocks as a whole are called the **wtxtcl** procedures. 

    

For example, **wtxMemRead**  returns the contents of a block of target memory (given its starting  address and length). That command in turn uses a special memory-block  datatype designed to permit memory transfers without unnecessary Tcl  data conversions. The following example uses **wtxMemRead**, together with the memory-block routine **memBlockWriteFile**,  to write a Tcl procedure that dumps target memory to a host file.  Because almost all the work is done on the host, this procedure works  whether or not the target run-time environment contains I/O libraries or  any networked access to the host file system.

    
    
    <a rel="nofollow"></a># tgtMemDump - copy target memory to host file  #  # SYNOPSIS:  #  tgtMemDump hostfile start nbytes

    
    
    <a rel="nofollow"></a>proc tgtMemDump {fname start nbytes} {      set memHandle [wtxMemRead $start $nbytes]      memBlockWriteFile $memHandle $fname  }

    

For reference information on the **wtxtcl** routines available in the Tornado shell, see the _Tornado API Programmer's Guide_ (or the online _Tornado API Reference_).

    

All of the commands defined for the C interpreter (_7.2.4 Invoking Built-In Shell Routines_) are also available, with a double-underscore prefix, from the Tcl level; for example, to call **i( ) **from the Tcl interpreter, run the Tcl procedure **__i**. However, in many cases, it is more convenient to call a **wtxtcl**  routine instead, because the WindSh commands are designed to operate in  the C-interpreter context. For example, you can call the dynamic linker  using **ld** from the Tcl interpreter, but the  argument that names the object module may not seem intuitive: it is the  address of a string stored on the target. It is more convenient to call  the underlying **wtxtcl** command. In the case of the dynamic linker, the underlying **wtxtcl** command is **wtxObjModuleLoad**, which takes an ordinary Tcl string as its argument, as described in the online _WTX Tcl API_.

#### _Tcl: Calling Target Routines_

    

The **shParse**  utility allows you to embed calls to the C interpreter in Tcl  expressions; the most frequent application is to call a single target  routine, with the arguments specified (and perhaps capture the result).  For example, the following sends a logging message to your VxWorks  target console:

    
    
    <a rel="nofollow"></a>tcl> <font color="#00a000"><b>shParse {logMsg("Greetings from Tcl!\n")}</b></font>   32

    

You can also use **shParse** to call WindSh commands more conveniently from the Tcl interpreter, rather than using their **wtxtcl **building blocks. For example, the following is a convenient way to spawn a task from Tcl, using the C-interpreter command **sp( )**, if you do not remember the underlying **wtxtcl** command:

    
    
    <a rel="nofollow"></a>tcl> <font color="#00a000"><b>shParse {sp appTaskBegin}  </b></font>task spawned: id = 25e388, name = u1  0

#### _Tcl: Passing Values to Target Routines_

    

Because **shParse**  accepts a single, ordinary Tcl string as its argument, you can pass  values from the Tcl interpreter to C subroutine calls simply by using  Tcl facilities to concatenate the appropriate values into a C  expression. 

    

For example, a more realistic way of calling **logMsg( ) **from  the Tcl interpreter would be to pass as its argument the value of a Tcl  variable rather than a literal string. The following example evaluates a  Tcl variable **tclLog** and inserts its value (with a newline appended) as the **logMsg( )** argument:

    
    
    <a rel="nofollow"></a>tcl> <font color="#00a000"><b>shParse "logMsg(\"$tclLog\\n\")"  </b></font>32

#### _7.7.2  Tcl: Calling under C Control_

    

To dip quickly into Tcl and return immediately to the C interpreter, you can type a single line of Tcl prefixed with the **?** character (rather than using **?** by itself to toggle into Tcl mode). For example:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>?set test wonder; puts "This is a $test."  </b></font>This is a wonder.    -> 

    

Notice that the **->** prompt indicates we are still in the C interpreter, even though we just executed a line of Tcl.

    

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**CAUTION: **You may not embed Tcl evaluation inside a C expression; the** ?** prefix works only as the first nonblank character on a line, and passes the entire line following it to the Tcl interpreter.

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

For example, you may occasionally  want to use Tcl control structures to supplement the facilities of the C  interpreter. Suppose you have an application under development that  involves several collaborating tasks; in an interactive development  session, you may need to restart the whole group of tasks repeatedly.  You can define a Tcl variable with a list of all the task entry points,  as follows:

    
    
    <a rel="nofollow"></a>-> ? <font color="#00a000"><b>set appTasks {appFrobStart appGetStart appPutStart }  </b></font>appFrobStart appGetStart appPutStart 

    

Then whenever you need to restart the whole list of tasks, you can use something like the following:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>? foreach it $appTasks {shParse "sp($it)"}  </b></font>task spawned: id = 25e388, name = u0  task spawned: id = 259368, name = u1  task spawned: id = 254348, name = u2  task spawned: id = 24f328, name = u3

#### _7.7.3  Tcl: Tornado Shell lnitialization_

    

When you execute an instance of the Tornado shell, it begins by looking for a file called **.wind\windsh.tcl** in the directory specified by the **HOME**  environment variable (if that environment variable is defined). In each  of these directories, if the file exists, the shell reads and executes  its contents as Tcl expressions before beginning to interact. You can  use this file to automate any initialization steps you perform  repeatedly.

    

You can also specify a Tcl expression to execute initially on the **windsh** command line, with the option **-e **_tclExpr_. For example, you can test an initialization file before saving it as **.wind\windsh.tcl** using this option, as follows:

    
    
    <a rel="nofollow"></a>C:\> <font color="#00a000"><b>windsh phobos -e "source c:\\fred\\tcltest"</b></font> 

    

Example 7-4: **Shell Initialization File**

    

This file causes I/O for target  routines called in WindSh to be directed to the target's standard I/O  rather than to WindSh. It changes the default C++ strategy to automatic  for this shell, sets a path for locating load modules, and causes  modules not to be copied to the target server.

    
    
    <a rel="nofollow"></a># Redirect Task I/O to WindSh   shConfig SH_GET_TASK_IO off   # Set C++ strategy   shConfig LD_CALL_XTORS on   # Set Load Path   shConfig LD_PATH "/folk/jmichel/project/app;/folk/jmichel/project/test"   # Let the Target Server directly access the module   shConfig LD_SEND_MODULES off 

  
  


### _7.8  The Shell Architecture_

#### _7.8.1  Controlling the Target from the Host_

    

Tornado integrates host and target  resources so well that it creates the illusion of executing entirely on  the target itself. In reality, however, most interactions with any  Tornado tool exploit the resources of both host and target. For example,  Table 7-17 shows how the shell distributes the interpretation and execution of the following simple expression:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>dir = opendir ("/myDev/myFile")</b></font> 

    

Parsing the expression is the  activity that controls overall execution, and dispatches the other  execution activities. This takes place on the host, in the shell's C  interpreter, and continues until the entire expression is evaluated and  the shell displays its result.

    

To avoid repetitive clutter, Table 7-17  omits the following important steps, which must be carried out to link  the activities in the three contexts (and two systems) shown in each  column of the table:

    

  * After every C-interpreter step, the shell program sends a request to the target server representing the next activity required.

    

  * The target server receives each such request,  and determines whether to execute it in its own context on the host. If  not, it passes an equivalent request on to the target agent to execute  on the target. 

    

Table 7-17: **Interpreting: dir = opendir ("/myDev/myFile")**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Tornado Shell   
(on host)**

</td>
<td >

**Target Server & Symbol Table   
(on host)**

</td>
<td >

**Agent   
(on target)**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

Parse the string   
**"/myDev/myFile"**.

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

Allocate memory for the string; return address **A**.

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

</td>
<td >

Write **"/myDev/myFile"**; return address **A**.

</td></tr> <tr valign="top" >
<td >

Parse the name   
**opendir**.

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

Look up **opendir**;   
return address **B**.

</td>
<td >

</td></tr> <tr valign="top" >
<td >

Parse the function call   
**B(A)**; wait for the result.

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

</td>
<td >

Spawn a task to run **opendir( )** and signal result **C** when done.

</td></tr> <tr valign="top" >
<td >

</td>
<td >

Receive **C** from target agent and pass it to host shell.

</td>
<td >

</td></tr> <tr valign="top" >
<td >

Parse the symbol **dir**. 

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

Look up **dir** (fails).

</td>
<td >

</td></tr> <tr valign="top" >
<td >

Request a new symbol table entry **dir**. 

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

Define **dir**; return symbol **D**.

</td>
<td >

</td></tr> <tr valign="top" >
<td >

Parse the assignment   
**D=C**.

</td>
<td >

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

Allocate agent-pool memory for the value of **dir**. 

</td>
<td >

</td></tr> <tr valign="top" >
<td >

</td>
<td >

</td>
<td >

Write the value of **dir**. 

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

The first access to server and agent is to allocate storage for the string **"/myDev/myFile"** on the target and store it there, so that VxWorks subroutines (notably **opendir( )**  in this case) have access to it. There is a pool of target memory  reserved for host interactions. Because this pool is reserved, it can be  managed from the host system. The server allocates the required memory,  and informs the shell of its location; the shell then issues the  requests to actually copy the string to that memory. This request  reaches the agent on the target, and it writes the 14 bytes (including  the terminating null) there.

    

The shell's C-expression interpreter must now determine what the name **opendir** represents. Because **opendir( )** is not one of the shell's own commands, the shell looks up the symbol (through the target server) in the symbol table.

    

The C interpreter now needs to evaluate the function call to **opendir( )**  with the particular argument specified, now represented by a memory  location on the target. It instructs the agent (through the server) to  spawn a task on the target for that purpose, and awaits the result.

    

As before, the C interpreter looks up a symbol name (**dir**)  through the target server; when the name turns out to be undefined, it  instructs the target server to allocate storage for a new **int** and to make an entry pointing to it with the name **dir** in the symbol table. Again these symbol-table manipulations take place entirely on the host.

    

The interpreter now has an address (in target memory) corresponding to **dir**, on the left of the assignment statement; and it has the value returned by **opendir( )**, on the right of the assignment statement. It instructs the agent (again, through the server) to record the result at the **dir** address, and evaluation of the statement is complete. 

#### _7.8.2  Shell Components_

    

The Tornado shell includes two  interpreters, a common front end for command entry and history, and a  back end that connects the shell to the global Tornado environment to  communicate with the target server. Figure 7-4 illustrates these components: 

    

Figure 7-4: **Components of the Tornado Shell**

    

    

Line Editing

    

The line-editing and command history facilities are designed to be unobtrusive, and support your access to the interpreters. _7.5 Shell Line Editing_ describes the vi-like editing and history front end.

  

    

C-Expression Interpreter

    

The most visible  component is the C-expression interpreter, because it is the interface  that most closely resembles the application programming environment. The  bulk of this chapter describes that interpreter.

  

    

Tcl Interpreter

    

An interface for extending the shell or automating shell interactions, described in _7.7 Tcl: Shell Interpretation_.

  

    

WTX Tcl

    

The back-end  mechanism that ties together all of Tornado; the Wind River Tool  Exchange protocol, implemented as a set of Tcl extensions.

  


#### _7.8.3  Layers of Interpretation_

    

In daily use, the shell seems to be a  seamless environment; but in fact, the characters you type in WindSh go  through several layers of interpretation, as illustrated by Figure 7-5. First, input is examined for special editing keystrokes (described in _7.5 Shell Line Editing_).  Then as much interpretation as possible is done in WindSh itself. In  particular, execution of any subroutine is first attempted in the shell  itself; if a shell built-in (also called a _primitive_)  with that name exists, the built-in runs without any further checking.  Only when a subroutine call does not match any shell built-ins does  WindSh call a target routine. See _7.2.4 Invoking Built-In Shell Routines_ for more information. For a list of all WindSh primitives, see _Table 7-13List of WindSh Commands_. 

    

Figure 7-5: **Layers of Interpretation in the Shell**

    

 

    

* * *

1: A target-resident version of the shell is also available; for more information, see _VxWorks Programmer's Guide: Target Shell_.

2: As  a special case of executing WindSh from the Windows command prompt, you  can configure the properties of the command-prompt icon--or a  shortcut--to run **windsh** _targetname_.

3: Memory allocated for string constants is never freed by the shell. See _7.3.7 Strings_ for more information.

4: **d**( ) is one of the WindSh commands, implemented in Tcl and executing on the host.

5: You can also use the **-e** option to run a Tcl expression at startup, or place Tcl initialization in **.wind/windsh.tcl** under your home directory. See _7.7.3 Tcl: Tornado Shell lnitialization_.

6: The WindSh Tcl-interpreter interface is described in _7.7 Tcl: Shell Interpretation_.

7: The  examples in this book assume you are using the default shell prompts,  but you can change the C interpreter prompt to whatever string you like  using **shellPromptSet( )**. 
