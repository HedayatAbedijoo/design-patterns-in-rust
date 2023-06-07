# State Pattern

Enables an object to alter its behavior when its internal state changes, encapsulating different states as separate classes and allowing for easy state transitions.

### Example

---

```rust
use std::cell::RefCell;
use std::rc::Rc;

// State trait
trait State {
    fn handle(self: Rc<Self>, context: &mut WorkflowContext);
}

// Concrete states
struct DraftState;
struct ReviewState;
struct ApprovedState;
struct PublishedState;

impl State for DraftState {
    fn handle(self: Rc<Self>, context: &mut WorkflowContext) {
        // Perform actions specific to the Draft state
        println!("Workflow is in the Draft state.");
        println!("Performing actions for Draft state...");

        // Transition to the next state
        context.set_state(Rc::new(ReviewState {}));
    }
}

impl State for ReviewState {
    fn handle(self: Rc<Self>, context: &mut WorkflowContext) {
        // Perform actions specific to the Review state
        println!("Workflow is in the Review state.");
        println!("Performing actions for Review state...");

        // Transition to the next state
        context.set_state(Rc::new(ApprovedState {}));
    }
}

impl State for ApprovedState {
    fn handle(self: Rc<Self>, context: &mut WorkflowContext) {
        // Perform actions specific to the Approved state
        println!("Workflow is in the Approved state.");
        println!("Performing actions for Approved state...");

        // Transition to the next state
        context.set_state(Rc::new(PublishedState {}));
    }
}

impl State for PublishedState {
    fn handle(self: Rc<Self>, context: &mut WorkflowContext) {
        // Perform actions specific to the Published state
        println!("Workflow is in the Published state.");
        println!("Performing actions for Published state...");

        // No further state transition from the Published state
        println!("Workflow is in its final state.");
    }
}

// Context that holds the state and manages state transitions
struct WorkflowContext {
    state: Rc<dyn State>,
}

impl WorkflowContext {
    fn new(state: Rc<dyn State>) -> Self {
        WorkflowContext { state }
    }

    fn set_state(&mut self, state: Rc<dyn State>) {
        self.state = state;
    }

    fn perform_workflow(&mut self) {
        // Call the handle method on the current state
        self.state.clone().handle(self);
    }
}

// Client code
fn main() {
    let initial_state = Rc::new(DraftState {});
    let mut workflow = WorkflowContext::new(initial_state);

    // Perform the workflow
    workflow.perform_workflow();

    // The workflow can be triggered again to transition to the next state
    workflow.perform_workflow();

    // Trigger the workflow multiple times to reach the final state
    workflow.perform_workflow();
    workflow.perform_workflow();
}


```
