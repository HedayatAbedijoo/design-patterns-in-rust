# Singleton Pattern

The Singleton pattern is a creational design pattern that ensures the creation of only one instance of a class throughout the lifetime of an application. It provides a global point of access to this instance.

### Examples:

---

```rust
use lazy_static::lazy_static;
use std::sync::{Arc, Mutex};
struct Singleton {
    data: String,
}

impl Singleton {
    fn new() -> Singleton {
        Singleton {
            data: String::from("Singleton Data"),
        }
    }

    fn get_instance() -> Arc<Singleton> {
        lazy_static! {
            static ref INSTANCE: Mutex<Option<Arc<Singleton>>> = Mutex::new(None);
        }

        let mut instance = INSTANCE.lock().unwrap();
        if instance.is_none() {
            *instance = Some(Arc::new(Singleton::new()));
        }
        Arc::clone(instance.as_ref().unwrap())
    }

    fn get_data(&self) -> &str {
        &self.data
    }
}

fn main() {
    let singleton = Singleton::get_instance();
    println!("Singleton Data: {}", singleton.get_data());
}

```

```toml
[dependencies]
lazy_static = "1.4.0"
```
