# Philosophers

## 📘 Project Overview

**Philosophers** is a multithreading project from the 42 curriculum that explores the classic dining philosophers problem. The goal is to learn the basics of threading processes, create threads, and understand the use of mutexes and semaphores to prevent data races and deadlocks.

> **Disclaimer:**  
> This document is an unofficial summary written for educational and documentation purposes.  
> It is not affiliated with or endorsed by 42 or its partners.  
> All 42 students are responsible for adhering to the academic integrity policy.  
> You may **not** publish or share any part of the official subject PDF, evaluation scripts, or Moulinette content.

---

## Contents

- [Goals](#goals)
- [The Problem](#the-problem)
- [General Requirements](#general-requirements)
- [Program Arguments](#program-arguments)
- [Log Format](#log-format)
- [Mandatory Part](#mandatory-part)
- [Bonus Part](#bonus-part)
- [Submission Guidelines](#submission-guidelines)

---

## Goals

- Learn the basics of threading a process
- Understand thread creation and management
- Explore the use of mutexes for synchronization
- Prevent data races and deadlocks
- Implement process-based solutions with semaphores (bonus)

---

## The Problem

The dining philosophers problem consists of:

- **One or more philosophers** sit at a round table
- There is a **large bowl of spaghetti** in the middle of the table
- **Philosophers alternate** between three states: eating, thinking, and sleeping
- There are **forks on the table** (as many forks as philosophers)
- A philosopher must **pick up both forks** (left and right) to eat spaghetti
- When finished eating, philosophers **put forks back** and start sleeping
- After sleeping, they start thinking again
- **The simulation stops** when a philosopher dies of starvation

### Key Rules
- Philosophers cannot communicate with each other
- Philosophers don't know if another philosopher is about to die
- **Philosophers should avoid dying!**

---

## General Requirements

- Written in **C**, following the **42 Norm**
- No memory leaks or undefined behavior
- **Global variables are forbidden!**
- Must compile with `-Wall -Wextra -Werror`
- **No data races** allowed
- Death messages must be displayed within **10 ms** of actual death

---

## Program Arguments

Both mandatory and bonus programs take the same arguments:

```
./philo number_of_philosophers time_to_die time_to_eat time_to_sleep [number_of_times_each_philosopher_must_eat]
```

- **number_of_philosophers**: Number of philosophers and forks
- **time_to_die** (ms): Max time since last meal before death
- **time_to_eat** (ms): Time needed to eat (holding two forks)
- **time_to_sleep** (ms): Time spent sleeping
- **number_of_times_each_philosopher_must_eat** (optional): Simulation stops when all philosophers have eaten this many times

### Philosopher Numbering
- Each philosopher has a number from **1** to **number_of_philosophers**
- Philosopher **1** sits next to philosopher **number_of_philosophers** (circular table)
- Philosopher **N** sits between philosophers **N-1** and **N+1**

---

## Log Format

Every state change must be formatted as:
```
timestamp_in_ms X has taken a fork
timestamp_in_ms X is eating
timestamp_in_ms X is sleeping
timestamp_in_ms X is thinking
timestamp_in_ms X died
```

- **timestamp_in_ms**: Current timestamp in milliseconds
- **X**: Philosopher number
- **No message overlap** allowed
- **Death messages** must appear within 10ms of actual death

---

## Mandatory Part

**Program name:** `philo`  
**Directory:** `philo/`  
**External functions allowed:**
- `memset`, `printf`, `malloc`, `free`, `write`
- `usleep`, `gettimeofday`
- `pthread_create`, `pthread_detach`, `pthread_join`
- `pthread_mutex_init`, `pthread_mutex_destroy`
- `pthread_mutex_lock`, `pthread_mutex_unlock`

### Implementation Rules
- **Each philosopher** = separate **thread**
- **One fork** between each pair of philosophers
- **Fork states** protected by **mutexes**
- If only **one philosopher**, they have access to just **one fork**

### Makefile Requirements
Must include rules: `$(NAME)`, `all`, `clean`, `fclean`, `re`

---

## Bonus Part

**Program name:** `philo_bonus`  
**Directory:** `philo_bonus/`  
**External functions allowed:**
- `memset`, `printf`, `malloc`, `free`, `write`
- `fork`, `kill`, `exit`, `waitpid`
- `pthread_create`, `pthread_detach`, `pthread_join`
- `usleep`, `gettimeofday`
- `sem_open`, `sem_close`, `sem_post`, `sem_wait`, `sem_unlink`

### Implementation Rules
- **All forks** are in the middle of the table
- **Fork availability** represented by a **semaphore**
- **Each philosopher** = separate **process**
- **Main process** should NOT act as a philosopher

> **Note:** Bonus part will only be assessed if the mandatory part is **PERFECT**

---

## Submission Guidelines

- Submit to assigned Git repository
- Only repository contents will be evaluated
- Mandatory part directory: `philo/`
- Bonus part directory: `philo_bonus/`
- Double-check file names for correctness

### Evaluation Process
- Peer evaluations
- Possible automated testing via **Deepthought**
- Any error stops evaluation immediately

---

## Testing Recommendations

Create test programs to verify:
- **No data races** or deadlocks
- **Proper synchronization** between threads/processes
- **Accurate timing** and state transitions
- **Death detection** within 10ms
- **Edge cases** (1 philosopher, large numbers, etc.)

---

## Key Concepts to Master

- **Thread synchronization** with mutexes
- **Process communication** with semaphores
- **Race condition prevention**
- **Deadlock avoidance**
- **Precise timing** with `gettimeofday`
- **Memory management** in multithreaded environments

---

## Final Note

This project teaches fundamental concepts of concurrent programming that are essential for system-level development. Understanding synchronization primitives and avoiding race conditions are critical skills for any systems programmer.

---
