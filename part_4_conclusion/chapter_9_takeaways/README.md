# Chapter 9: Takeaways

## Observations

### The Asynchronous Runtime Pattern

While writing this report I noticed a pattern that repeats itself over and over. I call it the <span style="color:red;">**Asynchronous Runtime Pattern**</span>. If we want to categorize it this pattern would fall under the behavioural/architectural patterns umbrella.

The main elements of the Asynchronous Runtime Pattern are:

1. A task stack (exactly 1)
2. An event-loop/watcher
3. A task queue (1 or more)
4. Workers (2 or more)

### Instances that implement the Asynchronous Runtime Pattern

The following are some use-case examples that implement the Asynchronous Runtime Pattern on different levels of abstraction.

1. on the software level:
   - **Web Servers**:
       1. A task stack => client requests
       2. An event-loop/watcher => the monitoring process
       3. A task queue => request queue
       4. Workers => worker threads/processes
   - **The Javascript Runtime**:
       1. A task stack => callstack
       2. An event-loop/watcher => event-loop
       3. A task queue => task-queue + promises-queue
       4. Workers => runtime worker threads 
   - **Backend Job Processing**:
       1. A task stack => callstack
       2. An event-loop/watcher => the monitoring process/thread
       3. A task queue => the messaging queue
       4. Workers => worker threads/processes 
2. on the hardware level:
   - ****:
3. on the organizational level:
   - **scrum**: 
       1. A task stack => Product Backlog
       2. An event-loop/watcher => Iteration
       3. A task queue => Sprint Backlog
       4. Workers => Autonomous Workers
## Future Insights

### Keep An Eye On

1. **AdonisJS**
2. **WebAssembly**