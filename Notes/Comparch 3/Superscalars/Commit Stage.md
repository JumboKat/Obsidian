### Precise Exceptions
When an instruction fault occurs, the exception handler must see the machine as if everything had run sequentially. This means that:
- Every instruction older than the exception E has finished and its results are visible
- No instruction younger than E has changed any architectural state
- E is handled, and it is either re-ran or skipped, before execution resumes.

![[Pasted image 20261009190859.png|442]]
For an out-of-order processor, instructions younger than E may have executed and computed their result before it. This means that their result must not have been written architecturally, and instead are flushed out. This is handled by using a commit stage with the **reorder buffer**.
### The Reorder Buffer
![[Pasted image 20261009191059.png|566]]
At the end of execution, each instruction must pass through the reorder buffer. Here, instructions are committed in order; this means that their results are only written if the instruction that was fetched before it has committed. Until then, the result is temporarily held, and in the event of an exception, is flushed.

With a unified PRF, the ROB holds no data itself; only the order, status, faulting flag, and the previous physical mapping of the destination (so that the RAT can free register names once an instruction is committed).

So, on a commit:
- The result is made architectural
- The previous physical register is sent back to the free list
- Let a store write to the data cache
- Check for a fault or branch misprediction recorded in the entry
### Handling Exceptions
![[Pasted image 20261009232100.png]]When an exception occurs, we first mark the offending entry and let it reach the head. This allows every older instruction to commit as normal. Then, everything younger is flushed. This means the ROB, issue queues, and the LSQ (load/store queue). Then, the front-end register map is restored (from RRAT or branch checkpoint), PC is set, and the handler is run. Stale physical registers return to the free list.