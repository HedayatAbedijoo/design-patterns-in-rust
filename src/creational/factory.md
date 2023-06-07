# Factory Pattern

The Factory pattern is a creational design pattern that provides an interface for creating objects without specifying their concrete classes. It delegates the responsibility of object instantiation to subclasses or specialized factory methods. This pattern promotes loose coupling and encapsulates the object creation logic, allowing flexibility in creating different types of objects based on certain conditions or parameters.

### Examples:

---

```rust
trait Logger {
    fn log(&self, message: &str);
}

struct ConsoleLogger;
struct FileLogger;

impl Logger for ConsoleLogger {
    fn log(&self, message: &str) {
        println!("Logging to console: {}", message);
    }
}

impl Logger for FileLogger {
    fn log(&self, message: &str) {
        // Code for logging to a file
        println!("Logging to file: {}", message);
    }
}

enum LoggerType {
    Console,
    File,
}

struct LoggerFactory;

impl LoggerFactory {
    fn create_logger(&self, logger_type: LoggerType) -> Box<dyn Logger> {
        match logger_type {
            LoggerType::Console => Box::new(ConsoleLogger),
            LoggerType::File => Box::new(FileLogger),
        }
    }
}

struct App {
    logger: Box<dyn Logger>,
}

impl App {
    fn new(logger: Box<dyn Logger>) -> Self {
        App { logger }
    }

    fn run(&self) {
        self.logger.log("Application started");
        // Rest of the application logic
    }
}

fn main() {
    let factory = LoggerFactory;
    let logger_type = LoggerType::Console;

    let logger = factory.create_logger(logger_type);
    let app = App::new(logger);

    app.run();
}

```

### For Testing and Mocking Purpose:

```rust
// The trait representing the collaborator dependency
trait Collaborator {
    fn do_something(&self);
}

// The concrete implementation of the collaborator
struct RealCollaborator;

impl Collaborator for RealCollaborator {
    fn do_something(&self) {
        println!("RealCollaborator: Doing something real...");
        // Actual implementation of the collaborator's behavior
    }
}

// The factory that creates instances of the collaborator
struct CollaboratorFactory;

impl CollaboratorFactory {
    fn create_collaborator(&self) -> Box<dyn Collaborator> {
        // In a real scenario, this method can create and return a real collaborator
        Box::new(RealCollaborator)
    }
}

// The client code that uses the collaborator
struct Client {
    collaborator: Box<dyn Collaborator>,
}

impl Client {
    fn new(collaborator: Box<dyn Collaborator>) -> Self {
        Client { collaborator }
    }

    fn perform_action(&self) {
        // Do something using the collaborator
        self.collaborator.do_something();
    }
}

// The test/mock implementation of the collaborator
struct MockCollaborator;

impl Collaborator for MockCollaborator {
    fn do_something(&self) {
        println!("MockCollaborator: Doing something mock...");
        // Custom implementation for testing purposes
    }
}

// The test code that uses the mocked collaborator
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_client_with_mock_collaborator() {
        let factory = CollaboratorFactory;
        let mock_collaborator = Box::new(MockCollaborator);

        let client = Client::new(mock_collaborator);
        client.perform_action();
        // Perform assertions on the client's behavior with the mock collaborator
    }
}

fn main() {
    let factory = CollaboratorFactory;
    let collaborator = factory.create_collaborator();

    let client = Client::new(collaborator);
    client.perform_action();
}

```
