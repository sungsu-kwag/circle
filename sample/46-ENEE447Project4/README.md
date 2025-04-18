# ENEE447 2025 Spring Project 4: Running a Task in User Mode with Its Own Virtual Memory Space  

## What are we doing in this project?  
In this project, we will run **one task in user mode** and give it **its own virtual‑memory (VM) space**.

- **Q:** *Why would we want to do that?*  
- **A:** For the same reasons modern OSes do:  
	- **Principle of least privilege.** User code can be buggy or hostile; if it runs with minimal privilege it can do minimal harm.  
	- **Stronger memory isolation.**  
		- Each task’s page table maps the *same* virtual addresses to *different* physical pages, so tasks cannot clobber each other’s data.  
		- Sometimes controlled sharing is useful (shared libraries, inter‑process communication, …). That requires extra kernel support that we do **not** implement here.  
		- Kernel pages can be marked privileged so user code cannot even read them.  

## Specifically, we are trying to solve the following two problems  

### Before you start  
Copy your **Project 3** solution to the locations below so Project 4 can compile (We will publish the solution after the extended deadline of project 3, you can use your own solution for starting.):  

- [`taskswitch.S`](../../lib/sched/taskswitch.S#L27-L30)  
- [`scheduler.cpp`](../../lib/sched/scheduler.cpp#L1)  

### Problem 1: Set up virtual memory for the user‑mode task  
- **Current behaviour.** The demo crashes immediately:  

  <img src="img/project 4 after copying p3 sol_part 1 init state_Run called.png" width="500">  

- **Goal.** After you set up VM correctly, the program runs silently (no output yet, no crash):  

  <img src="img/project 4 after impl vm_user task runs but no output_need syscall impl.png" width="500">  

#### Specifically, to solve Problem 1 you should  
1. Complete the `TODO`s in [`task.cpp`](../../lib/sched/task.cpp#L204-L260).  
2. Edit [`task.h`](../../include/circle/sched/task.h) if needed.  

### Problem 2: Implement system calls so the user task can trap into kernel mode  
At the end of Problem 1 the user task is alive but mute; all its library syscalls are stubs.  
Once syscalls are handled, you should see output like this:

<img src="img/project 4 after impl syscall_user task now runs and print.png" width="500">  

#### Specifically, to solve Problem 2 you should  
- Fill in the `TODO`s in [`syscallhandler.cpp`](../../lib/syscallhandler.cpp#L10).  

## What to submit on ELMS
1. **One PDF** that contains  
	- The names of all group members.  
	- A screenshot (or photo) proving *Problem 1* is solved.  
	- A screenshot (or photo) proving *Problem 2* is solved.  
2. Your modified `task.cpp` (and `task.h` if changed).  
3. Your modified `syscallhandler.cpp`.  
4. A **detailed explanation** of what you changed and why.  

## Documents for reference  
- [1] [ARM Architecture Reference Manual](https://documentation-service.arm.com/static/5f8dacc8f86e16515cdb865a)  
- [2] [ARM1176JZF‑S Technical Reference Manual](https://developer.arm.com/documentation/ddi0301/latest/)  
	- Raspberry Pi Zero uses the ARM1176JZF‑S CPU.  
	- **NOTE:** The core supports two ISAs—ARM and Thumb. We use **ARM ISA** only; ignore text that refers to *Thumb state*.  
		- For example, read “The **ARM‑state** core register set” instead of “The Thumb‑state core register set.”  
- [3] [ARM Procedure Call Standard](https://developer.arm.com/documentation/dui0041/c/ARM-Procedure-Call-Standard)  
