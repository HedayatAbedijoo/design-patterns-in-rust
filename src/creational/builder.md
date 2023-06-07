# Builder Pattern

The Builder pattern is a creational design pattern that allows for the construction of complex objects step by step. It provides a clear and readable way to create objects with many optional parameters or complex initialization logic.

### Examples:

---

```rust
#[derive(Debug)]
struct Pizza {
    base: Option<String>,
    sauce: Option<String>,
    toppings: Vec<String>,
    // ... other optional parameters
}

struct PizzaBuilder {
    pizza: Pizza,
}

impl PizzaBuilder {
    fn new() -> Self {
        PizzaBuilder {
            pizza: Pizza {
                base: None,
                sauce: None,
                toppings: Vec::new(),
                // ... initialize other optional parameters
            },
        }
    }

    fn add_base(mut self, base: String) -> Self {
        self.pizza.base = Some(base);
        self
    }

    fn add_sauce(mut self, sauce: String) -> Self {
        self.pizza.sauce = Some(sauce);
        self
    }

    fn add_topping(mut self, topping: String) -> Self {
        self.pizza.toppings.push(topping);
        self
    }

    // ... other setter methods for optional parameters

    fn build(self) -> Option<Pizza> {
        if !self.pizza.base.is_none() && !self.pizza.sauce.is_none() {
            Some(self.pizza)
        } else {
            None
        }
    }

    // OR
    //  fn build(self) -> Pizza {
    //     self.pizza
    // }
}

fn main() {
    let pizza = PizzaBuilder::new()
        .add_base("Thin Crust".to_string())
        .add_sauce("Tomato".to_string())
        .add_topping("Cheese".to_string())
        .add_topping("Mushrooms".to_string())
        .build();

    match pizza {
        Some(pizza) => println!("Successfully built pizza: {:?}", pizza),
        None => println!("Failed to build pizza due to missing parameters"),
    }
}


```
