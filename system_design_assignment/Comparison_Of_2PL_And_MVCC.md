# MVCC vs. 2-Phase Locking (2PL)

Multi-Version Concurrency Control (MVCC) and Two-Phase Locking (2PL) are primary strategies used in Database Management Systems to handle concurrent transactions.

### Core Differences

* **Mechanism:**
  * **2PL:** Relies on active locking (Shared/Exclusive). Transactions acquire locks during a growing phase and release them during a shrinking phase.
  * **MVCC:** Maintains multiple timestamped versions of a data item. Transactions read the historical version corresponding to their specific snapshot.
* **Blocking Behavior:**
  * **2PL:** Readers block writers, and writers block readers. This guarantees strict isolation but restricts overall system throughput.
  * **MVCC:** Readers do not block writers, and writers do not block readers. This significantly improves concurrency and responsiveness.
* **Deadlocks:**
  * **2PL:** Prone to deadlocks because transactions frequently wait for locked resources. Requires continuous deadlock detection (Wait-For Graphs).
  * **MVCC:** Read-only transactions never deadlock. Write-write conflicts are usually resolved by aborting the newer transaction, drastically reducing deadlock frequency.
* **Performance Overhead:**
  * **2PL:** Overhead stems from lock management, specifically maintaining lock tables and resolving contention.
  * **MVCC:** Overhead stems from storage inflation and the CPU cost of "garbage collection" (e.g., PostgreSQL's VACUUM) to purge obsolete data versions.

**Summary:** 2PL enforces strict serializability natively at the cost of concurrency. MVCC optimizes for high read-write workloads by trading storage space for performance. 