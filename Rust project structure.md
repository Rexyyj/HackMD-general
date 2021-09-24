# Rust project structure

* Packages: A Cargo feature that lets you build, test, and share crates
* Crates: A tree of modules that produces a library or executable
* Modules and use: Let you control the organization, scope, and privacy of paths
* Paths: A way of naming an item, such as a struct, function, or module

## Packages and Crates
1. A package can contain at most one library crate. It can contain as many binary crates as you’d like, but it must contain at least one crate (either library or binary).
2. Cargo knows that if the package directory contains src/lib.rs, the package contains a library crate with the same name as the package, and src/lib.rs is its crate root.
3. A package can have multiple binary crates by placing files in the src/bin directory: each file will be a separate binary crate.

## Modules
1. A module is defined by starting with a keyword "mod".
2. Inside modules, we can have other modules.
3. Module tree: if module A is contained inside module B, we say that module A is the child of module B and that module B is the parent of module A. Notice that the entire module tree is rooted under the implicit module named crate. eg.
```
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```
4. Modules define Rust’s privacy boundary: the line that encapsulates the implementation details external code isn’t allowed to know about, call, or rely on.
5. The way privacy works in Rust is that all items (functions, methods, structs, enums, modules, and constants) are private by default.
6. Items in a parent module can’t use the private items inside child modules, but items in child modules can use the items in their ancestor modules. 
7. Adding a keyword "pub" in front of a module makes the module public, but it doesn't means making the module's content public. ==> To make the contents also public, also add keyword "pub" in front of them.(it is called exposing the path)
8. If we use pub before a struct definition, we make the struct public, but the struct’s fields will still be private(not readable or mutable). ==> add "pub" to each field to make them public.
9. In contrast, if we make an enum public, all of its variants are then public. 
10. Using a semicolon after a module rather than using a block tells Rust to load the contents of the module from another file with the same name as the module. e.g.
```rust
// semicolon afer mod front_of_house
mod front_of_house;

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```

## Paths
1. Forms
   * An absolute path starts from a crate root by using a crate name or a literal crate.
   * relative path starts from the current module and uses self, super, or an identifier in the current module.
```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
 ```
2. Relative path with "super" keyword.-- "super" keywory is like ".." syntax in a file system. eg.
```rust
fn serve_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::serve_order();
    }

    fn cook_order() {}
}
```
3. Bringing paths into Scope with the "use" keyword. Adding use and a path in a scope is similar to creating a symbolic link in the filesystem. eg.
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use self::front_of_house::hosting;
// or use a new name with
// use self::front_of_house::hosting as RestHosting;
pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```
4. Re-exporing. By using "pub use", external code can call module's public child module.e.g.
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;
//with this. extern code can call eat at restatuant, otherwise not
pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```



## Refs
1. Public crate: https://crates.io/
2. 