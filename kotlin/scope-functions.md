# Scope Functions

Five scope functions in Kotlin: `let`, `run`, `with`, `apply`, and `also`. These functions execute a block of code on an object.

Each function accesses the context object in one of two ways: as a *lambda receiver* (`this`) or as a *lambda argument* (`it`).

`this` can be omitted. `it` is an implicit name or can have a custom name.

| Function | Object reference | Return value   | Extension function?                      |
| -------- | ---------------- | -------------- | ---------------------------------------- |
| `let`    | `it`             | lambda result  | yes                                      |
| `run`    | `this`           | lambda result  | yes                                      |
| `run`    | -                | lambda result  | no (called without context object)       |
| `with`   | `this`           | lambda result  | no (takes context object as an argument) |
| `apply`  | `this`           | context object | yes                                      |
| `also`   | `it`             | context object | yes                                      |

- `run`: object configuration and computing the result
- `run` (non-extension): running statements where an expression is required



### let

- To execute a lambda on non-null objects.
- To introduce an expression as a variable in local scope.

```kotlin
number?.let { // if number is not null, go inside the block
	it + 1
}
```

```kotlin
val x = number?.let {
	it + 1
	number // return value
}
```

Null check with `?.let` vs `if (number != null)`:

```kotlin
number?.let {
  // number is not null here
  // number is only read once and stored locally
}

if (number != null) {
	// number here might be null, for e.g. another thread might change it
	// compiler doesn't allow to assume number is not null here
}
```



### run

- Object configuration and computing the result.
- (non-extension) Running a block of statements where an expression is required.

```kotlin
val service = MultiportService("https://example.kotlinlang.org", 80)

val result = service.run {
    port = 8080
    query(prepareRequest() + " to port $port")
}

// the same code written with let() function:
val letResult = service.let {
    it.port = 8080
    it.query(it.prepareRequest() + " to port ${it.port}")
}
```

```kotlin
val hexNumberRegex = run {
    val digits = "0-9"
    val hexDigits = "A-Fa-f"
    val sign = "+-"

    Regex("[$sign]?[$digits$hexDigits]+")
}
```



### with

- Grouping function calls on an object.

```kotlin
val numbers = mutableListOf("one", "two", "three")
with(numbers) {
    println("The list has $size elements.") // this.size
}
```

```kotlin
val numbers = mutableListOf("one", "two", "three")
val firstNumber = with(numbers) {
    "The first element is ${first()}."
}
println(firstNumber)
```



### apply

- Object configuration.

```kotlin
val person = Person().apply {
	name = "Spiderman"	// this. can be omitted
	age = 35						// this. can be omitted
}
```



### also

- Additional effects or actions that don’t alter the object, such as logging or printing debug information.

```kotlin
val person = Person().also {
	it.name = "Spiderman"
	it.age = 35
}
```

```kotlin
val person = Person().also { newPerson ->
	newPerson.name = "Spiderman"
	newPerson.age = 35
}
```

```kotlin
private var i = 0

fun getSquaredI() = (i * i).also {
	i++
}
```





------

https://kotlinlang.org/docs/scope-functions.html

https://typealias.com/guides/understanding-let-also-run-apply/

https://rotadev.com/kotlin-what-is-the-difference-between-apply-and-also-dev/

https://www.journaldev.com/19467/kotlin-let-run-also-apply-with

https://medium.com/mobile-app-development-publication/mastering-kotlin-standard-functions-run-with-let-also-and-apply-9cd334b0ef84

https://blog.mindorks.com/using-scoped-functions-in-kotlin-let-run-with-also-apply

https://www.youtube.com/watch?v=Vy-dS2SVoHk

https://www.youtube.com/watch?v=0sPzDwS55wM&t=535s