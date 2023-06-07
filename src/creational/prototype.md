# Prototype Pattern

The Prototype pattern is a creational design pattern that enables the creation of new objects by cloning existing ones, without coupling the code to specific classes. It involves creating a prototypical object and then creating new objects by copying the prototype. This pattern is particularly useful when object creation is complex or when there is a need to create multiple instances with similar initial state.

### Examples:

---

```rust
trait Prototype {
    fn clone(&self) -> Box<dyn Prototype>;
    fn draw(&self);
}

#[derive(Clone)]
struct Circle {
    radius: f64,
}

impl Prototype for Circle {
    fn clone(&self) -> Box<dyn Prototype> {
        Box::new(self.clone())
    }

    fn draw(&self) {
        println!("Drawing a circle with radius {}", self.radius);
    }
}

#[derive(Clone)]
struct Rectangle {
    width: f64,
    height: f64,
}

impl Prototype for Rectangle {
    fn clone(&self) -> Box<dyn Prototype> {
        Box::new(self.clone())
    }

    fn draw(&self) {
        println!("Drawing a rectangle with width {} and height {}", self.width, self.height);
    }
}

fn main() {
    let circle_prototype: Box<dyn Prototype> = Box::new(Circle { radius: 5.0 });
    let rectangle_prototype: Box<dyn Prototype> = Box::new(Rectangle { width: 10.0, height: 8.0 });

    let shape1 = circle_prototype.clone();
    shape1.draw();

    let shape2 = rectangle_prototype.clone();
    shape2.draw();
}

```
