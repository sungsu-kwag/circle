# ENEE447 2025 Spring Project 1: Prioritize Scheduling Task

## Purpose

In this project, you will understand how the current **cooperative non-preemptive scheduler** (round-robin without priority) works and modify it to implement **priority scheduling**, where tasks with higher priority are executed first.

### Key Modifications
- We will modify the scheduler in Circle to implement priority scheduling.
  - Currently, the scheduler is a ["cooperative non-preemptive scheduler that uses the round-robin policy, without priorities"](https://github.com/sklaw/circle/blob/master/include/circle/sched/scheduler.h#L31-L33).
    - What is a cooperative non-preemptive scheduler? Read this Wikipedia article on [cooperative multitasking](https://en.wikipedia.org/wiki/Cooperative_multitasking).
  - We want to modify the scheduler so that it prioritizes tasks with the highest priority.

### Priority Scheduling Rules
1. **Higher priority tasks execute first.**
2. **If tasks have the same priority, they execute in the following order:**
   - `PrimeTask` → `LEDTask` → `ScreenTask`


## Required Tasks

### Understanding the System
0. Familiarize yourself with how the operating system and application (`sample/43-ENEE447Project1`) work.

### Implementation
1. Modify the task scheduler so that higher-priority tasks execute first.
2. Implement secondary prioritization:
   - If multiple tasks have the same priority, execute them in the order:
     - **PrimeTask** → **LEDTask** → **ScreenTask**
3. Test your implementation:
   - Modify the task order and priority of each tasks in `kernel.cpp`.


## How to test your code?

**How to build modified raspberry pi OS**
- kernel.img must be generated in the sample/43-ENEE447Project1 directory path.

```bash
# If you have modified source codes of the library (any code in lib or include)
./makeall clean && ./makeall

# If you have modified only codes in the sample but not in the library
cd sample/43-ENEE447Project1
make clean && make

```

**How to run on QEMU**
```bash
qemu-system-arm -M raspi0 -bios sample/43-ENEE447Project1/kernel.img
```

## Required Submission (Due Date : 3/2)

Submit the following files:

* Modified `scheduler.cpp` file, where you have updated the `GetNextTask` function.
* A PDF file containing:
	* Names of your group members.
	* Description of how current scheduling works
	* Screenshot(s) of your QEMU output demonstrating priority scheduling.
	* Description of how you implemented priority scheduling.
	* Answer to the Question: To block context switching, which line should be commented out in each task? Explain why.
    **(or you can put the most significant function name (from scheduler.cpp) that involved in context switching and describe it.)**

### Useful Link

* Understanding circle structure and functions in circle environment. 
	* circle-rpi documentation: https://circle-rpi.readthedocs.io/
