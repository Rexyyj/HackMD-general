# Rust Study

## Common programming concepts
1. By default variables are immutable.
2. Shadowing:declare a new variable with the same name as a previous variable. 
3. Rust is a statically typed language, which means that it must know the types of all variables at compile time. 
4. Number literals can also use _ as a visual separator to make the number easier to read, such as 1_000, which will have the same value as if you had specified 1000


## Ownership
1. In rust,  memory is managed through a system of ownership with a set of rules that the compiler checks at compile time.
2. Stack and Heap
>   All data stored on the stack must have a known, fixed size. Data with an unknown size at compile time or a size that might change must be stored on the heap instead. 

>   Stack==> LIFO,    Heap==> you request a certain amount of space for data

3. Ownership rules
    * Each value in Rust has a variable that’s called its owner.
    * There can only be one owner at a time.
    * When the owner goes out of scope, the value will be dropped.
4. Ownership notes
    * For types implemented **Copy**(eg. let x=2), we can directly use "let y  = x", which copies the value of x to y . For other types like String, we should use .clone(). When a variable of types implemented **Copy** is passed to a function, the value is copied into the function and the ownership remain in the variable. For variables of other types, the ownership will be directly passed to the function and the variable will not be able to use after the funciton.

## Reference and Borrowing
1. Reference allow you to refer to some value without taking ownership of it.
2. You can have only one mutable reference to a particular piece of data in a particular scope.
3. We also cannot have a mutable reference while we have an immutable one.
4. References must always be valid(Not able to create a dangling pointer).

## The Slice type
1. Slice type can be used to slice string or an array.
2. When a variable is sliced, the onwership of original variable is not changed.
3. When the original variable is cleared, the sliced variable is no longer avaliable. --> Error in complile time.

## Struct
1. It’s often useful to create a new instance of a struct that uses most of an old instance’s values but changes some. You’ll do this using struct update syntax. eg.
```rust=
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};

```
2. You can also define structs that look similar to tuples, called tuple structs. (eg. struct Color(i32,i32,i32);  )
3. It’s possible for structs to store references to data owned by something else, but to do so requires the use of lifetimes.
4. Rust does include functionality to print out debugging information, but we have to explicitly opt in to make that functionality available for our struct. To do that, we add the annotation #[derive(Debug)] just before the struct definition.
5. User "impl" keyword to define a metod in a struct. (Only outsid the struct,eg.
```rust=
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

7. Another useful feature of impl blocks is that we’re allowed to define functions within impl blocks that don’t take self as a parameter. These are called associated functions because they’re associated with the struct.They’re still functions, not methods.
8. Each struct is allowed to have multiple impl blocks. 