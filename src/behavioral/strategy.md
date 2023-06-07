# Strategy Pattern

Defines a family of interchangeable algorithms and encapsulates each algorithm separately, allowing them to be used interchangeably based on specific requirements.

### Example

---

```rust
// Payment Strategy trait
trait PaymentStrategy {
    fn process_payment(&self, amount: f64);
}

// Credit Card Payment Strategy
struct CreditCardPaymentStrategy {
    card_number: String,
    expiration_date: String,
    cvv: String,
}

impl PaymentStrategy for CreditCardPaymentStrategy {
    fn process_payment(&self, amount: f64) {
        println!("Processing credit card payment of {} USD", amount);
        // Logic to process payment with credit card
    }
}

// PayPal Payment Strategy
struct PayPalPaymentStrategy {
    email: String,
    password: String,
}

impl PaymentStrategy for PayPalPaymentStrategy {
    fn process_payment(&self, amount: f64) {
        println!("Processing PayPal payment of {} USD", amount);
        // Logic to process payment with PayPal
    }
}

// Bank Transfer Payment Strategy
struct BankTransferPaymentStrategy {
    account_number: String,
    routing_number: String,
}

impl PaymentStrategy for BankTransferPaymentStrategy {
    fn process_payment(&self, amount: f64) {
        println!("Processing bank transfer payment of {} USD", amount);
        // Logic to process payment with bank transfer
    }
}

// Payment Context
struct PaymentContext {
    payment_strategy: Box<dyn PaymentStrategy>,
}

impl PaymentContext {
    fn new(payment_strategy: Box<dyn PaymentStrategy>) -> Self {
        PaymentContext { payment_strategy }
    }

    fn process_payment(&self, amount: f64) {
        self.payment_strategy.process_payment(amount);
    }
}

fn main() {
    let credit_card_strategy = Box::new(CreditCardPaymentStrategy {
        card_number: "1234 5678 9012 3456".to_string(),
        expiration_date: "12/23".to_string(),
        cvv: "123".to_string(),
    });

    let paypal_strategy = Box::new(PayPalPaymentStrategy {
        email: "user@example.com".to_string(),
        password: "password123".to_string(),
    });

    let bank_transfer_strategy = Box::new(BankTransferPaymentStrategy {
        account_number: "123456789".to_string(),
        routing_number: "987654321".to_string(),
    });

    let payment_amount = 100.00;

    let credit_card_context = PaymentContext::new(credit_card_strategy);
    credit_card_context.process_payment(payment_amount);

    let paypal_context = PaymentContext::new(paypal_strategy);
    paypal_context.process_payment(payment_amount);

    let bank_transfer_context = PaymentContext::new(bank_transfer_strategy);
    bank_transfer_context.process_payment(payment_amount);
}

```
