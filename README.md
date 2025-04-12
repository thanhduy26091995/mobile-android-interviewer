# mobile-interviewer

### 1. Inline Function

The `inline` keyword is used to optimize **higher-order functions**. When a function is marked as `inline`, the compiler copies the function body along with the lambda directly into the call site.

#### ✅ Benefits

- Avoids object creation for lambdas
- Improves performance (especially for small functions)

### 2. Reified keyword

The `reified` keyword is used in combination with `inline` **funtions** to allow access generic type at a runtime

#### ✅ When to use

- When you want to use a generic type with `::class`, `is` or reflection
- When building DSLs, serializers or type safe builders

  ```kotlin
  inline fun <reified T> Any?.isOfType(): Boolean {
    return this is T
  }
  val value: Any = "Hello"
  println(value.isOfType<String>()) // true
  println(value.isOfType<Int>())    // false
  ```

### 3. Suspending and Blocking with Kotlin Coroutines

#### 3.1 Suspending: A suspending function **_pauses excution_** without blocking the thread

- Does not blocking the thread
- Lightweight and scalable
- Can only be use inside the suspend function or from coroutines

#### 4.2 Blocking: A blocking call hold onto the thread and prevents other works todo until it finished

- Block the thread
- Expsensive
- Normally **_runBlocking_** only work on the unit testing to hold until the code run

### 4. Launch and Async in Kotlin Coroutines

#### 4.1 Launch

- Returns: **_Job_** with no result
- Use when you want to do something with Coroutines but don't care a result
- Just **_Fire and Forget_**

  ```kotlin
  GlobalScope.launch {
      println("This runs in a coroutine")
  }
  ```

#### 4.2 Async

- Returns: **_Deferred<T>_** (A future/promise to wait a result)
- Use when you want to return a result from a coroutine
- **_async_** meaning **_I want to have a result_**

  ```kotlin
    val deferred: Deferred<Int> = GlobalScope.async {
    delay(1000)
    42
  }

  runBlocking {
      println("Result is ${deferred.await()}") // Result is 42
  }
  ```

### 5. What's Multidex in Android?

- Multidex allow use more than 1 Dex file
- By default, Android compiled code into 1 .dex file, that only have limit is 65,536 limit of functions (64k limit). That's why when app and libraries exceed that, it wil throw an error
- For modern Android we no need to care about it, app now can have more than .dex file

### 6. Is it possible to force the Garbage Collection in Android?

**_Yes, techically we can do it, but it's not recommended becauses_**

1. It can hurt peformance if done carelessly
2. The systems already handle it automatically and efficiently
3. Forcing it might interupt normal execution and make UI lag
4. It does not guarentee that the memory is reclaimed immediately

### 7. What is a JvmStatic Annotation in Kotlin?

- This will tell compiler generate this method as a static method in the Java bytecode
- Example without **_@JvmStatic_**

  ```
  class Utils {
      companion object {
          fun sayHello() {
              println("Hello from Kotlin")
          }
      }
  }

  Utils.Companion.sayHello(); // Notice the extra "Companion"
  ```

- Example with **_JvmStatic_**

  ```
  class Utils {
    companion object {
        @JvmStatic
        fun sayHello() {
            println("Hello from Kotlin")
        }
    }
  }

  Utils.sayHello(); // Cleaner and more natural for Java devs

  ```

- Where can we use **_@JvmStatic_**
  1. Inside a **_companion object_**
  2. Inside an object declaration (singleton)

### 8. JvmField Annotation in Kotlin

- Expose this property as a public fied in the generated Java bytecode - no getter/setter

1. Without **_@JvmField_**

   ```
   class MyClass {
     val name = "Kotlin"
   }

   MyClass obj = new MyClass();
   String n = obj.getName();  // Java must use getter

   ```

2. With **_JvmField_**

   ```
    class MyClass {
      @JvmField
      val name = "Kotlin"
    }

    MyClass obj = new MyClass();
    String n = obj.name;  // Direct field access! 🎯
   ```

### 9. Why is it recommended to use only the default constructor to create a Fragment?
- Because the Android framework may recreate your Fragmen during
  1. Configuration changes (Screen rotating)
  2. Process death and restoration
- When this happens, Android only use default constructor, so it does not know how to recreate your Fragment with custom constructor
- How to avoid that
  1. Use a ***companion object*** and pass data via ***Bundle***
