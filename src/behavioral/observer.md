# Observer Pattern

Allows objects to subscribe and receive updates from a subject when its state changes, enabling loose coupling between the subject and observers

### Example

---

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

// Subject or Publisher
struct Marketplace {
    subscribers: Vec<Weak<Customer>>,
}

impl Marketplace {
    fn new() -> Self {
        Marketplace {
            subscribers: Vec::new(),
        }
    }

    fn subscribe(&mut self, customer: Rc<Customer>) {
        self.subscribers.push(Rc::downgrade(&customer));
    }

    fn notify_subscribers(&self, product: &str) {
        for subscriber in &self.subscribers {
            if let Some(customer) = subscriber.upgrade() {
                customer.notify(product);
            }
        }
    }
}

// Observer or Subscriber
struct Customer {
    name: String,
}

impl Customer {
    fn new(name: &str) -> Self {
        Customer {
            name: name.to_string(),
        }
    }

    fn notify(&self, product: &str) {
        println!("Hey {}, the product {} is now available!", self.name, product);
    }
}

fn main() {
    let mut marketplace = Marketplace::new();

    let customer1 = Rc::new(Customer::new("John"));
    let customer2 = Rc::new(Customer::new("Alice"));

    marketplace.subscribe(customer1.clone());
    marketplace.subscribe(customer2.clone());

    marketplace.notify_subscribers("Phone");

    // Output:
    // Hey John, the product Phone is now available!
    // Hey Alice, the product Phone is now available!
}

```
