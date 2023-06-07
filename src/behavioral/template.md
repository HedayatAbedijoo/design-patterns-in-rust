# Template Pattern

Defines the skeleton of an algorithm in a base class and allows subclasses to override specific steps of the algorithm while keeping the overall structure intact.

### Example

---

```rust
// Abstract Recipe
trait Recipe {
    fn prepare_ingredients(&self);
    fn cook(&self);
    fn serve(&self);

    fn make_recipe(&self) {
        self.prepare_ingredients();
        self.cook();
        self.serve();
    }
}

// Concrete Recipe: Pasta Carbonara
struct PastaCarbonara;

impl Recipe for PastaCarbonara {
    fn prepare_ingredients(&self) {
        println!("Gather ingredients for Pasta Carbonara");
        println!("Boil water and cook pasta");
        println!("Chop bacon and garlic");
    }

    fn cook(&self) {
        println!("Cook bacon and garlic in a pan");
        println!("Mix cooked pasta with bacon and garlic");
        println!("Whisk eggs and Parmesan cheese");
        println!("Combine egg mixture with pasta");
        println!("Heat the mixture to create a creamy sauce");
    }

    fn serve(&self) {
        println!("Serve Pasta Carbonara with additional Parmesan cheese");
        println!("Enjoy!");
    }
}

// Concrete Recipe: Margherita Pizza
struct MargheritaPizza;

impl Recipe for MargheritaPizza {
    fn prepare_ingredients(&self) {
        println!("Gather ingredients for Margherita Pizza");
        println!("Prepare pizza dough");
        println!("Chop fresh tomatoes and basil leaves");
        println!("Grate mozzarella cheese");
    }

    fn cook(&self) {
        println!("Roll out the pizza dough");
        println!("Spread tomato sauce on the dough");
        println!("Sprinkle mozzarella cheese on top");
        println!("Add fresh tomatoes and basil leaves");
        println!("Bake the pizza in the oven");
    }

    fn serve(&self) {
        println!("Serve Margherita Pizza hot and fresh");
        println!("Enjoy!");
    }
}

fn main() {
    let pasta_carbonara = PastaCarbonara;
    pasta_carbonara.make_recipe();

    println!("------------------------");

    let margherita_pizza = MargheritaPizza;
    margherita_pizza.make_recipe();
}

```
