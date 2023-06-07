# Interpreter Pattern

Defines a representation of grammar and an interpreter to evaluate sentences in the language, enabling the interpretation of a language or expression.

### Example

---

```rust
use std::collections::HashMap;

trait Expression {
    fn interpret(&self, context: &Context) -> i32;
}

struct NumberExpression {
    value: i32,
}

impl NumberExpression {
    fn new(value: i32) -> Self {
        NumberExpression { value }
    }
}

impl Expression for NumberExpression {
    fn interpret(&self, _context: &Context) -> i32 {
        self.value
    }
}

struct AddExpression {
    left: Box<dyn Expression>,
    right: Box<dyn Expression>,
}

impl AddExpression {
    fn new(left: Box<dyn Expression>, right: Box<dyn Expression>) -> Self {
        AddExpression { left, right }
    }
}

impl Expression for AddExpression {
    fn interpret(&self, context: &Context) -> i32 {
        self.left.interpret(context) + self.right.interpret(context)
    }
}

struct Context {
    variables: HashMap<String, i32>,
}

impl Context {
    fn new() -> Self {
        Context {
            variables: HashMap::new(),
        }
    }

    fn set_variable(&mut self, name: &str, value: i32) {
        self.variables.insert(name.to_string(), value);
    }

    fn get_variable(&self, name: &str) -> Option<&i32> {
        self.variables.get(name)
    }
}

fn main() {
    let mut context = Context::new();
    context.set_variable("x", 5);
    context.set_variable("y", 3);

    let expression = AddExpression::new(
        Box::new(NumberExpression::new(2)),
        Box::new(AddExpression::new(
            Box::new(NumberExpression::new(3)),
            Box::new(NumberExpression::new(4)),
        )),
    );

    let result = expression.interpret(&context);
    println!("Result: {}", result);
}


```

**Regular Expression**

```rust
trait Regex {
    fn matches(&self, input: &str) -> bool;
}

struct EmailRegex {
    pattern: String,
}

impl EmailRegex {
    fn new(pattern: &str) -> Self {
        EmailRegex {
            pattern: pattern.to_string(),
        }
    }
}

impl Regex for EmailRegex {
    fn matches(&self, input: &str) -> bool {
        // Perform pattern matching logic based on the email regex pattern
        // Here, we'll assume a simplified implementation for demonstration purposes
        input.contains(&self.pattern)
    }
}

fn main() {
    let email_regex = EmailRegex::new(r"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$");

    let email = "test@example.com";
    let is_valid = email_regex.matches(email);

    if is_valid {
        println!("The email is valid!");
    } else {
        println!("Invalid email.");
    }
}

```
