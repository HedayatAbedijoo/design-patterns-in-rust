# Command Pattern

Encapsulates a request as an object, allowing parameterization of clients with different requests, queuing or logging requests, and supporting undoable operations.

### Example

---

```rust
// Device trait
trait Device {
    fn on(&self);
    fn off(&self);
}

// Light device
struct Light {
    name: String,
}

impl Device for Light {
    fn on(&self) {
        println!("{} light turned on", self.name);
        // Logic to turn on the light
    }

    fn off(&self) {
        println!("{} light turned off", self.name);
        // Logic to turn off the light
    }
}

// Thermostat device
struct Thermostat {
    name: String,
}

impl Device for Thermostat {
    fn on(&self) {
        println!("{} thermostat turned on", self.name);
        // Logic to turn on the thermostat
    }

    fn off(&self) {
        println!("{} thermostat turned off", self.name);
        // Logic to turn off the thermostat
    }
}

// Command trait
trait Command {
    fn execute(&self);
    fn undo(&self);
}

// Concrete command for turning on the device
struct TurnOnCommand<T: Device> {
    device: T,
}

impl<T: Device> Command for TurnOnCommand<T> {
    fn execute(&self) {
        self.device.on();
    }

    fn undo(&self) {
        self.device.off();
    }
}

// Concrete command for turning off the device
struct TurnOffCommand<T: Device> {
    device: T,
}

impl<T: Device> Command for TurnOffCommand<T> {
    fn execute(&self) {
        self.device.off();
    }

    fn undo(&self) {
        self.device.on();
    }
}

// Home automation system
struct HomeAutomation {
    commands: Vec<Box<dyn Command>>,
}

impl HomeAutomation {
    fn new() -> Self {
        HomeAutomation {
            commands: Vec::new(),
        }
    }

    fn add_command(&mut self, command: Box<dyn Command>) {
        self.commands.push(command);
    }

    fn execute_commands(&self) {
        for command in &self.commands {
            command.execute();
        }
    }

    fn undo_commands(&self) {
        for command in self.commands.iter().rev() {
            command.undo();
        }
    }
}

fn main() {
    let living_room_light = Light {
        name: "Living Room Light".to_string(),
    };
    let bedroom_light = Light {
        name: "Bedroom Light".to_string(),
    };
    let thermostat = Thermostat {
        name: "Thermostat".to_string(),
    };

    let turn_on_living_room_light = Box::new(TurnOnCommand {
        device: living_room_light,
    });
    let turn_off_bedroom_light = Box::new(TurnOffCommand {
        device: bedroom_light,
    });
    let turn_on_thermostat = Box::new(TurnOnCommand { device: thermostat });

    let mut home_automation = HomeAutomation::new();
    home_automation.add_command(turn_on_living_room_light);
    home_automation.add_command(turn_off_bedroom_light);
    home_automation.add_command(turn_on_thermostat);

    home_automation.execute_commands();
    println!("Undoing last command:");
    home_automation.undo_commands();
}

```
