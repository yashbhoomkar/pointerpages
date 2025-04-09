---
layout: single
title: "Process Scheduling Algorithms in Operating Systems"
date: 2025-02-15 09:00:00 +0530
categories:
  - operating systems
tags:
  - os
  - scheduling
  - round robin
  - fifo
  - sjf
  - priority
  - cpu
  - interview
toc: true
classes: wide
---

## 🧠 Introduction

In a multitasking operating system, **process scheduling** is the key to managing how the CPU is shared among multiple processes.

> A scheduler decides **which process gets the CPU**, for how long, and in what order.

This blog breaks down the **most common scheduling algorithms** in OS: FIFO, Round Robin, SJF, Priority, and Multilevel Queue.

---

## ⚙️ Terminologies First

- **Burst Time (BT)**: Time a process needs on the CPU
- **Arrival Time (AT)**: When the process enters the ready queue
- **Waiting Time (WT)**: Time process waits in the queue
- **Turnaround Time (TAT)**: Time from arrival to completion  
  \> `TAT = Completion Time - Arrival Time`  
  \> `WT = TAT - Burst Time`

---

## 1️⃣ FIFO / FCFS (First Come First Serve)

**Concept**: Processes are scheduled in the order they arrive.

| Pros        | Cons                      |
|-------------|---------------------------|
| Simple      | Convoy effect: long processes delay short ones |
| Fair in order | Poor average waiting time |

📝 Example:
Arrival: P1, P2, P3 BT: 4 3 2 → Order: P1 → P2 → P3

yaml
Copy
Edit

---

## 2️⃣ SJF (Shortest Job First)

**Concept**: Schedule the process with the **smallest burst time** first.

| Pros          | Cons                          |
|---------------|-------------------------------|
| Optimal avg WT| Starvation of long processes  |
|               | Requires BT prediction        |

📝 Example:
BT: P1=6, P2=2, P3=1 → Order: P3 → P2 → P1

yaml
Copy
Edit

---

## 3️⃣ Round Robin (RR)

**Concept**: Each process gets a **fixed time slice** (quantum), then moves to the back of the queue.

| Pros                  | Cons                        |
|-----------------------|-----------------------------|
| Fair CPU sharing      | Depends on quantum size     |
| Great for time-sharing| High context switch overhead|

📝 Example (Quantum = 2):

Queue: P1=4, P2=3, P3=5
Cycle: P1(2) → P2(2) → P3(2) → P1(2) → P2(1) → P3(3)

yaml
Copy
Edit

---

## 4️⃣ Priority Scheduling

**Concept**: Each process has a priority. Highest priority runs first.

| Pros                    | Cons                      |
|-------------------------|---------------------------|
| Flexible control        | Starvation of low priority |
| Great for real-time apps| Aging needed to prevent starvation |

📝 Example:
Priorities: P1=3, P2=1, P3=2 → Order: P2 → P3 → P1

yaml
Copy
Edit

---

## 5️⃣ Multilevel Queue Scheduling

**Concept**: Processes are grouped (interactive, batch, etc.) and each group has its own scheduling algorithm.

- Each queue has different priority.
- No process moves between queues (static).
- RR can be used inside queues.

🧠 Useful for systems with **distinguished process types**.

---

## 📊 Comparison Table

| Algorithm      | Preemptive | Starvation Possible | Best For              |
|----------------|------------|----------------------|------------------------|
| FCFS           | ❌         | ❌                   | Simplicity            |
| SJF            | ❌/✅      | ✅                   | Batch systems         |
| Round Robin    | ✅         | ❌                   | Time-sharing systems  |
| Priority       | ✅         | ✅                   | Real-time systems     |
| Multilevel     | ✅/❌      | Depends              | Complex environments  |

---

## 🧪 Pro Tip: Visualizing With Gantt Charts

When practicing scheduling questions, always draw a **Gantt chart** to understand execution flow and calculate:

- Completion Time (CT)
- Waiting Time (WT)
- Turnaround Time (TAT)

---

## 💡 Final Thoughts

Understanding process scheduling is **core to operating systems** and is often asked in interviews and exams.

Each algorithm is designed for specific use cases — there’s no one-size-fits-all. It's all about choosing the right tool for the job!

Want a post covering **Deadlocks**, **Memory Management**, or **Page Replacement Algorithms** next? Let me know 💡
