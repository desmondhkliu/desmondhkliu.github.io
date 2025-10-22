---
layout: page
title: Java Patterns
---

## 1. Singleton Pattern
A singleton ensures that **only one instance** of a class exists throughout the application lifecycle. 

Singleton is often used for shared resources like configuration, logging, or connection pools.


### Key Concerns About Singelton 

#### a. Thread Safety

In multithreaded environment, multiple threads might call **getInstance()** simultaneously when instance is still null — leading to multiple instances or race conditions.


**Example of Failure**

Thread A checks instance == null → true

Thread B checks instance == null → true

Both create new Singleton()

Result: Two instances → breaks Singleton guarantee


**How to Make Singleton Thread-Safe**
- Synchronized blocks
- Double-checked locking
- Static holder idiom
- Enum singleton

Example 1: Synchronized Method (Simplest but slower)

    public class Singleton {
        private static Singleton instance;

        private Singleton() {}

        public static synchronized Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
    }


Example 2: Double-Checked Locking (Efficient and safe)

    public class Singleton {
        private static volatile Singleton instance; // 'volatile' is crucial

        private Singleton() {}

        public static Singleton getInstance() {
            if (instance == null) { // First check (no locking)
                synchronized (Singleton.class) {
                    if (instance == null) { // Second check (with lock)
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }

With **volatile** - avoid JVM reordering of instructions and a partially constructed object could be visible to other threads.


Example 3: Static Holder Idiom (Thread-safe + Lazy)
    
    public class ConfigManager {
        private ConfigManager() {}

        private static class Holder {
            private static final ConfigManager INSTANCE = new ConfigManager();
        }

        public static ConfigManager getInstance() {
            return Holder.INSTANCE;
        }
    }


Example 4: Enum Singleton

    public enum ConfigManager {
        INSTANCE;  // <-- the single instance

        // Constructor (called once, automatically thread-safe)
        ConfigManager() {
            // Do something
        }
    }

    public class Main {
        public static void main(String[] args) {
            ConfigManager config = ConfigManager.INSTANCE;  // Access Singleton
        }
    }


#### b. Testability

Classic singletons use private constructors and static access, which makes mocking or replacing them in tests difficult.

**How to Make It Testable**
- Use dependency injection to pass the singleton
- Allow resetting or overriding the instance in test environments
- Use interfaces and factory methods to abstract the singleton

Example: Testable Singleton via Interface

    public interface Logger {
        void log(String message);
    }

    public class FileLogger implements Logger {
        public void log(String message) { 
            /* write to file */ 
        }
    }

    public class LoggerFactory {
        private static Logger instance = new FileLogger();

        public static Logger getLogger() {
            return instance;
        }

        // For testing
        public static void setLogger(Logger mockLogger) {
            instance = mockLogger;
        }
    }

Now in tests:

    LoggerFactory.setLogger(new InMemoryLogger());




