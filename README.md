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

### 3. Suspending and Blocking with Kotlin Coroutines
#### 3.1 Suspending: A suspending function ***pauses excution*** without blocking the thread
- Does not blocking the thread
- Lightweight and scalable
- Can only be use inside the suspend function or from coroutines

#### 4.2 Blocking: A blocking call hold onto the thread and prevents other works todo until it finished
- Block the thread
- Expsensive
- Normally ***runBlocking*** only work on the unit testing to hold until the code run

### 4. Launch and Async in Kotlin Coroutines
#### 4.1 Launch
- Returns: ***Job*** with no result
- Use when you want to do something with Coroutines but don't care a result
- Just ***Fire and Forget***

  ```kotlin
  GlobalScope.launch {
      println("This runs in a coroutine")
  }

#### 4.2 Async
- Returns: ***Deferred<T>*** (A future/promise to wait a result)
- Use when you want to return a result from a coroutine
- ***async*** meaning ***I want to have a result***

  ```kotlin
    val deferred: Deferred<Int> = GlobalScope.async {
    delay(1000)
    42
  }

  runBlocking {
      println("Result is ${deferred.await()}") // Result is 42
  }

### 5. What's Multidex in Android?
- Multidex allow use more than 1 Dex file
- By default, Android compiled code into 1 .dex file, that only have limit is 65,536 limit of functions (64k limit). That's why when app and libraries exceed that, it wil throw an error
- For modern Android we no need to care about it, app now can have more than .dex file 
