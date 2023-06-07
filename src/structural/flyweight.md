# Flyweight Pattern

Shares common state between multiple objects to reduce memory usage. It allows for efficient representation of large numbers of fine-grained objects.

### Example

---

```rust
// Flyweight: UI Component
#[derive(Debug, Clone)]
struct UIComponent {
    // Shared data
    component_type: String,
    // ... other shared properties

    // Unique data
    content: String,
    // ... other unique properties
}

impl UIComponent {
    fn new(component_type: String, content: String) -> Self {
        UIComponent {
            component_type,
            content,
            // initialize other properties
        }
    }

    fn render(&self) {
        println!(
            "Rendering {} component with content: {}",
            self.component_type, self.content
        );
        // Render the component
    }
}

// Flyweight Factory: UI Component Factory
struct UIComponentFactory {
    components: std::collections::HashMap<String, Box<UIComponent>>,
}

impl UIComponentFactory {
    fn get_component(&mut self, component_type: String, content: String) -> Box<UIComponent> {
        // Check if the component already exists in the factory
        if let Some(component) = self.components.get(&component_type) {
            return component.clone();
        }

        // If not found, create a new component and add it to the factory
        let component = Box::new(UIComponent::new(component_type.clone(), content));
        self.components.insert(component_type, component.clone());
        component
    }
}

// Client code
fn main() {
    let mut component_factory = UIComponentFactory {
        components: std::collections::HashMap::new(),
    };

    // Render UI components
    let button1 = component_factory.get_component("Button".to_string(), "Click me!".to_string());
    let button2 = component_factory.get_component("Button".to_string(), "Submit".to_string());
    let input1 =
        component_factory.get_component("Input".to_string(), "Enter your name".to_string());
    let input2 =
        component_factory.get_component("Input".to_string(), "Enter your email".to_string());

    button1.render();
    button2.render();
    input1.render();
    input2.render();

    // Check if the components are the same objects
    println!(
        "Are button1 and button2 the same object? {}",
        std::ptr::eq(&*button1, &*button2)
    );
    println!(
        "Are input1 and input2 the same object? {}",
        std::ptr::eq(&*input1, &*input2)
    );
}


```
