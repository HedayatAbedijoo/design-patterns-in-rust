# Adaptor Pattern

Converts the interface of a class into another interface that clients expect. It allows incompatible classes to work together by wrapping the adaptee with a compatible interface.

### Example

---

```rust
// Adaptee: Third-party SMS Service
struct ThirdPartySMS {
    api_key: String,
    api_secret: String,
}

impl ThirdPartySMS {
    fn send_sms(&self, recipient: &str, message: &str) {
        println!("Sending SMS to {}: {}", recipient, message);
        // Actual code to send SMS using the third-party SMS service API
    }
}

// Target: SMS Service Interface
trait SMSService {
    fn send_message(&self, recipient: &str, message: &str);
}

// Adapter: Adapts the ThirdPartySMS to the SMSService interface
struct SMSServiceAdapter {
    third_party_sms: ThirdPartySMS,
}

impl SMSService for SMSServiceAdapter {
    fn send_message(&self, recipient: &str, message: &str) {
        self.third_party_sms.send_sms(recipient, message);
    }
}

// Client code
fn main() {
    // Create an instance of the ThirdPartySMS (Adaptee)
    let third_party_sms = ThirdPartySMS {
        api_key: "API_KEY".to_owned(),
        api_secret: "API_SECRET".to_owned(),
    };

    // Create an instance of the SMSServiceAdapter, wrapping the ThirdPartySMS
    let sms_service = SMSServiceAdapter {
        third_party_sms: third_party_sms,
    };

    // Call the send_message method on the SMSService
    sms_service.send_message("+123456789", "Hello, world!");
}

```
