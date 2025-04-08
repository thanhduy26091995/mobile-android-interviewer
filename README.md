# mobile-interviewer

### 1. Inline Function

The `inline` keyword is used to optimize **higher-order functions**. When a function is marked as `inline`, the compiler copies the function body along with the lambda directly into the call site.

#### ✅ Benefits
- Avoids object creation for lambdas
- Improves performance (especially for small functions)
