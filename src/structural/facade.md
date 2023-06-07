# Facade Pattern

Provides a simplified interface to a complex subsystem, making it easier to use and understand. It hides the complexity of the underlying system and offers a unified interface.

### Example

---

```rust
// Subsystem 1: CPU
struct CPU;

impl CPU {
    pub fn power_on(&self) {
        println!("CPU: Powering on...");
    }

    pub fn check(&self) {
        println!("CPU: Checking system...");
    }

    pub fn initialize(&self) {
        println!("CPU: Initializing...");
    }
}

// Subsystem 2: Memory
struct Memory;

impl Memory {
    pub fn load(&self) {
        println!("Memory: Loading data...");
    }
}

// Subsystem 3: Hard Drive
struct HardDrive;

impl HardDrive {
    pub fn read(&self) {
        println!("Hard Drive: Reading data...");
    }
}

// Facade: Computer
struct Computer {
    cpu: CPU,
    memory: Memory,
    hard_drive: HardDrive,
}

impl Computer {
    pub fn new() -> Self {
        Computer {
            cpu: CPU {},
            memory: Memory {},
            hard_drive: HardDrive {},
        }
    }

    pub fn start(&self) {
        println!("Computer: Starting up...");
        self.cpu.power_on();
        self.cpu.check();
        self.cpu.initialize();
        self.memory.load();
        self.hard_drive.read();
        println!("Computer: Startup complete!");
    }
}

// Client code
fn main() {
    let computer = Computer::new();
    computer.start();
}
```
