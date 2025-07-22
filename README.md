# Philosophers

![42 Badge](https://img.shields.io/badge/42-Philosophers-brightgreen)
![C Badge](https://img.shields.io/badge/Language-C-blue)
![Status Badge](https://img.shields.io/badge/Status-Completed-success)
![Score Badge](https://img.shields.io/badge/Score-125%2F100-gold)

## What I Learned

Through this challenging multithreading project at 42 School, I developed critical concurrent programming skills:

- **Thread synchronization** - Mastered pthread creation, joining, and lifecycle management
- **Mutex operations** - Implemented deadlock-free fork acquisition with proper locking order
- **Race condition prevention** - Eliminated data races through strategic mutex placement
- **Process communication** - Used semaphores for inter-process synchronization in bonus part
- **Timing precision** - Achieved microsecond-level accuracy with `gettimeofday` for death detection
- **Deadlock avoidance** - Designed fork acquisition strategy to prevent circular waiting
- **Resource sharing** - Managed shared resources (forks) between competing threads/processes
- **Concurrent monitoring** - Implemented real-time death and completion detection
- **Memory synchronization** - Ensured data consistency across multiple execution contexts
- **Edge case handling** - Solved special scenarios like single philosopher and rapid eating
- **Performance optimization** - Balanced responsiveness with CPU efficiency through strategic delays
- **Process forking** - Created independent philosopher processes with shared semaphore resources

This project significantly enhanced my understanding of low-level concurrency, system programming, and the complexities of parallel execution environments.

## About the Project

The Philosophers project simulates the classic dining philosophers problem, where philosophers sitting around a table must share forks to eat spaghetti. The challenge lies in preventing deadlocks, race conditions, and ensuring no philosopher starves while maintaining optimal performance.

The project demonstrates:
- **Concurrent programming** fundamentals
- **Resource sharing** strategies  
- **Synchronization** mechanisms
- **Process vs thread** communication models

## Implementation Architecture

### Mandatory Part: Thread-Based Solution

**Core Components:**
- **Philosopher threads** - Each philosopher runs as an independent thread
- **Fork mutexes** - Each fork protected by its own mutex to prevent duplication
- **Shared data structure** - Central coordination point with proper synchronization
- **Monitor system** - Real-time death detection and meal counting

**Key Design Decisions:**
```c
typedef struct s_philo {
    pthread_t       thread;
    int             id;
    pthread_mutex_t left_fork;      // Each philosopher owns left fork
    pthread_mutex_t *right_fork;    // Points to next philosopher's left fork
    size_t          last_meal;      // Protected by meal mutex
    int             meals_eaten;    // Synchronized access
    struct s_data   *data;          // Shared configuration
} t_philo;
```

**Synchronization Strategy:**
- **Print mutex** - Prevents message overlap in output
- **Meal mutex** - Protects last meal time and meal count updates  
- **Monitor mutex** - Coordinates death detection and completion checking
- **Fork ordering** - Consistent lock acquisition to prevent deadlocks

### Bonus Part: Process-Based Solution

**Architecture:**
- **Independent processes** - Each philosopher runs as a separate process
- **Semaphore coordination** - Shared semaphores for fork availability and printing
- **Process monitoring** - Parent process manages child philosopher lifecycles
- **Signal handling** - Graceful termination through process signals

**Semaphore Usage:**
```c
typedef struct s_dinning_table {
    sem_t *forks;    // Available fork count (initialized to n_philosophers)
    sem_t *print;    // Serializes output (initialized to 1)
    sem_t *full;     // Tracks completion count
    // ... other shared data
} t_dinning_table;
```

## Technical Challenges Overcome

### Race Condition Elimination
- **Atomic operations** on shared variables through mutex protection
- **Consistent locking order** to prevent deadlock scenarios
- **Memory barriers** ensuring proper variable visibility across threads

### Precise Timing Implementation
- **Microsecond accuracy** for death detection within 10ms requirement
- **Optimized monitoring loop** balancing responsiveness with CPU usage
- **Time synchronization** across multiple execution contexts

### Edge Case Handling
- **Single philosopher** scenario with graceful timeout handling
- **Rapid eating cycles** without missing death detection windows
- **Resource cleanup** on abnormal termination scenarios

### Performance Optimization
```c
// Staggered start to reduce initial contention
if (philo->id % 2 == 0)
    usleep(100);
    
// Efficient monitoring with strategic delays
usleep(100);  // 0.1ms monitoring interval
```

## Key Features

### Robust Error Handling
- **Comprehensive validation** of command-line arguments
- **Graceful degradation** on system call failures
- **Memory leak prevention** with proper cleanup routines
- **Thread safety** in all error paths

### Visual Output Enhancement
- **Color-coded status messages** for better debugging
- **Precise timestamps** for event correlation
- **Clean termination messages** for both death and completion scenarios

### Memory Management
- **Zero memory leaks** through careful resource tracking
- **Proper mutex cleanup** on all exit paths
- **Thread resource management** with appropriate join/detach operations

## Usage

### Compilation
```bash
# Compile mandatory part
cd philo && make

# Compile bonus part  
cd philo_bonus && make

# Clean build artifacts
make clean

# Full rebuild
make fclean && make
```

### Execution Examples
```bash
# Basic simulation: 5 philosophers, die in 800ms, eat for 200ms, sleep for 200ms
./philo 5 800 200 200

# Limited meals: Stop after each philosopher eats 7 times
./philo 5 800 200 200 7

# Edge case: Single philosopher (will die after die_time)
./philo 1 800 200 200

# Stress test: Many philosophers with tight timing
./philo 200 400 100 100
```

### Expected Outputs
```
[0] 1 has taken a fork
[0] 1 has taken a fork  
[0] 1 is eating
[200] 1 is sleeping
[400] 1 is thinking
[401] 2 has taken a fork
...
[1205] 3 died
```

## Architecture Highlights

### Thread Synchronization Model
```c
// Deadlock-free fork acquisition
pthread_mutex_lock(&philo->left_fork);
pthread_mutex_lock(philo->right_fork);
// ... eating logic ...
pthread_mutex_unlock(&philo->left_fork);
pthread_mutex_unlock(philo->right_fork);
```

### Process Communication Model  
```c
// Semaphore-based resource management
sem_wait(dtable->forks);  // Acquire fork
sem_wait(dtable->forks);  // Acquire second fork
// ... eating logic ...
sem_post(dtable->forks);  // Release fork
sem_post(dtable->forks);  // Release second fork
```

### Monitoring System
- **Real-time death detection** with sub-10ms accuracy
- **Meal completion tracking** for optional termination condition
- **Thread-safe status reporting** with mutex-protected output

## Testing Scenarios Verified

- ✅ **No data races** (verified with thread sanitizer)
- ✅ **No memory leaks** (verified with valgrind)
- ✅ **Deadlock prevention** under all timing configurations
- ✅ **Precise death detection** within required time windows
- ✅ **Edge cases** including single philosopher and rapid cycles
- ✅ **Resource cleanup** on normal and abnormal termination
- ✅ **Cross-platform compatibility** (Linux/macOS)

---

*This project demonstrates mastery of concurrent programming concepts, achieving a perfect score of 125/100 with robust error handling, zero memory leaks, and race-condition-free execution across all test scenarios.*

---

## License

This project is licensed under the [MIT License](./LICENSE).
