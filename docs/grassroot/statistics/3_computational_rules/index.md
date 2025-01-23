# Basic Probability Computation Rules

## 1. Fundamental Concepts

### Sample Space
The sample space (Ω) represents all possible outcomes of a random experiment. For example, when rolling a die, Ω = {1, 2, 3, 4, 5, 6}.

### Events
An event is a subset of the sample space. We typically denote events with capital letters (A, B, etc.).

### Probability Measure
For any event A, the probability P(A) must satisfy:
- 0 ≤ P(A) ≤ 1
- P(Ω) = 1
- P(∅) = 0

## 2. Addition Rules

### Basic Addition Rule
For mutually exclusive events A and B:
P(A ∪ B) = P(A) + P(B)

### General Addition Rule
For any two events A and B:
P(A ∪ B) = P(A) + P(B) - P(A ∩ B)

### Extension to Multiple Events
For three events A, B, and C:
P(A ∪ B ∪ C) = P(A) + P(B) + P(C) - P(A ∩ B) - P(A ∩ C) - P(B ∩ C) + P(A ∩ B ∩ C)

## 3. Multiplication Rules

### Independent Events
Two events A and B are independent if:
P(A ∩ B) = P(A) × P(B)

### Dependent Events
For dependent events, use conditional probability:
P(A ∩ B) = P(A) × P(B|A)

### Chain Rule
For multiple events:
P(A ∩ B ∩ C) = P(A) × P(B|A) × P(C|A ∩ B)

## 4. Important Relationships

### Mutually Exclusive Events
Events A and B are mutually exclusive if:
- A ∩ B = ∅
- P(A ∩ B) = 0

### Complement Rule
For any event A:
P(A') = 1 - P(A)

### Conditional Probability
P(B|A) = P(A ∩ B) / P(A), where P(A) > 0

## 5. Common Mistakes and Pitfalls

### Independence vs. Mutual Exclusivity
- Independent events CAN occur together
- Mutually exclusive events CANNOT occur together
- Two events cannot be both independent and mutually exclusive (unless one has probability 0)

### Addition Rule Selection
- Use basic addition rule ONLY for mutually exclusive events
- Always use general addition rule when events can overlap

## 6. Practical Applications

### Example: Card Drawing
Drawing cards from a standard deck:
- P(Drawing a King) = 4/52 = 1/13
- P(Drawing a Heart) = 13/52 = 1/4
- P(Drawing a King of Hearts) = 1/52
- P(Drawing a King OR a Heart) = 15/52 (using general addition rule)

### Example: Rolling Dice
Rolling two dice:
- P(Sum = 7) = 6/36 = 1/6
- P(First die = 6) = 1/6
- These events are independent: knowing the sum doesn't tell us the value of the first die

## 7. Verification Methods

### Testing for Independence
To verify if events A and B are independent, check if:
P(A ∩ B) = P(A) × P(B)

### Testing for Mutual Exclusivity
To verify if events A and B are mutually exclusive, check if:
P(A ∩ B) = 0

## 8. Additional Considerations

### Law of Total Probability
For a partition of events B₁, B₂, ..., Bₙ:
P(A) = P(A|B₁)P(B₁) + P(A|B₂)P(B₂) + ... + P(A|Bₙ)P(Bₙ)

### Bayes' Theorem
P(A|B) = P(B|A)P(A) / P(B)

## Practice Problems
1. If P(A) = 0.3, P(B) = 0.4, and P(A ∩ B) = 0.12, calculate:
   - P(A ∪ B)
   - Are A and B independent?

2. Three cards are drawn from a deck without replacement. Calculate:
   - P(All are aces)
   - P(All are the same suit)

[Solutions provided in separate section]