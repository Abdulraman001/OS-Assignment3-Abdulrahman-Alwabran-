# Assignment 3 - Complete Documentation

**Student Name**: [Abdulrahman kahled alwbran ]  
**Student ID**: [445050098]  
**Date Submitted**: [1/5/2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: https://drive.google.com/file/d/1DJwpA9Onyeuli9pbqhePjCu8noZVaZYG/view?usp=drivesdk

**Video filename**: `445050098_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Date, Time]
**What I implemented**: Basic Round Robin scheduling and Process class structure. 

**Challenges encountered**: Processes were finishing instantly or out of order.

**How I solved it**:Implemented Thread.sleep() to simulate real execution time. 

**Testing approach**:Printing process IDs to the console to verify order. 

**Time spent**: Time spent: 3 Hours

---

### Entry 2 - [Date, Time]
**What I implemented**: Student ID seed logic for random data generation.

**Challenges encountered**: Random values changed every run, making testing impossible.

**How I solved it**:  Used Random(seed) to ensure identical results for the same ID.

**Testing approach**: Running the code 3 times with the same ID to check consistency. 

**Time spent**:1 Hour 

---

### Entry 3 - [Date, Time]
**What I implemented**: Semaphores for CPU mutual exclusion.

**Challenges encountered**:  Multiple threads were "executing" on the CPU simultaneously.

**How I solved it**: Wrapped the execution block with semaphore.acquire() and release().

**Testing approach**: Monitoring console logs to ensure only one "Start" appears before an "End".

**Time spent**: 2.5 Hours 

---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?  ( contextSwitchCount )
- Why is concurrent access a problem? ( Because its giving me incorrect data )
- What incorrect behavior could occur? ( it is possible to enter different data into the contextSwitchCount)

**Your Answer**:

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
(The main difference between a lock and a semaphore is that a lock allows only one thread to access a resource at a time, while a semaphore allows multiple threads to access a resource based on a specific limit. Locks are used for exclusive access, whereas semaphores are used to manage shared resources .)

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

(Deadlock is a situation where two or more processes are waiting for each other indefinitely, causing the system to stop progressing.

Two prevention techniques:

Resource ordering: Resources are requested in a fixed order.
Avoid hold and wait: A process must request all needed resources at once.

In my code, deadlocks were prevented by using resource ordering and ensuring threads released resources properly after use.)

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

Lock Design: Fine-Grained
 Choice: Separate locks for each counter.
 Why: Counters are independent; no need to block one to update another.
 Trade-offs: Better speed/concurrency vs. higher complexity.
 Best for Concurrency: Fine-grained, because it minimizes lock contention, allowing simultaneous updates.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**:  contextSwitchCount, completedProcessCount, totalWaitingTime.

**Why they need protection**: To prevent race conditions and ensure data accuracy when multiple threads update them simultaneously.

**Synchronization mechanism used**: Fine-grained ReentrantLock

**Code snippet**:contextSwitchLock.lock();
try {
    contextSwitchCount++;
} finally {
    contextSwitchLock.unlock();
}

```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**:  executionLog (an ArrayList of Strings).

**Why it needs protection**:ArrayList is not thread-safe. Concurrent access by multiple threads can cause data corruption or ConcurrentModificationException. 

**Synchronization mechanism used**:  ReentrantLock (logLock).

**Code snippet**:
 logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}


```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**:  To ensure Mutual Exclusion by allowing only one process to execute on the CPU at any given time, preventing execution overlap.

**Number of permits and why**: 1 permit. It acts as a Binary Semaphore (Mutex) to guarantee that the CPU is treated as a non-sharable resource.

**Where implemented**: Inside the run() and runToCompletion() methods of the Process class.

**Code snippet**: SharedResources.cpuSemaphore.acquire();
try {
    // Process execution logic
} finally {
    SharedResources.cpuSemaphore.release();
}
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**:  What I tested: Running the program multiple times to verify consistent results.
1 Run the simulation multiple times using the same Student ID (Seed).
2 Monitor the Total Context Switches and Average Waiting Time in each run.
3 Verify that all values (Burst Times, Process order, and Final Statistics) remain identical across all runs, ensuring the simulation is deterministic and the synchronization is stable.
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 1 Initialization: The program uses the Student ID as a seed to generate consistent random processes and adds them to a Ready Queue.

 2 Scheduling (Round Robin): The CPU executes each process for a specific Time Quantum. If unfinished, the process moves to the back of the queue.
 
 3. Synchronization:
    Semaphore: Acts as a gatekeeper to ensure only one process uses the CPU at a time.
    Fine-grained Locks: Safely update shared counters (waiting time, context switches) without data conflict.
 4. Final Output: Once all processes finish, the system displays a Performance Table and synchronization statistics.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**:  Multiple threads simultaneously add entries to the ArrayList while the system updates shared counters.

**Results**: Zero exceptions. The logs were recorded correctly and the final count was accurate. 

**What this proves**: The logLock (ReentrantLock) effectively synchronizes the non-thread-safe ArrayList, ensuring data integrity during concurrent access.

---

### Test 3: Correctness Verification
**What I tested**:  Verifying final statistics (Total Burst Time, Context Switches).

**Expected values**: Total Burst Time = Sum of all process burst times; Context Switches = Number of execution turns minus one. 

**Actual values**: Matches exactly with the sum of manual calculations from the console log. 

**Analysis**:  The statistics are 100% accurate, proving that the Fine-grained locks prevent lost updatnd maintain perfect data integrity.

---

### Test 4: Different Scenarios
**Scenario tested**:  Increasing the Time Quantum (e.g., from 2000ms to 5000ms).

**Purpose**:  To observe the impact of a larger quantum on Context Switching and Waiting Time.

**Results**: Total context switches decreased, while the average waiting time for shorter processes increased.

**What I learned**: A larger quantum improves efficiency by reducing overhead (fewer switches) but reduces system responsiveness for smaller tasks.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

Synchronization is vital to prevent Race Conditions and data corruption. I learned that Semaphores control CPU access, while Locks protect shared variables like logs. It ensures that multi-threaded tasks remain Atomic and consistent. Without it, concurrent updates would fail, leading to inaccurate system statistics.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking. Prevents simultaneous withdrawals from exceeding the balance.

**Example 2**: Flight Booking. Prevents booking the same seat for two different people.

---

### How I would explain synchronization to others:

It’s like a talking stick in a circle; only the person holding it can speak. This prevents everyone from talking at once, ensuring the conversation stays clear and organized.

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Abdulraman001/OS-Assignment3-Abdulrahman-Alwabran-/edit/main/ASSIGNMENT_DOCUMENTATION.md

**Number of commits**: 18

**Commit messages**: 
1. Setup Round Robin logic.
2. Added Semaphores/Locks.
3. Added ID Seed logic.
4. Final UI and Stats.

---

## Summary

**Total time spent on assignment**:  7 hours

**Key takeaways**: 
1 Synchronization is essential to prevent data corruption in multi-threading.
2. Semaphores effectively manage shared hardware resources like the CPU.
3. Seed logic ensures that random simulations are consistent and testable.

**Most challenging aspect**: Implementing Fine-grained locking to protect shared counters without causing performance bottlenecks or deadlocks.

**What I'm most proud of**: Successfully creating a stable, thread-safe simulation that produces 100% accurate statistics every time.

---

**End of Documentation**
