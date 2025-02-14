# ENEE447 2024 Spring Project 2: Preemptive Multitasking (Initiated by Timer Interrupt)

## Purpose
- In this project, you will design and implement **preemptive multitasking** in Circle by utilizing an IRQ handler triggered by timer interrupts.
  - **Preemptive multitasking** leverages hardware interrupts (IRQ) to forcibly suspend the currently running task and invoke the scheduler, ensuring that all tasks receive CPU time even if they do not yield voluntarily.
  - The main focus is on designing the preemptive multitasking mechanism within the IRQ handler, so that context switches occur correctly whenever the timer interrupt fires.

## Required Tasks
1. **Examine the Task Code:**
   - Review the [`Run` function in `kernel.cpp`](kernel.cpp#L78-L116) and the [`Run` function in `testtask.cpp`](testtask.cpp#L17-L34).
     - **Note:** Unlike project 1, none of these tasks call `Yield`. They run indefinitely inside a `while(1)` loop, so cooperative multitasking won’t work. Preemptive multitasking is necessary to switch between these tasks.
2. **Understand the Preemptive Setup:**
   - Locate the call to `EnablePreemptiveMultitasking` in `kernel.cpp` ([see lines L98-L99](kernel.cpp#L98-L99)). Read its definition in [`scheduler.cpp`](../../lib/sched/scheduler.cpp#L466-L480).
     - Notice that besides initializing global variables, `EnablePreemptiveMultitasking` registers a periodic handler (`a_simple_timer_interrupt_handler`) which is called on every timer interrupt.
3. **Review the Timer Interrupt Handler:**
   - Study [`a_simple_timer_interrupt_handler` in `scheduler.cpp`](../../lib/sched/scheduler.cpp#L452-L463).
     - **Important:** This function does not perform the context switch directly. Instead, it sets the flag `should_contextswith_on_irq_return` so that the context switch is handled later in the IRQ exit code.
4. **Implement Context Switching:**
   - Examine [`IRQStub` in `exceptionstub.S`](../../lib/exceptionstub.S#L80) and implement the `TODO`s under the label `do_context_switch_on_irq_return`.
   - Read the code and comments in [`ContextSwitchOnIrqReturn_by_modifyingTaskContextSavedByIrqStub` in `scheduler.cpp`](../../lib/sched/scheduler.cpp#L482) and complete the `TODO` within this function.

## Expected Results
- **Current Behavior:**
  - When running the sample, the output shows only the main task executing:
    ```
    logger: Circle 45.1 started on Raspberry Pi Zero
    00:00:00.54 timer: SpeedFactor is 1.85
    00:00:00.54 kernel: Compile time: Feb 25 2023 15:03:24
    Task main is running.
    Task main is running.
    Task main is running.
    Task main is running.
    Task main is running.
    Task main is running.
    Task main is running.
    ```
- **Expected Behavior After Modification:**
  - After implementing preemptive multitasking with interrupt handler, you should see four tasks running concurrently: **main task**, **task A**, **task B**, and **task C**. For example:
    ```
    logger: Circle 45.1 started on Raspberry Pi Zero
    00:00:00.54 timer: SpeedFactor is 1.85
    00:00:00.54 kernel: Compile time: Feb 25 2023 15:03:24
    Task main is running.
    00:00:01.54 sched: Current task is task main, will switch to task A.
    Task A is running.
    00:00:02.54 sched: Current task is task A, will switch to task B.
    Task B is running.
    00:00:03.54 sched: Current task is task B, will switch to task C.
    Task C is running.
    00:00:04.54 sched: Current task is task C, will switch to task main.
    Task main is running.
    Task main is running.
    00:00:05.54 sched: Current task is task main, will switch to task A.
    Task A is running.
    Task A is running.
    ```

## Required Submission (Due Date : 3/2)
Submit the following on ELMS before your lab:
1. **Source Code Modifications:**
   - The modified `exceptionstub.S` file (with your implementations under the label `do_context_switch_on_irq_return`).
   - The modified `scheduler.cpp` file (with your implementation of `ContextSwitchOnIrqReturn_by_modifyingTaskContextSavedByIrqStub`).
2. **Documentation (PDF):**
   - Names of your group members.
   - A screenshot or photo showing the output where all four tasks (main task, task A, task B, and task C) are running concurrently.
   - Answers (2-3 sentences each) to the following questions:
     - What are the differences between project 1 and project 2 in terms of context switching?
     - What is IRQ mode?

### Useful Link
- **[ARM Architecture Reference Manual](https://documentation-service.arm.com/static/5f8dacc8f86e16515cdb865a)**
- **[ARM1176JZF-S Technical Reference Manual](https://developer.arm.com/documentation/ddi0301/latest/)**
  - The Raspberry Pi Zero uses the ARM1176JZF-S processor. Note that this processor supports both the ARM ISA and the Thumb ISA. For this project, focus on the ARM ISA.
  - For instance, when reviewing the available registers, refer to [The ARM state core register set](https://developer.arm.com/documentation/ddi0301/h/programmer-s-model/registers/the-arm-state-core-register-set?lang=en) instead of the Thumb register set.
- **[ARM Procedure Call Standard](https://developer.arm.com/documentation/dui0041/c/ARM-Procedure-Call-Standard)**
