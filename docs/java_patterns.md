	1. Singleton Pattern
	A singleton ensures that only one instance of a class exists throughout the application lifecycle. It’s often used for shared resources like configuration, logging, or connection pools.

Thread-Safety
In multi-threaded environments, multiple threads might try to create the singleton simultaneously — leading to multiple instances or race conditions.
To achieve:
- Synchronized blocks (classic but slower)
- Double-checked locking (efficient and safe)
- Static holder idiom (best for lazy + thread-safe)
- Enum singleton (guaranteed by JVM)
