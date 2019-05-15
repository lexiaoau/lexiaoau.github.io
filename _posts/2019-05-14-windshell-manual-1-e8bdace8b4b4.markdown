---
author: lexiao
comments: true
date: 2019-05-14 01:35:58+00:00
layout: post
link: http://localhost/blog/?p=362
slug: windshell-manual-1-%e8%bd%ac%e8%b4%b4
title: Windshell Manual  -- 1 [转贴]
wordpress_id: 362
categories:
- 杂而无多
---

# _7_

# _Shell_

#### _ _

  
  


### _7.1  Introduction_

    

The Tornado shell, WindSh, allows  you to download application modules, and to invoke both VxWorks and  application module subroutines. This facility has many uses: interactive  exploration of the VxWorks operating system, prototyping, interactive  development, and testing.

    

WindSh can interpret most C language  expressions; it can execute most C operators and resolve symbolic data  references and subroutine invocations. You can also interact with the  shell through a Tcl interpreter, which provides a full set of control  structures and lower-level access to target facilities. For a more  detailed explanation of the Tcl interface, see _7.7 Tcl: Shell Interpretation_. 

    

WindSh executes on the development  host, not the target, but it allows you to spawn tasks, to read from or  write to target devices, and to exert full control of the target.**1**  Because the shell executes on the host system, you can use it with  minimal intrusion on target resources. As with other Tornado tools, only  the target agent is required on the target system. Thus, the shell can  remain always available; you can use it to maintain a production system  if appropriate as well as for experimentation and testing during  development.

    

Shell operation involves three components of the Tornado system, as shown in Figure 7-1.

    

  * The _shell_ is where you  directly exercise control; it receives your commands and executes them  locally on the host, dispatching requests to the target server for any  action involving the symbol table or target-resident programs or data.

    

  * The _target server_ manages  the symbol table and handles all communications with the remote target,  dispatching function calls and sending their results back as needed.  (The symbol table itself resides entirely on the host, although the  addresses it contains refer to the target system.)

    

  * The _target agent_ is the only  component that runs on the target; it is a minimal monitor program that  mediates access to target memory and other facilities. 

    

Figure 7-1: **Tornado and the Shell**

    

    

The shell has a dual role:

    

  * It acts as a command interpreter that provides access to all VxWorks facilities by allowing you to call any VxWorks routine. 

    

  * It can be used as a prototyping and debugging  tool for the application developer. You can run application modules  interactively by calling any application routine. The shell provides  notification of any hardware exceptions. See _System Modification and Debugging_, for information about downloading application modules.

    

The capabilities of WindSh include the following:

    

  * task-specific breakpoints
    

  * task-specific single-stepping
    

  * symbolic disassembler
    

  * task and system information utilities
    

  * ability to call user routines
    

  * ability to create and examine variables symbolically
    

  * ability to examine and modify memory
    

  * exception trapping
  
  


### _7.2  Using the Shell_

    

The shell reads lines of input from  an input stream, parses and evaluates each line, and writes the result  of the evaluation to an output stream. With its default C-expression  interpreter, the shell accepts the same expression syntax as a C  compiler with only a few variations.

    

The following sections explain how  to start and stop the shell and provide examples illustrating some  typical uses of the shell's C interpreter. In the examples, the default  shell prompt for interactive input in C is "->". User input is shown  in bold face and shell responses are shown in a plain roman face.

#### _7.2.1  Starting and Stopping the Tornado Shell_

    

There are three ways to start a Tornado shell: 

    

  * From the Tornado Launch toolbar: Click the  button. This launches a shell for the currently selected target server (see _Tornado Launch Toolbar_).

    

  * From the Tools menu: Click on Shell. The dialog box shown in Figure 7-2 appears, which allows you to select a target server from the Targets drop-down list. 

    

Figure 7-2: **Shell Target-Selection Dialog Box**

    

    

  * From the Windows command prompt: Invoke **windsh**, specifying the target-server name, as in the following example:**2**

    
    
    <a rel="nofollow"></a>C:\> <font color="#00a000"><b>windsh phobos</b></font> 

    

If you start a Tornado shell from the IDE, a shell window like the one shown in Figure 7-3  appears, with the arrow prompt (->). If you start a shell from the  Windows command prompt, WindSh executes in the environment where you  call it, using the command-prompt window. 

    

Figure 7-3: **Shell Initial Display**

    

    

Regardless of how you start it, you can terminate a Tornado shell session by executing the **exit( )** or the **quit( )** command or by typing **CTRL+D**.  If the shell is not accepting input (for instance, if it loses the  connection to the target server) you can use the interrupt key (CTRL+BREAK).

    

You may run as many different  shells attached to the same target as you wish. All functions called  from a shell have their output redirected to the WindSh window from  which they received input unless you changed the shell defaults using **shConfig** (see _WindSh Environment Variables_). 

#### _7.2.2  Downloading From the Shell_

    

One of the most useful shell features for interactive development is the dynamic linker. With the shell command **ld( )**, you can download and link new portions of the application. 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>ld < /home/</b></font><i>moduleDir</i><font color="#00a000"><b>/</b></font><i>module</i><font color="#00a000"><b>.o</b></font> 

    

Because the linking is dynamic, you  only have to rebuild the particular piece you are working on, not the  entire application. Download can be cancelled with **CTRL+C** or by clicking Cancel in the load progress indicator window. The dynamic linker is discussed further in _VxWorks Programmer's Guide: Configuration and Build_.

    

The WTX error **(0x10197) ****EXCHANGE_TIMEOUT**  may occur when a WTX request keeps the target server busy longer than  30 seconds (default timeout). This may happen when loading a large  object module. A Tcl procedure is available to change the default  timeout. From WindSh use **wtxTimeout** _sec_ where _sec_ is the number of seconds before timeout:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>?wtxTimeout 120</b></font>

#### _7.2.3  Shell Features_

    

The shell provides many features  which simplify your development and testing activities. These include  command name and path completion, command and function synopsis  printing, automatic data conversion, calculation with most C operators  and variables, and help on all shell and VxWorks functions.

#### _I/O Redirection_

    

Developers often call routines that  display data on standard output or accept data from standard input. By  default the standard output and input streams are directed to the same  window as the Tornado shell. For example, in a default configuration of  Tornado, invoking **printf( )** from the shell window gives the following display:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>printf("Hello World\n")  </b></font>Hello World!  value = 13 = 0xd  -> 

    

This behavior can be dynamically modified using the Tcl procedure **shConfig** as follows:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>?shConfig SH_GET_TASK_IO off  </b></font>->  -> <font color="#00a000"><b>printf("Hello World!\n")  </b></font>value = 13 = 0xd  ->

    

The shell reports the **printf( )**  result, indicating that 13 characters have been printed. The output,  however, goes to the target's standard output, not to the shell. 

    

To determine the current configuration, use **shConfig**. If you issue the command without an argument, all parameters are listed. Use an argument to list only one parameter. 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>?shConfig SH_GET_TASK_IO  </b></font>SH_GET_TASK_IO = off

    

For more information on **shConfig**, see _WindSh Environment Variables_.

    

The standard input and output are  only redirected for the function called from WindSh. If this function  spawns other tasks, the input and output of the spawned tasks are not  redirected to WindSh. To have all IO redirected to WindSh, you can start  the target server with the options **-C -redirectShell**.

#### _Target Symbol and Path Completion_

    

Start to type any target symbol name or any existing directory name and then type **CTRL+D**.  The shell automatically completes the command or directory name for  you. If there are multiple options, it prints them for you and then  reprints your entry. For example, entering an ambiguous request  generates the following result:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>C:\Tor [CTRL+D]  </b></font>Tornado/ TorClass/  -> C:\Tor

    

You can add one or more letters and then type **CTRL+D** again until the path or symbol is complete.

#### _Synopsis Printing_

    

Once you have typed the complete function name, typing **CTRL+D** again prints the function synopsis and then reprints the function name ready for your input:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>_taskIdDefault [CTRL+D]  </b></font>taskIdDefault() - set the default task ID (WindSh)    int taskIdDefault      (      int tid     /* user-supplied task ID; if 0, return default */      )    -> _taskIdDefault

    

If the routine exists on both host  and target, the WindSh synopsis is printed. To print the target synopsis  of a function add the meta-character **@** before the function name. 

    

You can extend the synopsis printing function to include your own routines. To do this, follow these steps:

    

  1. Create the files that include the new routines following Wind River Coding Conventions. (See the _VxWorks Programmer's Guide: Coding Conventions_.)
    

  1. Include these files in your project. (See _Creating, Adding, and Removing Application Files_.)
    

  1. Add the file names to the **DOC_FILES** macro in your makefile.
    

  1. Go to the top of your project tree and run "make synopsis":

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>cd $WIND_BASE/target/src/projectX  </b></font>-> <font color="#00a000"><b>make synopsis</b></font>

    

This adds a file **projectX** to the **host/resource/synopsis** directory.

#### _HTML Help_

    

Typing any function name, a space, and **CTRL+W** opens a browser and displays the HTML reference page for the function. Be sure to leave a space after the function name.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>i [CTRL+W]</b></font>

    

or

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>@i [CTRL+W]</b></font>

    

Typing **CTRL+W** without any function name launches the HTML help tool in a new browser window. 

    

Typing **CTRL+W** without a typing a space after the function name launches the HTML help tool if the function name is unique. If not, **CTRL+W** acts as **CTRL+D** and returns a list of functions whose names begin with the string you entered. 

#### _Data Conversion _

    

The shell prints all integers and  characters in both decimal and hexadecimal, and if possible, as a  character constant or a symbolic address and offset.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>68  </b></font>value = 68 = 0x44 = 'D'

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>0xf5de  </b></font>value = 62942 = 0xf5de = _init + 0x52

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>'s'  </b></font>value = 115 = 0x73 = 's'

#### _Data Calculation_

    

Almost all C operators can be used for data calculation. Use "(" and ")" to force order of precedence.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>(14 * 9) / 3  </b></font>value = 42 = 0x2a = '*'

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>(0x1355 << 3) & 0x0f0f  </b></font>value = 2568 = 0xa08

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>4.3 * 5  </b></font>value = 21.5

#### _Calculations With Variables_

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>(j + k) * 3  </b></font>value = ...

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>*(j + 8 * k)  </b></font>(<i>address (j + 8 * k)</i>): value = 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>x = (val1 - val2) / val3  </b></font>new symbol "x" added to symbol table  address =   value = 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>f = 1.41 * 2  </b></font>new symbol "f" added to symbol table  f = <i>(address of f)</i>: value = 2.82

    

Variable **f** gets an 8-byte floating point value.

    
    
    <a rel="nofollow"></a>-> ddd=5.2  new symbol "ddd" added to symbol table.  ddd = 0xba0e2c: value = 5.2

    
    
    <a rel="nofollow"></a>-> eee=10.5  new symbol "eee" added to symbol table.  eee = 0xba0e24: value = 10.5

    
    
    <a rel="nofollow"></a>-> fff=(double)ddd+(double)eee  new symbol "fff" added to symbol table.  fff = 0xba0e1c: value = 15.7  -> 

#### _WindSh Environment Variables_

    

WindSh allows you to change the behavior of a particular shell session by setting the environment variables listed in Table 7-1. The Tcl procedure **shConfig**  allows you to display and set how I/O redirection, C++ constructors and  destructors, loading, and the load path are defined and handled by the  shell.

    

Table 7-1: **WindSh Environment Variables**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Variable**

</td>
<td >

**Result**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**SH_GET_TASK_IO**

</td>
<td >

Sets the I/O  redirection mode for called functions. The default is "on", which  redirects input and output of called functions to WindSh. To have input  and output of called functions appear in the target console, set **SH_GET_TASK_IO** to "off." 

</td></tr> <tr valign="top" >
<td >

**LD_CALL_XTORS**

</td>
<td >

Sets the C++  strategy related to constructors and destructors. The default is  "target", which causes WindSh to use the value set on the target using **cplusXtorSet( )**. If **LD_CALL_XTORS**  is set to "on", the C++ strategy is set to automatic (for the current  WindSh only). "Off" sets the C++ strategy to manual for the current  shell. 

</td></tr> <tr valign="top" >
<td >

**LD_SEND_MODULES**

</td>
<td >

Sets the load mode.  The default "on" causes modules to be transferred to the target server.  This means that any module WindSh can see can be loaded. If **LD_SEND_MODULES** if "off", the target server must be able to see the module to load it.

</td></tr> <tr valign="top" >
<td >

**LD_PATH**

</td>
<td >

Sets the search path for modules using the separator ";". When a **ld( )**  command is issued, WindSh first searches the current directory and  loads the module if it finds it. If not, WindSh searches the directory  path for the module. 

</td></tr> <tr valign="top" >
<td >

**LD_COMMON_MATCH_ALL**

</td>
<td >

Sets the loader behavior for common symbols. If it is set to **on**,  the loader tries to match a common symbol with an existing one. If a  symbol with the same name is already defined, the loader take its  address. Otherwise, the loader creates a new entry. If set to **off**, the loader does not try to find an existing symbol. It creates an entry for each common symbol. 

</td></tr> <tr valign="top" >
<td >

**DSM_HEX_MOD**

</td>
<td >

Sets the  disassembling "symbolic + offset" mode. When set to "off" the "symbolic +  offset" address representation is turned on and addresses inside the  disassembled instructions are given in terms of "symbol name + offset."  When set to "on" these addresses are given in hexadecimal.

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

Because **shConfig** is a Tcl procedure, use the **?** to move from the C interpreter to the Tcl interpreter. (See _7.7.2 Tcl: Calling under C Control_.)

    

Example 7-1: **Using shConfig to Modify WindSh Behavior**

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>?shConfig  </b></font>SH_GET_TASK_IO = on  LD_CALL_XTORS = target  LD_SEND_MODULES = on  LD_PATH = C:/ProjectX/lib/objR4650gnutest/;C:/ProjectY/lib/objR4560gnuvx  -> <font color="#00a000"><b>?shConfig LD_CALL_XTORS on  </b></font>-> <font color="#00a000"><b>?shConfig LD_CALL_XTORS  </b></font>LD_CALL_XTORS = on

#### _7.2.4  Invoking Built-In Shell Routines_

    

Some of the commands (or routines)  that you can execute from the shell are built into the host shell,  rather than running as function calls on the target. These facilities  parallel interactive utilities that can be linked into VxWorks itself.  By using the host commands, you minimize the impact on both target  memory and performance. 

    

The following sections give summaries of the Tornado WindSh commands. For more detailed reference information, see the **windsh** reference entry (either online, or in _. _).

    

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**WARNING: **Most  of the shell commands correspond to similar routines that can be linked  into VxWorks for use with the target-resident version of the shell (_VxWorks Programmer's Guide: Target Shell_).  However, the target-resident routines differ in some details. For  reference information on a shell command, be sure to consult the **windsh** entry in the online _Tornado Tools Reference_. Although there is usually an entry with the same name in the _VxWorks API Reference_, it describes a related target routine, not the shell command.

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

#### _Task Management_

    

Table 7-2 summarizes the WindSh commands that manage VxWorks tasks. 

    

Table 7-2: **WindSh Commands for Task Management **

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Call**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**sp( )**

</td>
<td >

Spawn a task with default parameters.

</td></tr> <tr valign="top" >
<td >

**sps( )**

</td>
<td >

Spawn a task, but leave it suspended.

</td></tr> <tr valign="top" >
<td >

**tr( )**

</td>
<td >

Resume a suspended task.

</td></tr> <tr valign="top" >
<td >

**ts( )**

</td>
<td >

Suspend a task.

</td></tr> <tr valign="top" >
<td >

**td( )**

</td>
<td >

Delete a task.

</td></tr> <tr valign="top" >
<td >

**period( )**

</td>
<td >

Spawn a task to call a function periodically.

</td></tr> <tr valign="top" >
<td >

**repeat( )**

</td>
<td >

Spawn a task to call a function repeatedly.

</td></tr> <tr valign="top" >
<td >

**taskIdDefault( )**

</td>
<td >

Set or report the default (current) task ID (see _7.3.4 The "Current" Task and Address_ for a discussion of how the current task is established and used).

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

The **repeat( )** and **period( )** commands spawn tasks whose entry points are **_repeatHost** and **_periodHost**. The shell downloads these support routines when you call **repeat( )** or **period( )**.  (With remote target servers, that download sometimes fails; for a  discussion of when this is possible, and what you can do about it, see _7.6 Object Module Load Path_.) These tasks may be controlled like any other tasks on the target; for example, you can suspend or delete them with **ts( )** or **td( )** respectively.

#### _Task Information_

    

Table 7-3 summarizes the WindSh commands that report task information. 

    

Table 7-3: **WindSh Commands for Task Information **

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Call**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**i( )**

</td>
<td >

Display system  information. This command gives a snapshot of what tasks are in the  system, and some information about each of them, such as state, PC, SP,  and TCB address. To save memory, this command queries the target  repeatedly; thus, it may occasionally give an inconsistent snapshot.

</td></tr> <tr valign="top" >
<td >

**iStrict( )**

</td>
<td >

Display the same information as **i( )**,  but query target system information only once. At the expense of  consuming more intermediate memory, this guarantees an accurate  snapshot.

</td></tr> <tr valign="top" >
<td >

**ti( )**

</td>
<td >

Display task  information. This command gives all the information contained in a  task's TCB. This includes everything shown for that task by an **i**( ) command, plus all the task's registers, and the links in the TCB chain. If _task_ is 0 (or the argument is omitted), the current task is reported on.

</td></tr> <tr valign="top" >
<td >

**w( )**

</td>
<td >

Print a summary of each task's pending information, task by task. This routine calls **taskWaitShow( )** in quiet mode on all tasks in the system, or a specified task if the argument is given. 

</td></tr> <tr valign="top" >
<td >

**tw( )**

</td>
<td >

Print information about the object the given task is pending on. This routine calls **taskWaitShow( )** on the given task in verbose mode.

</td></tr> <tr valign="top" >
<td >

**checkStack( )**

</td>
<td >

Show a stack usage  summary for a task, or for all tasks if no task is specified. The  summary includes the total stack size (SIZE), the current number of  stack bytes (CUR), the maximum number of stack bytes used (HIGH), and  the number of bytes never used at the top of the stack (MARGIN = SIZE -  HIGH). Use this routine to determine how much stack space to allocate,  and to detect stack overflow. This routine does not work for tasks that  use the **VX_NO_STACK_FILL** option.

</td></tr> <tr valign="top" >
<td >

**tt( )**

</td>
<td >

Display a stack trace.

</td></tr> <tr valign="top" >
<td >

**taskIdFigure( )**

</td>
<td >

Report a task ID, given its name.

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

The **i( )**  command is commonly used to get a quick report on target activity. (To  see this information periodically, use the Tornado browser; see _9. Browser_). If nothing seems to be happening, **i( )** is often a good place to start investigating. To display summary information about all running tasks:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>i  </b></font>  NAME       ENTRY      TID    PRI   STATUS    PC      SP     ERRNO  DELAY  --------- ----------- -------- --- -------- ------- -------- ------- -----  tExcTask  _excTask      3ad290   0 PEND       4df10   3ad0c0       0     0  tLogTask  _logTask      3aa918   0 PEND       4df10   3aa748       0     0  tWdbTask  0x41288       3870f0   3 READY      23ff4   386d78  3d0004     0  tNetTask  _netTask      3a59c0  50 READY      24200   3a5730       0     0  tFtpdTask _ftpdTask     3a2c18  55 PEND       23b28   3a2938       0     0  value = 0 = 0x0

    

The **w( )** and **tw( )** commands allow you to see what object a VxWorks task is pending on. **w( )** displays summary information for all tasks, while **tw( )** displays object information for a specific task. Note that the **OBJ_NAME** field is used only for objects that have a symbolic name associated with the address of their structure.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>w  </b></font>  NAME       ENTRY     TID       STATUS    DELAY OBJ_TYPE   OBJ_ID   OBJ_NAME  ---------- ---------- -------- --------- ----- ---------- -------- ----------  tExcTask   _excTask   3d9e3c   PEND          0 MSG_Q(R)   3d9ff4   N/A  tLogTask   _logTask   3d7510   PEND          0 MSG_Q(R)   3d76c8   N/A  tWdbTask   _wdbCmdLoo 36dde4   READY         0                 0  tNetTask   _netTask   3a43d0   READY         0                 0  u0         _smtask1   36cc2c   PEND          0 MSG_Q_S(S) 370b61   N/A  u1         _smtask3   367c54   PEND          0 MSG_Q_S(S) 370b61   N/A  u3         _taskB     362c7c   PEND          0 SEM_B       8d378   _mySem2  u6         _smtask1   35dca4   PEND          0 MSG_Q_S(S) 370ae1   N/A  u9         _task3B    358ccc   PEND          0 MSG_Q(S)    8cf1c   _myMsgQ  value = 0 = 0x0  ->   -> <font color="#00a000"><b>tw u1  </b></font>  NAME       ENTRY     TID       STATUS    DELAY OBJ_TYPE   OBJ_ID   OBJ_NAME  ---------- ---------- -------- --------- ----- ---------- -------- ----------  u1         _smtask3   367c54   PEND          0 MSG_Q_S(S) 370b61   N/A    Message Queue Id   : 0x370b61  Task Queueing      : SHARED_FIFO  Message Byte Len   : 100  Messages Max       : 0  Messages Queued    : 0  Senders Blocked    : 2  Send Timeouts      : 0  Receive Timeouts   : 0    Senders Blocked:  TID        CPU Number Shared TCB  ---------- ---------- ----------  0x36cc2c       0      0x36e464  0x367c54       0      0x36e47c    value = 0 = 0x0  ->

#### _System Information_

    

Table 7-4 shows the WindSh commands that display information from the symbol table, from the target system, and from the shell itself. 

    

Table 7-4: **WindSh Commands for System Information **

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Call**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**devs( )**

</td>
<td >

List all devices known on the target system.

</td></tr> <tr valign="top" >
<td >

**lkup( )**

</td>
<td >

List symbols from symbol table.

</td></tr> <tr valign="top" >
<td >

**lkAddr( )**

</td>
<td >

List symbols whose values are near a specified value.

</td></tr> <tr valign="top" >
<td >

**d( )**

</td>
<td >

Display target memory. You can specify a starting address, size of memory units, and number of units to display.

</td></tr> <tr valign="top" >
<td >

**l( )**

</td>
<td >

Disassemble and display a specified number of instructions.

</td></tr> <tr valign="top" >
<td >

**printErrno( )**

</td>
<td >

Describe the most recent error status value.

</td></tr> <tr valign="top" >
<td >

**version( )**

</td>
<td >

Print VxWorks version information.

</td></tr> <tr valign="top" >
<td >

**cd( )**

</td>
<td >

Change the host working directory (no effect on target).

</td></tr> <tr valign="top" >
<td >

**ls( )**

</td>
<td >

List files in host working directory.

</td></tr> <tr valign="top" >
<td >

**pwd( )**

</td>
<td >

Display the current host working directory.

</td></tr> <tr valign="top" >
<td >

**help( )**

</td>
<td >

Display a summary of selected shell commands.

</td></tr> <tr valign="top" >
<td >

**h( )**

</td>
<td >

Display up to 20 lines of command history.

</td></tr> <tr valign="top" >
<td >

**shellHistory**( ) 

</td>
<td >

Set or display shell history.

</td></tr> <tr valign="top" >
<td >

**shellPromptSet**( ) 

</td>
<td >

Change the C-interpreter shell prompt.

</td></tr> <tr valign="top" >
<td >

**printLogo( )**

</td>
<td >

Display the Tornado shell logo.

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

The **lkup( )**  command takes a regular expression as its argument, and looks up all  symbols containing strings that match. In the simplest case, you can  specify a substring to see any symbols containing that string. For  example, to display a list containing routines and declared variables  with names containing the string _dsm_, do the following:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>lkup "dsm"  </b></font>_dsmData                  0x00049d08 text     (vxWorks)  _dsmNbytes                0x00049d76 text     (vxWorks)  _dsmInst                  0x00049d28 text     (vxWorks)  mydsm                    0x003c6510 bss     (vxWorks)

    

Case is significant, but position is not (**mydsm** is shown, but **myDsm** would not be). To explicitly write a search that would match either **mydsm** or **myDsm**, you could write the following:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>lkup "[dD]sm"</b></font>

    

Regular-expression searches of the  symbol table can be as simple or elaborate as required. For example, the  following simple regular expression displays the names of three  internal VxWorks semaphore functions:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>lkup "sem.Take"  </b></font>_semBTake                 0x0002aeec text     (vxWorks)  _semCTake                 0x0002b268 text     (vxWorks)  _semMTake                 0x0002bc48 text     (vxWorks)  value = 0 = 0x0

    

Another information command is a symbolic disassembler, **l( )**. The command syntax is:

    
    
    <a rel="nofollow"></a>l [<i>adr</i>[, <i>n</i>]]

    

This command lists _n_ disassembled instructions, starting at _adr_. If _n_ is 0 or not given, the _n_ from a previous **l**( ) or the default value (10) is used. If _adr_ is 0, **l**( ) starts from where the previous **l**( ) stopped, or from where an exception occurred (if there was an exception trap or a breakpoint since the last **l**( ) command).

    

The disassembler uses any symbols  that are in the symbol table. If an instruction whose address  corresponds to a symbol is disassembled (the beginning of a routine, for  instance), the symbol is shown as a label in the address field. Symbols  are also used in the operand field. The following is an example of  disassembled code for an MC680_x_0 target:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>l printf  </b></font>_printf

    
    
    <a rel="nofollow"></a>    00033bce  4856                     PEA         (A6)      00033bd0  2c4f                     MOVEA .L    A7,A6      00033bd2  4878 0001                PEA         0x1      00033bd6  4879 0003 460e           PEA         _fioFormatV + 0x780      00033bdc  486e 000c                PEA         (0xc,A6)      00033be0  2f2e 0008                MOVE  .L    (0x8,A6),-(A7)      00033be4  6100 02a8                BSR         _fioFormatV      00033be8  4e5e                     UNLK         A6      00033bea  4e75                    RTS

    

This example shows the **printf( )** routine. The routine does a **LINK**, then pushes the value of **std_out** onto the stack and calls the routine **fioFormatV( )**. Notice that symbols defined in C (routine and variable names) are prefixed with an underbar ( **_** ) by the compiler.

    

Perhaps the most frequently used system information command is **d( )**,  which displays a block of memory starting at the address which is  passed to it as a parameter. As with any other routine that requires an  address, the starting address can be a number, the name of a variable or  routine, or the result of an expression.

    

Several examples of variations on **d( )** appear below.

    

Display starting at address 1000 decimal:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d (1000)</b></font>

    

Display starting at 1000 hex:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d 0x1000</b></font>

    

Display starting at the address contained in the variable **dog**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d dog</b></font>

    

The above is different from a display starting at the address of **dog**. For example, if **dog** is a variable at location 0x1234, and that memory location contains the value 10000, **d( )** displays starting at 10000 in the previous example and at 0x1234 in the following:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d &dog</b></font>

    

Display starting at an offset from the value of **dog**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d dog + 100</b></font>

    

Display starting at the result of a function call:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d func (dog)</b></font>

    

Display the code of **func( )** as a simple hex memory dump:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>d func</b></font> 

    

When you use **cd( )**  in the host shell, you are changing the working directory on the host.  It does not change the directory on the target. WindSh has no knowledge  of the target file system. Thus if you mount a drive on the target from  the host shell and try to **cd( )** to it, you see the following: 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>cd "/ata0/"</b></font>   couldn't change working directory to "\ata0": no such file or directory   value = -1 = 0xffffffff

    

However, the result is different if you execute **cd( ) **and **ls( )** on the target by prefixing the commands with **@**: 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>@cd "/ata0/"</b></font>   value = 0 = 0x0 

    
    
    <a rel="nofollow"></a>-> <b>@ls</b>   IO.SYS   MSDOS.SYS   DRVSPACE.BIN

    

**@cd** and **@ls** only work if you have the component **INCLUDE_DISK_UTIL** included in your target image. 

    

The above also applies if you wish to use the target server file system (TSFS) from WindSh: **cd "/tgtsvr"** does not work but **@cd "/tgtsvr"** does. To use TSFS you must have the TSFS component, **INCLUDE_WDB_TSFS**, installed in VxWorks and start the target server with the **"-R **_dirName_**"** or **"-R **_dirName_** -RW"** option.

#### _System Modification and Debugging_

    

Developers often need to change the  state of the target, whether to run a new version of some software  module, to patch memory, or simply to single-step a program. Table 7-5 summarizes the WindSh commands of this type. 

    

Table 7-5: **WindSh Commands for System Modification and Debugging **

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Call**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**ld**( ) 

</td>
<td >

Load an object module into target memory and link it dynamically into the run-time.

</td></tr> <tr valign="top" >
<td >

**unld**( ) 

</td>
<td >

Remove a dynamically-linked object module from target memory, and free the storage it occupied.

</td></tr> <tr valign="top" >
<td >

**m**( ) 

</td>
<td >

Modify memory in_ width_ (byte, short, or long) starting at _adr_. The **m**( )  command displays successive words in memory on the terminal; you can  change each word by typing a new hex value, leave the word unchanged and  continue by typing ENTER, or return to the shell by typing a dot (.).

</td></tr> <tr valign="top" >
<td >

**mRegs**( ) 

</td>
<td >

Modify register values for a particular task.

</td></tr> <tr valign="top" >
<td >

**b**( ) 

</td>
<td >

Set or display breakpoints, in a specified task or in all tasks.

</td></tr> <tr valign="top" >
<td >

**bh**( ) 

</td>
<td >

Set a hardware breakpoint. 

</td></tr> <tr valign="top" >
<td >

**s**( ) 

</td>
<td >

Step a program to the next instruction.

</td></tr> <tr valign="top" >
<td >

**so**( ) 

</td>
<td >

Single-step, but step over a subroutine. 

</td></tr> <tr valign="top" >
<td >

**c**( ) 

</td>
<td >

Continue from a breakpoint.

</td></tr> <tr valign="top" >
<td >

**cret**( ) 

</td>
<td >

Continue until the current subroutine returns. 

</td></tr> <tr valign="top" >
<td >

**bdall**( ) 

</td>
<td >

Delete all breakpoints.

</td></tr> <tr valign="top" >
<td >

**bd**( ) 

</td>
<td >

Delete a breakpoint.

</td></tr> <tr valign="top" >
<td >

**reboot**( ) 

</td>
<td >

Return target control to the target boot ROMs, then reset the target server and reattach the shell.

</td></tr> <tr valign="top" >
<td >

**bootChange**( ) 

</td>
<td >

Modify the saved values of boot parameters (see _2.5.4 Description of Boot Parameters_).

</td></tr> <tr valign="top" >
<td >

**sysSuspend**( ) 

</td>
<td >

If supported by the target-agent configuration, enter system mode. See _7.2.7 Using the Shell for System Mode Debugging_.

</td></tr> <tr valign="top" >
<td >

**sysResume**( ) 

</td>
<td >

If supported by the target agent (and if system mode is in effect), return to task mode from system mode.

</td></tr> <tr valign="top" >
<td >

**agentModeShow**( ) 

</td>
<td >

Show the agent mode (_system_ or _task_). 

</td></tr> <tr valign="top" >
<td >

**sysStatusShow**( ) 

</td>
<td >

Show the system context status (suspended or running). 

</td></tr> <tr valign="top" >
<td >

**quit**( ) or **exit**( ) 

</td>
<td >

Dismiss the shell.

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

The **m( )** command provides an interactive way of manipulating target memory.

    

The remaining commands in this group  are for breakpoints and single-stepping. You can set a breakpoint at  any instruction. When that instruction is executed by an eligible task  (as specified with the **b( )** command), the task  that was executing on the target suspends, and a message appears at the  shell. At this point, you can examine the task's registers, do a task  trace, and so on. The task can then be deleted, continued, or  single-stepped. 

    

If a routine called from the shell  encounters a breakpoint, it suspends just as any other routine would,  but in order to allow you to regain control of the shell, such suspended  routines are treated in the shell as though they had returned 0. The  suspended routine is nevertheless available for your inspection.

    

When you use **s( )**  to single-step a task, the task executes one machine instruction, then  suspends again. The shell display shows all the task registers and the _next_ instruction to be executed by the task.

    

You can use the **bh( )**  command to set hardware breakpoints at any instruction or data element.  Instruction hardware breakpoints can be useful to debug code running in  ROM or Flash EPROM. Data hardware breakpoints (watchpoints) are useful  if you want to stop when your program accesses a specific address.  Hardware breakpoints are available on some BSPs; see your BSP  documentation to determine if they are supported for your BSP. The  arguments of the **bh( )** command are architecture specific. For more information, run the **help( )**  command. The number of hardware breakpoints you can set is limited by  the hardware; if you exceed the maximum number, you will receive an  error.

#### _C++ Development_

    

Certain WindSh commands are intended specifically for work with C++ applications. Table 7-6 summarizes these commands. For more discussion of these shell commands, see _VxWorks Programmer's Guide: C++ Development_. 

    

Table 7-6: **WindSh Commands for C++ Development**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Call**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**cplusCtors**( ) 

</td>
<td >

Call static constructors manually.

</td></tr> <tr valign="top" >
<td >

**cplusDtors**( ) 

</td>
<td >

Call static destructors manually.

</td></tr> <tr valign="top" >
<td >

**cplusStratShow**( ) 

</td>
<td >

Report on whether current constructor/destructor strategy is manual or automatic.

</td></tr> <tr valign="top" >
<td >

**cplusXtorSet**( ) 

</td>
<td >

Set constructor/destructor strategy.

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

In addition, you can use the Tcl routine **shConfig** to set the environment variable **LD_CALL_XTORS**  within a particular shell. This allows you to use a different C++  strategy in a shell than is used on the target. For more information on **shConfig**, see _WindSh Environment Variables_. 

#### _Object Display_

    

Table 7-7  summarizes the WindSh commands that display VxWorks objects. The  browser provides displays that are analogous to the output of many of  these routines, except that browser windows can update their contents  periodically; see _9. Browser_. 

    

Table 7-7: **WindSh Commands for Object Display **

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Call**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**show**( ) 

</td>
<td >

Print information on a specified object in the shell window.

</td></tr> <tr valign="top" >
<td >

**browse**( ) 

</td>
<td >

Display a specified object in the Tornado browser.

</td></tr> <tr valign="top" >
<td >

**classShow**( ) 

</td>
<td >

Show information about a class of VxWorks kernel objects. List available classes with:
    
    <a rel="nofollow"></a><font color="#009090"><tt>  </tt></font>  -> <font color="#00a000"><b>lkup "ClassId"</b></font>

</td></tr> <tr valign="top" >
<td >

**taskShow**( ) 

</td>
<td >

Display information from a task's TCB.

</td></tr> <tr valign="top" >
<td >

**taskCreateHookShow**( ) 

</td>
<td >

Show the list of task create routines. 

</td></tr> <tr valign="top" >
<td >

**taskDeleteHookShow**( ) 

</td>
<td >

Show the list of task delete routines. 

</td></tr> <tr valign="top" >
<td >

**taskRegsShow**( ) 

</td>
<td >

Display the contents of a task's registers. 

</td></tr> <tr valign="top" >
<td >

**taskSwitchHookShow**( ) 

</td>
<td >

Show the list of task switch routines. 

</td></tr> <tr valign="top" >
<td >

**taskWaitShow**( ) 

</td>
<td >

Show information about the object a task is pended on. Note that **taskWaitShow**( ) cannot give object IDs for POSIX semaphores or message queues. 

</td></tr> <tr valign="top" >
<td >

**semShow**( ) 

</td>
<td >

Show information about a semaphore.

</td></tr> <tr valign="top" >
<td >

**semPxShow**( ) 

</td>
<td >

Show information about a POSIX semaphore.

</td></tr> <tr valign="top" >
<td >

**wdShow**( ) 

</td>
<td >

Show information about a watchdog timer.

</td></tr> <tr valign="top" >
<td >

**msgQShow**( ) 

</td>
<td >

Show information about a message queue.

</td></tr> <tr valign="top" >
<td >

**mqPxShow**( ) 

</td>
<td >

Show information about a POSIX message queue.

</td></tr> <tr valign="top" >
<td >

**iosDrvShow**( ) 

</td>
<td >

Display a list of system drivers.

</td></tr> <tr valign="top" >
<td >

**iosDevShow**( ) 

</td>
<td >

Display the list of devices in the system.

</td></tr> <tr valign="top" >
<td >

**iosFdShow**( ) 

</td>
<td >

Display a list of file descriptor names in the system.

</td></tr> <tr valign="top" >
<td >

**memPartShow**( ) 

</td>
<td >

Show partition blocks and statistics.

</td></tr> <tr valign="top" >
<td >

**memShow**( ) 

</td>
<td >

Display the total  amount of free and allocated space in the system partition, the number  of free and allocated fragments, the average free and allocated fragment  sizes, and the maximum free fragment size. Show current as well as  cumulative values. With an argument of 1, also display the free list of  the system partition.

</td></tr> <tr valign="top" >
<td >

**smMemShow**( ) 

</td>
<td >

Display the amount of free space and statistics on memory-block allocation for the shared-memory system partition.

</td></tr> <tr valign="top" >
<td >

**smMemPartShow**( ) 

</td>
<td >

Display the amount of free space and statistics on memory-block allocation for a specified shared-memory partition.

</td></tr> <tr valign="top" >
<td >

**moduleShow**( ) 

</td>
<td >

Show the current status for all the loaded modules.

</td></tr> <tr valign="top" >
<td >

**moduleIdFigure**( ) 

</td>
<td >

Report a loaded module's module ID, given its name.

</td></tr> <tr valign="top" >
<td >

**intVecShow**( ) 

</td>
<td >

Display the  interrupt vector table. This routine displays information about the  given vector or the whole interrupt vector table if _vector_ is equal to -1. Note that **intVecShow**( ) is not supported on architectures such as ARM and PowerPC that do not use interrupt vectors. 

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

#### _Network Status Display_

    

Table 7-8 summarizes the WindSh commands that display information about the VxWorks network. 

    

Table 7-8: **WindSh Commands for Network Status Display**

     <table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td >

**Call**

</td>
<td >

**Description**

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="top" >
<td >

**hostShow**( ) 

</td>
<td >

Display the host table. 

</td></tr> <tr valign="top" >
<td >

**icmpstatShow**( ) 

</td>
<td >

Display statistics for ICMP (Internet Control Message Protocol). 

</td></tr> <tr valign="top" >
<td >

**ifShow**( ) 

</td>
<td >

Display the attached network interfaces. 

</td></tr> <tr valign="top" >
<td >

**inetstatShow**( ) 

</td>
<td >

Display all active connections for Internet protocol sockets. 

</td></tr> <tr valign="top" >
<td >

**ipstatShow**( ) 

</td>
<td >

Display IP statistics. 

</td></tr> <tr valign="top" >
<td >

**routestatShow**( ) 

</td>
<td >

Display routing statistics. 

</td></tr> <tr valign="top" >
<td >

**tcpstatShow**( ) 

</td>
<td >

Display all statistics for the TCP protocol. 

</td></tr> <tr valign="top" >
<td >

**tftpInfoShow**( ) 

</td>
<td >

Get TFTP status information. 

</td></tr> <tr valign="top" >
<td >

**udpstatShow**( ) 

</td>
<td >

Display statistics for the UDP protocol. 

</td></tr> <tr >
<td colspan="20" >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

In order for a protocol-specific command to work, the appropriate protocol must be included in your VxWorks configuration.

#### _Resolving Name Conflicts between Host and Target_

    

If you invoke a name that stands for  a host shell command, the shell always invokes that command, even if  there is also a target routine with the same name. Thus, for example, **i( )** always runs on the host, regardless of whether you have the VxWorks routine of the same name linked into your target. 

    

However, you may occasionally need  to call a target routine that has the same name as a host shell command.  The shell supports a convention allowing you to make this choice: use  the single-character prefix **@** to identify the target version of any routine. For example, to run a target routine named **i( )**, invoke it with the name **@i( )**.

#### _7.2.5  Running Target Routines from the Shell_

    

All target routines are available  from WindSh. This includes both VxWorks routines and your application  routines. Thus the shell provides a powerful tool for testing and  debugging your applications using all the host resources while having  minimal impact on how the target performs and how the application  behaves.

    

##### _Invocations of VxWorks Subroutines_

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>taskSpawn ("tmyTask", 10, 0, 1000, myTask, fd1, 300)  </b></font>value = 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>fd = open ("file", 0, 0)  </b></font>new symbol "fd" added to symbol table  fd = <i>(address of fd)</i>: value = 

    

##### _Invocations of Application Subroutines_

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>testFunc (123)  </b></font>value = 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>myValue = myFunc (1, &val, testFunc (123))  </b></font>myValue = <i>(address of myValue)</i>: value = 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>myDouble = (double ()) myFuncWhichReturnsADouble (x)  </b></font>myDouble = <i>(address of myDouble)</i>: value = 

    

For situations where the result of a routine is something other than a 4-byte integer, see _Function Calls_.

#### _7.2.6  Rebooting from the Shell_

    

In an interactive real-time  development session, it is sometimes convenient to restart everything to  make sure the target is in a known state. WindSh provides the **reboot( )** command or **CTRL+SHIFT+X** to make this easy.

    

When you execute **reboot( )** or type **CTRL+SHIFT+X**, the following reboot sequence occurs:

    

  1. The shell displays a message to confirm rebooting has begun:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>reboot  </b></font>Rebooting...

    

  1. The target reboots.
    

  1. The original target server on the host  detects the target reboot and restarts itself, with the same  configuration as previously. The target-server configuration options **-Bt** (timeout) and **-Br**  (retries) govern how long the new server waits for the target to  reboot, and how many times the new server attempts to reconnect; see the  **tgtsvr** reference entry in the online _Tornado Tools Reference_.
    

  1. The shell detects the target-server  restart and begins an automatic-restart sequence (initiated any time it  loses contact with the target server for any reason), indicated with the  following messages:

    
    
    <a rel="nofollow"></a>Target connection has been lost.  Restarting shell...  Waiting to attach to target server......

    

  1. When WindSh establishes contact with the new target server, it displays the Tornado shell logo and awaits your input.

    

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**CAUTION: **If the target server timeout (**-Bt**) and retry count (**-Br**)  are too low for your target and your connection method, the new target  server may abandon execution before the target finishes rebooting. The  default timeout is one second, and the default retry count is three;  thus, by default the target server waits three seconds for the target to  reboot. If the shell does not restart in a reasonably short time after a  **reboot( )**, try starting a new target server manually.

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

#### _7.2.7  Using the Shell for System Mode Debugging_

    

The bulk of this chapter discusses  the shell in its most frequent style of use: attached to a normally  running VxWorks system, through a target agent running in task mode. You  can also use the shell with a system-mode agent. Entering system mode  stops the entire target system: all tasks, the kernel, and all ISRs.  Similarly, breakpoints affect all tasks. 

    

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**CAUTION: **When you use system mode debugging, you cannot execute expressions that call target-resident routines. You must use **sp( )**  to spawn a task with the target-resident routine as the entry point  argument. A newly-spawned task will not execute until you allow the  kernel to run long enough to schedule that task. 

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

Depending on how the target agent is configured, you may be able to switch between system mode and task mode; see _4.7 Configuring the Target-Host Communication Interface_. When the agent supports mode switching, the following WindSh commands control system mode:

    

**sysSuspend( )**

    

Enter system mode and stop the target system.

  

    

**sysResume( )**

    

Return to task mode and resume execution of the target system.

  


    

The following commands are to determine the state of the system and the agent:

    

**agentModeShow( )**

    

Show the agent mode (_system_ or _task_).

  

    

**sysStatusShow( )**

    

Show the system context status (_suspended_ or _running_).

  


    

The following shell commands behave differently in system mode:

    

**b( )**

    

Set a system-wide breakpoint; the system stops when this breakpoint is encountered by any task, or the kernel, or an ISR.

  

    

**c( )**

    

Resume execution of the entire system (but remain in system mode). 

  

    

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**WARNING: **If you are running either CrossWind or Look! you must not use **c( )** from the shell to continue; instead continue from the debugger itself. Using **c( )** from the shell when the debugger is running will confuse the debugger.

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

**i( )**

    

Display the state of the system context and the mode of the agent. 

  

    

**s( )**

    

Single-step the entire system.

  

    

**sp( )**

    

Add a task to the  execution queue. The task does not begin to execute until you continue  the kernel or step through the task scheduler.

  


    

The following example shows how to use system mode debugging to debug a system interrupt. 

    

Example 7-2: **System-Mode Debugging**

    

In this case, **usrClock( )**  is attached to the system clock interrupt handler which is called at  each system clock tick when VxWorks is running. First suspend the system  and confirm that it is suspended using either **i( )** or **sysStatusShow( )**. 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>sysSuspend  </b></font>value = 0 = 0x0  ->  -> <font color="#00a000"><b>i  </b></font>NAME      ENTRY      TID      PRI   STATUS  PC      SP      ERRNO  DELAY  --------- ---------- -------- ----- ------- ------- ------- -----  -----  tExcTask  _excTask   3e8f98   0     PEND    47982   3e8ef4  0      0  tLogTask  _logTask   3e6670   0     PEND    47982   3e65c8  0      0  tWdbTask  0x3f024    398e04   3     PEND    405ac   398d50  30067  0  tNetTask  _netTask   3b39e0   50    PEND    405ac   3b3988  0      0    Agent mode     : Extern  System context : Suspended  value = 0 = 0x0  ->   -> <font color="#00a000"><b>sysStatusShow  </b></font>System context is suspended  value = 0 = 0x0

    

Next, set the system mode breakpoint  on the entry point of the interrupt handler you want to debug. Since  the target agent is running in system mode, the breakpoint will  automatically be a system mode breakpoint, which you can confirm with  the **b( )** command. Resume the system using **c( )** and wait for it to enter the interrupt handler and hit the breakpoint.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>b usrClock  </b></font>value = 0 = 0x0  -> <font color="#00a000"><b>b  </b></font>0x00022d9a: _usrClock          Task:     SYSTEM Count:  0  value = 0 = 0x0  -> <font color="#00a000"><b>c  </b></font>value = 0 = 0x0  ->   Break at 0x00022d9a: _usrClock               Task: SYSTEM

    

You can now debug the interrupt  handler. For example, you can determine which task was running when  system mode was entered using **taskIdCurrent( )** and **i( )**.

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>taskIdCurrent  </b></font>_taskIdCurrent = 0x838d0: value = 3880092 = 0x3b349c  -> <font color="#00a000"><b>i  </b></font>NAME      ENTRY      TID      PRI   STATUS  PC      SP      ERRNO  DELAY  --------- ---------- -------- ----- ------- ------- ------- -----  -----  tExcTask  _excTask   3e8a54   0     PEND    4eb8c   3e89b4  0      0  tLogTask  _logTask   3e612c   0     PEND    4eb8c   3e6088  0      0  tWdbTask  0x44d54    389774   3     PEND    46cb6   3896c0  0      0  tNetTask  _netTask   3b349c   50    READY   46cb6   3b3444  0      0    Agent mode     : Extern  System context : Suspended  value = 0 = 0x0

    

You can trace all the tasks except  the one that was running when you placed the system in system mode and  you can step through the interrupt handler. 

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>tt tLogTask  </b></font>4da78   _vxTaskEntry   +10 : _logTask (0, 0, 0, 0, 0, 0, 0, 0, 0, 0)  3f2bc   _logTask       +18 : _msgQReceive (3e62e4, 3e60dc, 20, ffffffff)  27e64   _msgQReceive   +1ba: _qJobGet ([3e62e8, ffffffff, 0, 0, 0, 0])  value = 0 = 0x0  -> <font color="#00a000"><b>l  </b></font>_usrClock  00022d9a  4856                     PEA         (A6)  00022d9c  2c4f                     MOVEA .L    A7,A6  00022d9e  61ff 0002 3d8c           BSR         _tickAnnounce  00022da4  4e5e                     UNLK        A6  00022da6  4e75                     RTS   00022da8  352e 3400                MOVE  .W    (0x3400,A6),-(A2)  00022dac  4a75 6c20                TST   .W    (0x20,A5,D6.L*4)  00022db0  3234 2031                MOVE  .W    (0x31,A4,D2.W*1),D1  00022db4  3939 382c 2031           MOVE  .W    0x382c2031,-(A4)  00022dba  343a 3337                MOVE  .W    (0x3337,PC),D2  value = 0 = 0x0  -> <font color="#00a000"><b>s  </b></font>d0   =      3e   d1   =     3700   d2     =     3000   d3     =   3b09dc  d4   =       0   d5   =        0   d6     =        0   d7     =        0  a0   =   230b8   a1   =   3b3318   a2     =   3b3324   a3     =    7e094  a4   =  38a7c0   a5   =        0   a6/fp  =    bcb90   a7/sp  =    bcb84  sr   =    2604   pc   =    230ba      000230ba  2c4f                MOVEA .L    A7,A6  value = 0 = 0x0

    

Return to task mode and confirm that return by calling **i( )**:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>sysResume</b></font>   value = 0 = 0x0  

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>i  </b></font>NAME      ENTRY      TID      PRI   STATUS  PC      SP      ERRNO  DELAY  --------- ---------- -------- ----- ------- ------- ------- -----  -----  tExcTask  _excTask   3e8f98   0     PEND    47982   3e8ef4  0      0  tLogTask  _logTask   3e6670   0     PEND    47982   3e65c8  0      0  tWdbTask  0x3f024    398e04   3     READY   405ac   398d50  30067  0  tNetTask  _netTask   3b39e0   50    PEND    405ac   3b3988  0      0  value = 0 = 0x0

    

If you want to debug an application  you have loaded dynamically, set an appropriate breakpoint and spawn a  task which runs when you continue the system:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>sysSuspend  </b></font>value = 0 = 0x0  -> ld < test.o  Loading /view/didier.temp/vobs/wpwr/target/lib/objMC68040gnutest//test.o /  value = 400496 = 0x61c70 = _rn_addroute + 0x1d4  -> <font color="#00a000"><b>b</b></font> <i>address  </i>value = 0 = 0x0  -> <font color="#00a000"><b>sp test  </b></font>value = 0 = 0x0  -> <font color="#00a000"><b>c</b></font>

    

The application breaks on _address_ when the instruction at _address_ is executed.

#### _7.2.8  Interrupting a Shell Command_

    

Occasionally it is desirable to  abort the shell's evaluation of a statement. For example, an invoked  routine may loop excessively, suspend, or wait on a semaphore. This may  happen as the result of errors in arguments specified in the invocation,  errors in the implementation of the routine itself, or simply oversight  as to the consequences of calling the routine.

    

To regain control of the shell in such cases, press the interrupt character on the keyboard, usually CTRL+BREAK from Tornado or **CTRL+C**  from the console. This makes the shell stop waiting for a result and  allows input of a new statement. Any remaining portions of the statement  are discarded and the task that ran the function call is deleted.  

<table cellpadding="2" cellspacing="0" border="0" > <tbody > <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="top" >
<td >
</td>
<td >

**CAUTION: ****CTRL+BREAK** and **CTRL+C**  do not interrupt non-blocking functions. If the task transmitting the  break request is of lower priority than the task to be interrupted, the  request is not conveyed until the original task completes. 

</td></tr> <tr valign="top" >
<td >  

</td>
<td >

* * *

</td></tr> <tr valign="center" >
<td colspan="20" >  

</td></tr></table>

    

Pressing CTRL+BREAK or **CTRL+C** is also necessary to regain control of the shell after calling a routine on the target that ends with **exit( )** rather than **return**.

    

Occasionally a subroutine invoked  from the shell may incur a fatal error, such as a bus/address error or a  privilege violation. When this happens, the failing routine is  suspended. If the fatal error involved a hardware exception, the shell  automatically notifies you of the exception. For example:

    
    
    <a rel="nofollow"></a>-> <font color="#00a000"><b>taskSpawn -4  </b></font>Exception number 11: Task: 0x264ed8 (tCallTask) 

    

In cases like this, you do not need to type CTRL+BREAK  to recover control of the shell; it automatically returns to the  prompt, just as if you had interrupted. Whether you interrupt or the  shell does it for you, you can proceed to investigate the cause of the  suspension. For example, in the case above you could run the Tornado  browser on **tCallTask**.

    

An interrupted routine may have left  things in a state which was not cleared when you interrupted it. For  instance, a routine may have taken a semaphore, which cannot be given  automatically. Be sure to perform manual cleanup if you are going to  continue the application from this point.

  
  

