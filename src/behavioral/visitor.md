# Visitor Pattern

Separates the algorithm from the objects it operates on, allowing new operations to be added to the object structure without modifying the objects themselves.

### Example

---

```rust
// Node trait representing a node in the tree
trait Node {
    fn accept(&self, visitor: &mut dyn Visitor);
}

// Leaf node in the tree
struct LeafNode {
    value: i32,
}

impl Node for LeafNode {
    fn accept(&self, visitor: &mut dyn Visitor) {
        visitor.visit_leaf_node(self);
    }
}

// Composite node in the tree
struct CompositeNode {
    children: Vec<Box<dyn Node>>,
}

impl Node for CompositeNode {
    fn accept(&self, visitor: &mut dyn Visitor) {
        visitor.visit_composite_node(self);
    }
}

// Visitor trait defining the operations to be performed on nodes
trait Visitor {
    fn visit_leaf_node(&mut self, node: &LeafNode);
    fn visit_composite_node(&mut self, node: &CompositeNode);
}

// Concrete visitor implementation
struct SumVisitor {
    sum: i32,
}

impl SumVisitor {
    fn new() -> Self {
        SumVisitor { sum: 0 }
    }
}

impl Visitor for SumVisitor {
    fn visit_leaf_node(&mut self, node: &LeafNode) {
        self.sum += node.value;
    }

    fn visit_composite_node(&mut self, node: &CompositeNode) {
        for child in &node.children {
            child.accept(self);
        }
    }
}

fn main() {
    // Create the tree structure
    let leaf1 = Box::new(LeafNode { value: 5 });
    let leaf2 = Box::new(LeafNode { value: 10 });
    let composite1 = Box::new(CompositeNode {
        children: vec![leaf1, leaf2],
    });

    let leaf3 = Box::new(LeafNode { value: 15 });
    let leaf4 = Box::new(LeafNode { value: 20 });
    let composite2 = Box::new(CompositeNode {
        children: vec![leaf3, leaf4],
    });

    let root = Box::new(CompositeNode {
        children: vec![composite1, composite2],
    });

    // Create the visitor and perform the operations on the tree
    let mut sum_visitor = SumVisitor::new();
    root.accept(&mut sum_visitor);

    println!("Sum of all leaf node values: {}", sum_visitor.sum);
}

```
