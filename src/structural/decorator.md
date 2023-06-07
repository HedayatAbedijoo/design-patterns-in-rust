# Decorator Pattern

Dynamically adds new behaviors or responsibilities to an object by wrapping it in a decorator class. It provides a flexible alternative to subclassing for extending functionality.

### Example

---

```rust
// Define a trait representing the Component interface
trait Beverage {
    fn get_description(&self) -> String;
    fn get_cost(&self) -> f64;
}

// Implementation of a concrete Component
struct Coffee;

impl Beverage for Coffee {
    fn get_description(&self) -> String {
        "Coffee".to_string()
    }

    fn get_cost(&self) -> f64 {
        1.0
    }
}

// Implementation of a decorator
struct Milk {
    beverage: Box<dyn Beverage>,
}

impl Milk {
    fn new(beverage: Box<dyn Beverage>) -> Self {
        Milk { beverage }
    }
}

impl Beverage for Milk {
    fn get_description(&self) -> String {
        format!("{} with Milk", self.beverage.get_description())
    }

    fn get_cost(&self) -> f64 {
        self.beverage.get_cost() + 0.5
    }
}

// Implementation of another decorator
struct Sugar {
    beverage: Box<dyn Beverage>,
}

impl Sugar {
    fn new(beverage: Box<dyn Beverage>) -> Self {
        Sugar { beverage }
    }
}

impl Beverage for Sugar {
    fn get_description(&self) -> String {
        format!("{} with Sugar", self.beverage.get_description())
    }

    fn get_cost(&self) -> f64 {
        self.beverage.get_cost() + 0.25
    }
}

fn main() {
    // Create a base component
    let coffee = Box::new(Coffee);

    // Decorate the component with Milk
    let coffee_with_milk = Box::new(Milk::new(coffee));

    // Decorate the component with Sugar
    let coffee_with_milk_and_sugar = Box::new(Sugar::new(coffee_with_milk));

    // Get the final description and cost of the decorated beverage
    println!("Description: {}", coffee_with_milk_and_sugar.get_description());
    println!("Cost: ${}", coffee_with_milk_and_sugar.get_cost());
}

```
