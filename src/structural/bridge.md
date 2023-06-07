# Bridge Pattern

Separates an abstraction from its implementation, allowing them to vary independently. It helps in decoupling an abstraction from its implementation details, promoting flexibility.

### Example

---

```rust
// Abstraction: Remote Control
trait RemoteControl {
    fn power_on(&self);
    fn power_off(&self);
    fn set_channel(&self, channel: u8);
    fn next_channel(&self);
    fn previous_channel(&self);
}

// Implementor: TV
trait TV {
    fn power_on(&self);
    fn power_off(&self);
    fn set_channel(&self, channel: u8);
}

// Concrete Implementor: Sony TV
struct SonyTV;

impl TV for SonyTV {
    fn power_on(&self) {
        println!("Sony TV: Power ON");
    }

    fn power_off(&self) {
        println!("Sony TV: Power OFF");
    }

    fn set_channel(&self, channel: u8) {
        println!("Sony TV: Set Channel to {}", channel);
    }
}

// Concrete Implementor: LG TV
struct LGTV;

impl TV for LGTV {
    fn power_on(&self) {
        println!("LG TV: Power ON");
    }

    fn power_off(&self) {
        println!("LG TV: Power OFF");
    }

    fn set_channel(&self, channel: u8) {
        println!("LG TV: Set Channel to {}", channel);
    }
}

// Refined Abstraction: Advanced Remote Control
struct AdvancedRemoteControl {
    tv: Box<dyn TV>,
}

impl AdvancedRemoteControl {
    fn new(tv: Box<dyn TV>) -> Self {
        AdvancedRemoteControl { tv }
    }

    fn mute(&self) {
        println!("Advanced Remote Control: Mute");
    }
}

impl RemoteControl for AdvancedRemoteControl {
    fn power_on(&self) {
        self.tv.power_on();
    }

    fn power_off(&self) {
        self.tv.power_off();
    }

    fn set_channel(&self, channel: u8) {
        self.tv.set_channel(channel);
    }

    fn next_channel(&self) {
        // Additional functionality in Advanced Remote Control
        println!("Advanced Remote Control: Next Channel");
        // Delegating to TV
        self.tv.set_channel(1);
    }

    fn previous_channel(&self) {
        // Additional functionality in Advanced Remote Control
        println!("Advanced Remote Control: Previous Channel");
        // Delegating to TV
        self.tv.set_channel(1);
    }
}

// Client code
fn main() {
    // Create instances of the concrete implementors
    let sony_tv = Box::new(SonyTV);
    let lg_tv = Box::new(LGTV);

    // Use the abstraction with different implementors
    let remote1 = AdvancedRemoteControl::new(sony_tv);
    remote1.power_on();
    remote1.set_channel(5);
    remote1.mute();

    let remote2 = AdvancedRemoteControl::new(lg_tv);
    remote2.power_on();
    remote2.next_channel();
    remote2.power_off();
}

```
