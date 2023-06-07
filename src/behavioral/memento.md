# Memento Pattern

The Memento pattern allows an object to capture its internal state and store it externally, without violating encapsulation. It provides the ability to restore the object's state to a previous state.

### Example

---

```rust
struct TextEditor {
    text: String,
    cursor_position: usize,
    undo_stack: Vec<TextEditorMemento>,
}

impl TextEditor {
    fn new() -> Self {
        TextEditor {
            text: String::new(),
            cursor_position: 0,
            undo_stack: Vec::new(),
        }
    }

    fn insert_text(&mut self, text: &str) {
        self.text.insert_str(self.cursor_position, text);
        self.cursor_position += text.len();
    }

    fn move_cursor_left(&mut self) {
        if self.cursor_position > 0 {
            self.cursor_position -= 1;
        }
    }

    fn move_cursor_right(&mut self) {
        if self.cursor_position < self.text.len() {
            self.cursor_position += 1;
        }
    }

    fn undo(&mut self) {
        if let Some(memento) = self.undo_stack.pop() {
            self.text = memento.text;
            self.cursor_position = memento.cursor_position;
        }
    }

    fn save_undo_state(&mut self) {
        let memento = TextEditorMemento {
            text: self.text.clone(),
            cursor_position: self.cursor_position,
        };
        self.undo_stack.push(memento);
    }

    fn display(&self) {
        println!("Text: {}", self.text);
        println!(
            "Cursor Position: {}",
            " ".repeat(self.cursor_position) + "^"
        );
    }
}

struct TextEditorMemento {
    text: String,
    cursor_position: usize,
}

fn main() {
    let mut editor = TextEditor::new();

    editor.insert_text("Hello");
    editor.display(); // Text: Hello, Cursor Position: ^

    editor.save_undo_state();

    editor.insert_text(" World");
    editor.display(); // Text: Hello World, Cursor Position: ^

    editor.save_undo_state();

    editor.move_cursor_left();
    editor.display(); // Text: Hello World, Cursor Position: ^

    editor.undo();
    editor.display(); // Text: Hello World, Cursor Position: ^

    editor.undo();
    editor.display(); // Text: Hello, Cursor Position: ^
}

```
