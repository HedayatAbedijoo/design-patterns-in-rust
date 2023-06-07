# Composite Pattern

Composes objects into tree structures to represent part-whole hierarchies. It treats individual objects and compositions of objects uniformly, simplifying the interaction between them.

### Example

---

```rust
// Define a trait for the nodes in the tree
trait Node {
    fn get_name(&self) -> &str;
    fn add_child(&mut self, child: Box<dyn Node>);
    fn print(&self, depth: usize);
}

// Implementation of a leaf node representing a file
struct File {
    name: String,
}

impl File {
    // Create a new File node with the given name
    fn new(name: String) -> Self {
        File { name }
    }
}

impl Node for File {
    // Get the name of the file
    fn get_name(&self) -> &str {
        &self.name
    }

    // Since a file cannot have children, this method does nothing
    fn add_child(&mut self, _child: Box<dyn Node>) {
        // Files cannot have children, so this method is empty
    }

    // Print the name of the file
    fn print(&self, depth: usize) {
        // Indent the output based on the depth in the tree structure
        println!("{}{}", "-".repeat(depth), self.name);
    }
}

// Implementation of a composite node representing a directory
struct Directory {
    name: String,
    children: Vec<Box<dyn Node>>,
}

impl Directory {
    // Create a new Directory node with the given name
    fn new(name: String) -> Self {
        Directory {
            name,
            children: Vec::new(),
        }
    }
}

impl Node for Directory {
    // Get the name of the directory
    fn get_name(&self) -> &str {
        &self.name
    }

    // Add a child node to the directory
    fn add_child(&mut self, child: Box<dyn Node>) {
        self.children.push(child);
    }

    // Print the name of the directory and recursively print its children
    fn print(&self, depth: usize) {
        // Indent the output based on the depth in the tree structure
        println!("{}{}", "-".repeat(depth), self.name);

        // Recursively print the children nodes
        for child in &self.children {
            child.print(depth + 1);
        }
    }
}

fn main() {
    // Create the tree structure
    let mut root = Directory::new("Root".to_string());
    let mut subdirectory1 = Directory::new("Subdirectory 1".to_string());
    let mut subdirectory2 = Directory::new("Subdirectory 2".to_string());
    let file1 = Box::new(File::new("File 1".to_string()));
    let file2 = Box::new(File::new("File 2".to_string()));
    let file3 = Box::new(File::new("File 3".to_string()));

    // Add files and subdirectories to the parent directories
    subdirectory1.add_child(file1);
    subdirectory1.add_child(file2);
    subdirectory2.add_child(file3);
    root.add_child(Box::new(subdirectory1));
    root.add_child(Box::new(subdirectory2));

    // Print the entire tree structure
    root.print(0);
}

```
