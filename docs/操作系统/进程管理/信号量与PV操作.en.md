## Concept of semaphore
- A semaphore is an integer that represents the number of available resources in the system.
- When the semaphore S is greater than or equal to zero, it means that there are S resources available for concurrent processes.
- When the semaphore S is less than zero, |S| represents how many processes are waiting for the resource.

## Operation of semaphore
Semaphores can only be accessed through **PV operations** (P operations and V operations).

## Classification of semaphores
Semaphores are divided into two types according to their uses:

1. **Public semaphores**:
      - All related processes can perform P operations and V operations on them.
      - The initial value of a public semaphore is usually 1, which is used to implement mutual exclusive access of processes (for example, to ensure exclusive access to shared resources).

2. **Private semaphores**:
      - Only the process that owns the semaphore can perform P operations, and other processes can only perform V operations.
      - The initial value of a private semaphore is often 0 or a positive integer, which is used to implement process synchronization and ensure the order of certain operations.

## P operation and V operation
The P operation and V operation you described correspond to the "wait" (P) and "release" (V) operations of the semaphore, respectively. Their main functions are as follows:

1. **P operation** (wait operation):

**①**: Subtract 1 from the semaphore S.

**②**: If the value after S is subtracted by 1 is greater than or equal to zero, it means that the semaphore has resources available and the process can continue to execute.

**③**: If the value after S is subtracted by 1 is less than zero, it means that there are not enough resources, the process needs to be blocked, and is placed in the waiting queue of the semaphore, waiting for the resources to be released. At this time, the control is transferred to the system's process scheduling.

![p operation](../../photos/pcz.png)

2. **V operation** (release operation):

**①**: Add 1 to the semaphore S.

**②**: If the value after S is added by 1 is greater than zero, it means that there are resources available for other processes to use, and the current process can continue to execute.

**③**: If the value after S plus 1 is less than or equal to zero, it means that there is a process waiting for the semaphore. At this time, it is necessary to wake up a process from the waiting queue and give it control.

![v operation](../../photos/vcz.png)

## Use PV operation to achieve process mutual exclusion

### 1. Set up a mutual exclusion semaphore S to represent the critical section

- **Definition of semaphore S**; semaphore S is used to represent the resource status of the critical section. The initial value of S is set to 1, indicating that there is currently no process in the critical section and it can enter the critical section.

**The value range of S: 1, 0, negative number**

- **S = 1**: Indicates that no process is entering the critical section, that is, a process can enter.

- **S = 0**: Indicates that a process has entered the critical section and other processes must wait.

- **S < 0**: Indicates that a process has entered the critical section and |S| processes are waiting to enter the critical section.

### 2. **PV operation indicates the application and release of the critical section by the process**
PV operation (P operation and V operation) is used to control the process's entry and exit from the critical section:

- **Before entering the critical section**:
      - When the process wants to enter the critical section, it first executes the **P(S)** operation, that is, "applying for" semaphore S.
      - If the value of semaphore S is greater than 0 (S > 0), it means that there is no process in the critical section, the process can enter, and the semaphore S is reduced by 1.
      - If the value of semaphore S is 0 or negative (S <= 0), the process will be blocked and put in the waiting queue, waiting for the release of the semaphore.

- **After exiting the critical section**:
      - When the process completes the critical section operation, it executes the **V(S)** operation, that is, "releasing" semaphore S.
      - The semaphore S is increased by 1, indicating that the critical section resources are released, which may wake up a process in the waiting queue and allow it to enter the critical section.
      - If the value of semaphore S is still non-negative, other processes may continue to enter the critical section.

### 4. **Mutual Exclusion**
- The function of semaphore S is to ensure that only one process can enter the critical section at any time. When a process executes P(S), if the value of semaphore S is 1, the semaphore will be reduced to 0, indicating that it has entered the critical section and other processes must wait.
- Other processes will be blocked when the semaphore is 0 or negative until the semaphore is restored through the V(S) operation, allowing a process to enter the critical section.

This method ensures the mutual exclusion of the critical section, avoids multiple processes entering the critical section at the same time, and ensures the correct management of shared resources.

### 5. **Note**
- Since P(S) and V(S) are atomic operations, they cannot be interrupted, so no race conditions will occur.
- The value of the semaphore may be negative, indicating the number of processes waiting to enter the critical section. This mechanism allows multiple processes to access the critical section in an orderly manner without causing deadlock.

## Use PV operation to synchronize processes
Your summary is very comprehensive. Here is a detailed explanation and a practical example of how to synchronize processes using PV operation:

### Synchronization mechanism
Synchronization mechanism is used to ensure that multiple concurrent processes can collaborate correctly and orderly, avoid data conflicts and ensure data consistency. Typical synchronization mechanisms include PV operation and pipe.

### PV operation to synchronize processes
Semaphore S can be used to represent an expected message. Its value "0" indicates that the message has not been generated yet, and its value is not "0" indicates that the message already exists.

#### Steps:
1. **P operation**: Test whether the message has arrived.
- If S>0, the process continues to execute and enters the critical section.
- If S<=0, the process enters the waiting state until S>0.

2. **V operation**: Send a message.
- Increase the value of semaphore S to wake up the process blocked by waiting for the semaphore.

#### Notes:
- When defining semaphores, its meaning should be clarified according to the specific problem.
- Each semaphore is associated with a specific message. When there are multiple messages, multiple semaphores need to be defined.
- Call the corresponding P operation or V operation for different semaphores.