
- ReentrantLock vs StampedLock
    - [[Locking mechanisms]]
    - [[Volatility]]
    - [[Atomicity]]
    
- Race condition (casos reales y cómo evitarlos)
    
- Thread lifecycle
	- [[Thread lifecycle]]
	- [[Runnable vs Callable]]
    
- Diferencias entre repository, component y service
	-EVERYTHING IS A COMPONENT! Service y Repository son especializaciones
    
- Distributed transactions -> 2pc vs saga
	- [[2PC]]
	- [[[Saga](https://microservices.io/patterns/data/saga.html)]]
	- consistency vs scalabilty
    
- Estrategias de GC en la JVM
	- Young objects die fast
	- https://www.datadoghq.com/blog/understanding-java-gc/
	- Serial GC: Simplest, for low CPU and memory environments.
	- Parallel GC: Uses the CPU cores to run the garbage collection in multiple threads.
	- G1 GC: Most common nowadays, tracks differents memory areas and once it looks like it's going to be full, it makes a pass.
	- 
    
- Cómo encontrar un memory leak **sin reiniciar** la aplicación
	- JMAP
    
- API versioning
    
- Circuit Breaker pattern -> acompañado de retry, timeout
    
- Cómo atacar un problema de **slow SQL query**
    
- Optimistic vs Pessimistic Locking