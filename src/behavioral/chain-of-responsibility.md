# Chain of Responsibility Pattern

Decouples the sender of a request from its receivers, forming a chain of objects that can handle the request dynamically, giving each receiver the chance to process the request or pass it to the next receiver.

### Example

---

```rust
struct Request {
    path: String,
    method: String,
}

trait Middleware {
    fn handle_request(&self, request: &Request) -> Option<String>;
}

struct AuthenticationMiddleware {
    next: Option<Box<dyn Middleware>>,
}

impl Middleware for AuthenticationMiddleware {
    fn handle_request(&self, request: &Request) -> Option<String> {
        // Perform authentication logic here
        println!("Authentication middleware: Authenticating request...");

        // If authentication succeeds, pass the request to the next middleware
        self.next.as_ref().and_then(|middleware| middleware.handle_request(request))
    }
}

struct LoggingMiddleware {
    next: Option<Box<dyn Middleware>>,
}

impl Middleware for LoggingMiddleware {
    fn handle_request(&self, request: &Request) -> Option<String> {
        // Perform logging logic here
        println!("Logging middleware: Logging request - path: {}, method: {}", request.path, request.method);

        // Pass the request to the next middleware
        self.next.as_ref().and_then(|middleware| middleware.handle_request(request))
    }
}

struct AuthorizationMiddleware {
    next: Option<Box<dyn Middleware>>,
}

impl Middleware for AuthorizationMiddleware {
    fn handle_request(&self, request: &Request) -> Option<String> {
        // Perform authorization logic here
        println!("Authorization middleware: Authorizing request...");

        // If authorization succeeds, pass the request to the next middleware
        self.next.as_ref().and_then(|middleware| middleware.handle_request(request))
    }
}

struct RequestHandler {
    middleware: Option<Box<dyn Middleware>>,
}

impl RequestHandler {
    fn handle_request(&self, request: Request) -> Option<String> {
        self.middleware.as_ref().and_then(|middleware| middleware.handle_request(&request))
    }
}

fn main() {
    // Create the chain of middleware
    let authentication_middleware = AuthenticationMiddleware { next: None };
    let logging_middleware = LoggingMiddleware { next: Some(Box::new(authentication_middleware)) };
    let authorization_middleware = AuthorizationMiddleware { next: Some(Box::new(logging_middleware)) };

    // Create the request handler
    let request_handler = RequestHandler { middleware: Some(Box::new(authorization_middleware)) };

    // Handle a sample request
    let request = Request { path: "/api/users".to_string(), method: "GET".to_string() };
    let response = request_handler.handle_request(request);

    // Process the response, e.g., send it back to the client
    println!("Response: {:?}", response);
}

```
