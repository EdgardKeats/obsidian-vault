
ReentrantLock 

Since Java 5.

A general purpose mutex (limits the access to a shared resource or thread so only one can access at a time). It's similar to the keyword "synchronized", but with the possibilty of adding conditions over it (like a timeout for example).

Has reentrancy; Has a fair option with new ReentrantLock(true); it's interruptible; try-lock semantics; allows Condition usage

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class TryLockExample {
    private final Lock lock = new ReentrantLock();

    public void performTask() {
        // 1. Attempt to acquire the lock immediately
        if (lock.tryLock()) { 
            try {
                // 2. Manipulate protected state safely
                System.out.println(Thread.currentThread().getName() + " acquired the lock.");
                Thread.sleep(1000); 
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                // 3. Always unlock inside 'finally' if tryLock() returned true
                lock.unlock(); 
                System.out.println(Thread.currentThread().getName() + " released the lock.");
            }
        } else {
            // 4. Perform an alternative action if the lock is busy
            System.out.println(Thread.currentThread().getName() + " could not get lock. Doing fallback work.");
        }
    }
}

```

Timed lock 

```
import java.util.concurrent.TimeUnit;

if (lock.tryLock(5, TimeUnit.SECONDS)) { // Waits up to 5 seconds
    try {
        // Critical section
    } finally {
        lock.unlock();
    }
} else {
    System.out.println("Timed out waiting for the lock.");
}

```

⚠️ Crucial Gotcha: Fair Locks & "Barging"

If you configure a `ReentrantLock(true)` to be **fair** (meaning threads get the lock in the order they queued), **the plain `tryLock()` method breaks this rule**. 

- `tryLock()` uses **barging**: it will immediately snatch the lock if it happens to be free right at that millisecond, even if a long queue of other threads has been waiting.

- If you want to respect fairness with a timeout, use `tryLock(0, TimeUnit.SECONDS)` instead. 




LINKS

https://medium.com/@kaustubh.saha/the-evolution-of-locking-in-java-reentrantlock-vs-reentrantreadwritelock-vs-stampedlock-51794bb12db3

