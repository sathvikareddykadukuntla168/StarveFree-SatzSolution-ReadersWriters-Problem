# StarveFree-SatzSolution-ReadersWritersProblem

This repository contains a solution for starve free readers-writers problem. This also can be said as 3rd readers-writers problem; where in the 1st and 2nd problems are preference-based while third goes for no starvation of readers or writers.

# Problem 

The reader-writer problems deal with synchronysing multiple processes trying to read or write upon a shared data.

## First Readers-Writers Problem

It gives priority to readers.That means writers come to action after all the readers are done.This could lead to starvation of writers.

## Second Readers-Writers Problem

It gives priority to writers.That means no writer is kept waiting longer than absolutely necessary.So readers are kept waiting.This could lead to starvation of readers.

## Third Readers-Writers Problem

It gives **"NO"** priority to any.The requests for same resource will be served in the order they arrive (FIRST COME FIRST SERVE).Here no potential starvation is seen because no thread is given priority and so  work advances smoothly.

### Points to focus 
 * When a writer is waiting for resource no new reader/writer enters critical section
 * When reader exits ; it checks if there is a writer waiting and if it is the last reader before the writer.
 
 # Solution 

 To implement the first come first serve we use queue data structure.  So the semaphore struct has a value and a queue attached to it whose head and tail are poperties of semaphore.I implemented push and pop functions of the semaphore similar to queue data structure. The wait and signal functions are written to manage access of semaphore in `<SemaphoreClass>`.

The queue manages the waiting processes and it gets blocked after pushing one process onto the queue. It is released in FIFO order when semaphor is released from a previously possessing process.

## How it works

Any numbers of readers can read at once. When a writer is there; no other reader or writer should be in critical section.A boolean writerwaiting is declared to know the presence of writer ahead.

When a reader comes he first waits for permission to modify readers_in then increases it, then signals it to wakeup.Then the reader reads and waits for readersout semaphore to amend readers_out and decreases it. After this before exiting; it checks if it is the last reader before a writer with writer_waiting and if it is so, it signals writers to wakeup; else directly wakesup readersout.

When a writer comes; it holdson to a readersin until it exits to ensure no readers comes in while writer is there in critical section. It checks if any readers are present already; If there is no readers it directly writes and signals to wakeup all semaphores.If there are readers; it makes writerwaiting to true to indicate its presence and then signals to wakeup readersout.(Readersin is still intact).Then it waits for writers semaphore and then changes the bool value since it is no more waiting.Then it writes and at the end wakesup readersin to allow further readers/writers.

# References
* [arxiv.org](https://arxiv.org/ftp/arxiv/papers/1309/1309.4507.pdf#:~:text=The%20only%20downside%20it%20has,commonly%20known%20solution%20is%20proposed.&text=This%20solution%20is%20simple%20and%20fast%20enough)
* Operating System Concepts by Abraham Silberschatz, Peter B. Galvin, Greg Gagne
