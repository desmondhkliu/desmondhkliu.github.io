---
layout: page
title: Java Patterns
---
Some core design patterns:
    
|#  |**Pattern**      |**Category**  |**Intent**                       |**Example**      |  
|1  |[Singleton](#1-singleton-pattern)|Creational    |One global instance              |Logging, Config  |  
|2	|[Factory](#2-factory-pattern)|Creational	 |Centralized object creation      |DAO factory      |  
|3	|[Builder](#3-builder-pattern)|Creational	 |Build complex immutable objects  |DTOs, Config     |  
|4	|[Strategy](#4-strategy-pattern)|Behavioral	 |Switch algorithms dynamically	   |Payment methods  |  
|5	|[Observer](#5-observer-pattern)|Behavioral	 |Notify subscribers of changes	   |Event systems    |  
|6	|[Decorator](#6-decorator-pattern)|Structural	 |Add features dynamically	       |Streams, UI      |  
|7	|[Adapter](#7-adapter-pattern)|Structural	 |Bridge incompatible interfaces   |Legacy system integration|  
|8	|[Proxy](#8-proxy-pattern)|Structural	 |Control access, lazy loading	   |AOP, Hibernate   |  
|9	|[Template Method](#9-template-method-pattern)|Behavioral	 |Skeleton algorithm with hooks	   |Parsing, Workflow engine|  
  
<br/>  
## 1. Singleton Pattern
A singleton ensures that **only one instance** of a class exists throughout the application lifecycle.   
<br/>
#### Where to Use
Shared resources (configuration, logging, thread pools, cache)  
<br/>
#### Key Concerns  
**a. Thread Safety**  
In multithreaded environment, multiple threads might call **getInstance()** simultaneously when instance is still null — leading to multiple instances or race conditions.  
<br/>
**- Example of Failure**  
Thread A checks instance == null -> true  
Thread B checks instance == null -> true  
Both create new Singleton()  
Result: Two instances → breaks Singleton guarantee
<br/>  
**- How to Make Singleton Thread-Safe**  
  - Synchronized blocks  
  - Double-checked locking  
  - Static holder idiom  
  - Enum singleton  
<br/>

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

<br/>
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
<br/>  
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

<br/>
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

<br/>  
**b. Testability**  
Classic singletons use private constructors and static access, which makes mocking or replacing them in tests difficult.  
<br/>
**How to Make It Testable**  
- Use dependency injection to pass the singleton  
- Allow resetting or overriding the instance in test environments  
- Use interfaces and factory methods to abstract the singleton  
<br/>  

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
  
Test:  

    LoggerFactory.setLogger(new FileLogger());
  
<br/>  
#### Key Benefit  
- Controlled access to one instance  
- Save memory  
- Thread-safe with simple static instance  
<br/><br/>  

## 2. Factory Pattern
Define an interface or method for creating objects but let subclasses or logic decide which class to instantiate.  
<br/>  
#### Where to Use  
When object creation logic is complex or based on runtime conditions.  
<br/>  
#### Key Concerns  
<br/>  
#### Key Benefit  
- Encapsulate object creation logic  
- Promote loose coupling  
<br/>  

## 3. Builder Pattern
Separate the construction of a complex object from its representation so that the same construction process can create different representations.  
<br/>  
#### Where to Use  
When creating objects with many optional parameters.  
<br/>  
#### Key Concerns  
<br/>  
#### Key Benefit  
- Readable object creation  
- Handle immutability easily  
- Avoid telescoping constructors  
<br/> 

## 4. Strategy Pattern
Define a family of algorithms, encapsulate each one, and make them interchangeable at runtime.  
<br/>
#### Where to Use
When you have multiple ways to perform an operation and want to switch behavior dynamically.  
<br/>
#### Key Concerns  
<br/>
#### Key Benefit  
- Replace if/else chains  
- Easy to extend new strategies  
<br/>

## 5. Observer Pattern
Define a one-to-many dependency between objects so when one object changes state, all dependents are notified automatically.  
<br/>
#### Where to Use  
Event-driven systems (e.g. GUI listeners, message systems)  
<br/>
#### Key Concerns  
<br/>  
#### Key Benefit  
- Event-driven  
- Loose coupling between subjects and observers  
<br/>

## 6. Decorator Pattern
Attach additional responsibilities to an object dynamically without altering its structure.  
<br/>  
#### Where to Use  
Add behavior to objects at runtime instead of inheritance.  
<br/>  
#### Key Concerns  
<br/>  
#### Key Benefit  
- Add features without subclass explosion  
- Flexible runtime composition  
<br/>  

## 7. Adapter Pattern
Convert the interface of a class into another interface that clients expect — so that incompatible classes can work together.  
<br/>
#### Where to Use  
When integrating legacy code or third-party APIs that don’t match your expected interface.  
<br/>  
#### Key Concerns  
<br/>  
#### Key Benefit  
- Integrate incompatible interfaces without changing existing code  
- Increase code reusability  
<br/>  

## 8. Proxy Pattern
Provide a surrogate or placeholder for another object to control access to it (e.g. lazy initialization, security, remote access, caching).  
<br/>
#### Where to Use  
When you want to control access, delay creation, or add logging around another object transparently.  
<br/>  
#### Key Concerns  
<br/>  
#### Key Benefit  
- Lazy loading save memory  
- Add access control, logging, or caching without changing original logic  
<br/>  

## 9. Template Method Pattern
Define the skeleton of an algorithm in a base class and let subclasses fill in specific steps.  
<br/>
#### Where to Use  
When multiple algorithms share the same general steps but differ in specific details.  
<br/>  
#### Key Concerns  
<br/>  
#### Key Benefit  
- Promote code reuse for common workflow logic  
- Enforce consistent process steps while allowing flexibility  