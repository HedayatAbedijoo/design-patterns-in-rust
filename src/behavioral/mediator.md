# Mediator Pattern

Defines an object that encapsulates how a set of objects interact, promoting loose coupling between objects by centralizing their communication through the mediator.

### Examples

---

```rust
use std::cell::RefCell;
use std::collections::HashMap;
use std::rc::Rc;

// Mediator: ChatRoom
// The ChatRoom acts as the central mediator that facilitates communication between users.

struct ChatRoom {
    users: HashMap<String, Rc<RefCell<User>>>, // Stores the users in the chat room
    public_messages: Vec<(String, String)>,    // Stores public messages
}

impl ChatRoom {
    fn new() -> Self {
        ChatRoom {
            users: HashMap::new(),
            public_messages: Vec::new(),
        }
    }

    // Mediator: add_user
    // Adds a user to the chat room and registers them in the users' HashMap.

    fn add_user(&mut self, user: Rc<RefCell<User>>) {
        let username = user.borrow().username.clone();
        self.users.insert(username, user);
    }

    // Mediator: send_message
    // Sends a message from a sender to a recipient in the chat room.
    // If the recipient is "public", the message is stored as a public message.
    // Otherwise, the message is sent to the recipient's User object.

    fn send_message(&mut self, sender: &str, recipient: &str, message: &str) {
        if recipient == "public" {
            self.public_messages
                .push((sender.to_string(), message.to_string()));
        } else if let Some(user) = self.users.get(recipient) {
            user.borrow().receive_message(sender, message);
        }
    }

    // Mediator: broadcast_message
    // Sends a message from a sender to all users in the chat room.

    fn broadcast_message(&self, sender: &str, message: &str) {
        for user in self.users.values() {
            user.borrow().receive_message(sender, message);
        }
    }
}

// Colleague: User
// The User represents a participant in the chat room who can send and receive messages.

struct User {
    username: String,
    chat_room: Rc<RefCell<ChatRoom>>, // Reference to the ChatRoom mediator
}

impl User {
    fn new(username: String, chat_room: Rc<RefCell<ChatRoom>>) -> Self {
        User {
            username,
            chat_room,
        }
    }

    // Colleague: send_message
    // Sends a message from the user to a recipient using the ChatRoom mediator.

    fn send_message(&mut self, recipient: &str, message: &str) {
        self.chat_room
            .borrow_mut()
            .send_message(&self.username, recipient, message);
    }

    // Colleague: receive_message
    // Receives a message from a sender and displays it on the user's console.

    fn receive_message(&self, sender: &str, message: &str) {
        println!(
            "{} received a message from {}: {}",
            self.username, sender, message
        );
    }
}

fn main() {
    let chat_room = Rc::new(RefCell::new(ChatRoom::new()));

    let user1 = Rc::new(RefCell::new(User::new(
        "John".to_string(),
        Rc::clone(&chat_room),
    )));
    let user2 = Rc::new(RefCell::new(User::new(
        "Emily".to_string(),
        Rc::clone(&chat_room),
    )));
    let user3 = Rc::new(RefCell::new(User::new(
        "Michael".to_string(),
        Rc::clone(&chat_room),
    )));

    chat_room.borrow_mut().add_user(Rc::clone(&user1));
    chat_room.borrow_mut().add_user(Rc::clone(&user2));
    chat_room.borrow_mut().add_user(Rc::clone(&user3));

    user1
        .borrow_mut()
        .send_message("Emily", "Hi Emily! How are you?");
    user2
        .borrow_mut()
        .send_message("Michael", "Hey Michael, did you see John's message?");
    user3
        .borrow_mut()
        .send_message("John", "Yes, I did. Let's continue the conversation.");
    user1
        .borrow_mut()
        .send_message("public", "This is a public message.");
    chat_room
        .borrow()
        .broadcast_message("Admin", "Attention: New event coming up!");

    // Output:
    // Emily received a message from John: Hi Emily! How are you?
    // Michael received a message from Emily: Hey Michael, did you see John's message?
    // John received a message from Michael: Yes, I did. Let's continue the conversation.
    // Emily received a message from Admin: Attention: New event coming up!
    // Michael received a message from Admin: Attention: New event coming up!
    // John received a message from Admin: Attention: New event coming up!
}

```
