# GitHub Copilot: Practical Exercises

This file contains hands-on exercises to practice using GitHub Copilot effectively. Work through these exercises to build your skills from basic to advanced.

## How to Use This File

1. Open this file in VS Code with GitHub Copilot enabled
2. For each exercise, write the comment or function signature as indicated
3. Let Copilot suggest the implementation
4. Review and test the generated code
5. Try variations and alternative approaches

---

## Beginner Exercises

### Exercise 1: Basic Function Completion

**Task**: Create a function that converts Celsius to Fahrenheit

**Your Turn**: Type this comment and let Copilot complete it:

```javascript
// Function to convert Celsius to Fahrenheit
function celsiusToFahrenheit(celsius) {
    // Let Copilot complete this
}
```

**Expected Learning**: Basic function completion

---

### Exercise 2: Array Operations

**Task**: Create a function that finds the maximum value in an array

**Your Turn**:

```python
# Function to find the maximum value in an array of numbers
def find_max(numbers):
    # Let Copilot complete this
```

---

### Exercise 3: String Manipulation

**Task**: Create a function that reverses a string

**Your Turn**:

```javascript
// Function to reverse a string
function reverseString(str) {
    // Let Copilot complete this
}
```

---

### Exercise 4: Object Creation

**Task**: Create a class for a basic calculator

**Your Turn**:

```python
# Calculator class with basic operations: add, subtract, multiply, divide
class Calculator:
    # Let Copilot complete this
```

---

### Exercise 5: Conditional Logic

**Task**: Create a function that checks if a number is even or odd

**Your Turn**:

```java
// Function to check if a number is even or odd, return "even" or "odd"
public static String checkEvenOdd(int number) {
    // Let Copilot complete this
}
```

---

## Intermediate Exercises

### Exercise 6: API Data Fetching

**Task**: Create an async function that fetches user data from an API

**Your Turn**:

```javascript
// Async function to fetch user data from JSONPlaceholder API
// Handle errors and return the data or null
async function fetchUser(userId) {
    // Let Copilot complete this
}
```

---

### Exercise 7: Data Filtering

**Task**: Filter an array of objects based on criteria

**Your Turn**:

```javascript
// Function to filter products by price range and category
// products: array of objects with {name, price, category}
// minPrice: minimum price
// maxPrice: maximum price
// category: category to filter (optional, null means all)
function filterProducts(products, minPrice, maxPrice, category = null) {
    // Let Copilot complete this
}
```

---

### Exercise 8: Form Validation

**Task**: Create a comprehensive form validator

**Your Turn**:

```typescript
// Validate user registration form
// Returns object with {isValid: boolean, errors: string[]}
// Validate: email format, password length (min 8), password contains number and uppercase
interface ValidationResult {
    isValid: boolean;
    errors: string[];
}

function validateRegistrationForm(email: string, password: string, confirmPassword: string): ValidationResult {
    // Let Copilot complete this
}
```

---

### Exercise 9: Database Query Builder

**Task**: Create a simple SQL query builder

**Your Turn**:

```python
# Class to build SQL SELECT queries
# Should support: table name, columns, WHERE conditions, ORDER BY, LIMIT
class QueryBuilder:
    # Let Copilot complete this
```

---

### Exercise 10: File Operations

**Task**: Create a function to read and parse CSV files

**Your Turn**:

```python
# Function to read CSV file and return list of dictionaries
# Each dictionary represents a row with column names as keys
import csv

def read_csv_file(filepath):
    # Let Copilot complete this
```

---

## Advanced Exercises

### Exercise 11: Custom Hook in React

**Task**: Create a custom React hook for form handling

**Your Turn**:

```jsx
// Custom hook for form state management
// Should handle: field values, validation, submission, reset
// Returns: values, errors, handleChange, handleSubmit, reset
import { useState } from 'react';

function useForm(initialValues, validationRules, onSubmit) {
    // Let Copilot complete this
}
```

---

### Exercise 12: Debounce Function

**Task**: Implement a debounce utility function

**Your Turn**:

```javascript
// Create a debounce function that delays execution
// func: function to debounce
// delay: delay in milliseconds
// Returns a debounced version of the function
function debounce(func, delay) {
    // Let Copilot complete this
}
```

---

### Exercise 13: Binary Search Tree

**Task**: Implement a Binary Search Tree with insert and search methods

**Your Turn**:

```python
# Binary Search Tree implementation
# Include: Node class, BST class with insert, search, and inorder traversal
class Node:
    # Let Copilot complete this

class BinarySearchTree:
    # Let Copilot complete this
```

---

### Exercise 14: Middleware Pattern

**Task**: Create an Express.js authentication middleware

**Your Turn**:

```javascript
// Authentication middleware for Express.js
// Check for JWT token in Authorization header
// Verify token and attach user to request object
// Return 401 if token is invalid or missing
const jwt = require('jsonwebtoken');

function authMiddleware(req, res, next) {
    // Let Copilot complete this
}
```

---

### Exercise 15: Design Pattern - Observer

**Task**: Implement the Observer pattern

**Your Turn**:

```typescript
// Observer pattern implementation
// Subject class that maintains observers and notifies them
// Observer interface
interface Observer {
    update(data: any): void;
}

class Subject {
    // Let Copilot complete this
}
```

---

## Testing Exercises

### Exercise 16: Unit Tests

**Task**: Write unit tests for a function

**Your Turn**:

```javascript
// Original function
function isPalindrome(str) {
    const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
    return cleaned === cleaned.split('').reverse().join('');
}

// Write Jest unit tests for isPalindrome function
// Test: normal palindrome, not palindrome, with spaces, with special chars, empty string
describe('isPalindrome', () => {
    // Let Copilot complete the tests
});
```

---

### Exercise 17: Integration Test

**Task**: Create integration tests for an API endpoint

**Your Turn**:

```javascript
// Integration tests for user registration API
// Test: successful registration, duplicate email, invalid email, missing fields
const request = require('supertest');
const app = require('../app');

describe('POST /api/register', () => {
    // Let Copilot complete the tests
});
```

---

## Real-World Scenarios

### Exercise 18: E-commerce Cart

**Task**: Build a shopping cart system

**Your Turn**:

```javascript
// Shopping cart class with methods:
// - addItem(product, quantity)
// - removeItem(productId)
// - updateQuantity(productId, quantity)
// - getTotal()
// - applyDiscount(code)
// - checkout()
class ShoppingCart {
    // Let Copilot complete this
}
```

---

### Exercise 19: Rate Limiter

**Task**: Implement a rate limiting system

**Your Turn**:

```python
# Rate limiter class using token bucket algorithm
# Limit requests per user within a time window
from datetime import datetime, timedelta

class RateLimiter:
    # Initialize with max_requests and time_window (in seconds)
    # Method: is_allowed(user_id) returns True/False
    # Let Copilot complete this
```

---

### Exercise 20: Data Caching

**Task**: Create a caching system with TTL

**Your Turn**:

```typescript
// Cache class with time-to-live (TTL) support
// Methods: set(key, value, ttl), get(key), delete(key), clear()
// Automatically remove expired entries
class Cache<T> {
    // Let Copilot complete this
}
```

---

## Challenge Exercises

### Exercise 21: Promise Pool

**Task**: Create a promise pool to limit concurrent operations

**Your Turn**:

```javascript
// PromisePool class to limit concurrent async operations
// Constructor takes concurrency limit
// Method: add(asyncFunction) returns a promise
// Ensures no more than N promises run simultaneously
class PromisePool {
    // Let Copilot complete this
}
```

---

### Exercise 22: Virtual DOM Differ

**Task**: Implement a simple virtual DOM diffing algorithm

**Your Turn**:

```javascript
// Simple virtual DOM diffing
// Compare two virtual DOM trees and return array of changes
// VNode: { type: string, props: object, children: array }
// Changes: { type: 'CREATE'|'UPDATE'|'DELETE', node, parent, ... }
function diffVirtualDOM(oldVNode, newVNode) {
    // Let Copilot complete this
}
```

---

### Exercise 23: Event Emitter

**Task**: Build a custom event emitter

**Your Turn**:

```typescript
// Custom EventEmitter class
// Methods: on(event, callback), off(event, callback), emit(event, ...args), once(event, callback)
class EventEmitter {
    // Let Copilot complete this
}
```

---

### Exercise 24: Lazy Loading Images

**Task**: Create a lazy loading utility for images

**Your Turn**:

```javascript
// Lazy load images using Intersection Observer
// Function takes selector for images to lazy load
// Add data-src attribute to images, load when visible
function lazyLoadImages(selector) {
    // Let Copilot complete this
}
```

---

### Exercise 25: State Machine

**Task**: Implement a finite state machine

**Your Turn**:

```python
# Finite State Machine for order processing
# States: pending, processing, shipped, delivered, cancelled
# Transitions: confirm, ship, deliver, cancel
# Validate state transitions
class OrderStateMachine:
    # Let Copilot complete this
```

---

## Tips for Each Level

### Beginner Tips
1. Start with clear, simple comments
2. Accept suggestions and study them
3. Modify suggestions to understand how they work
4. Test the generated code

### Intermediate Tips
1. Provide more context in comments
2. Use descriptive variable names
3. Try multiple suggestions (Alt+] / Alt+[)
4. Combine Copilot with your own logic

### Advanced Tips
1. Break complex problems into smaller parts
2. Use Copilot Chat for architecture questions
3. Review suggestions critically
4. Optimize generated code for your use case

### Challenge Tips
1. Use Copilot for boilerplate, write core logic yourself
2. Ask Copilot to explain complex suggestions
3. Refactor suggestions for better performance
4. Add comprehensive tests

---

## Practice Projects

After completing exercises, try these complete projects:

### Project 1: Todo API
Build a complete REST API for a todo application with:
- User authentication
- CRUD operations
- Data validation
- Error handling
- Unit tests

### Project 2: Weather Dashboard
Create a weather dashboard with:
- API integration
- Data visualization
- Local storage
- Responsive design
- Error handling

### Project 3: Chat Application
Build a real-time chat app with:
- WebSocket connection
- User authentication
- Message history
- Typing indicators
- File uploads

### Project 4: Blog CMS
Create a blog content management system with:
- Rich text editor
- Image uploads
- Categories and tags
- Search functionality
- SEO optimization

---

## Evaluation Checklist

After each exercise, ask yourself:

- [ ] Did I understand the generated code?
- [ ] Did I test the code with different inputs?
- [ ] Could I write this code myself now?
- [ ] Did I try multiple suggestions?
- [ ] Did I improve or optimize the suggestion?
- [ ] Did I add proper error handling?
- [ ] Did I write tests for the code?

---

## Next Steps

1. Complete exercises in order
2. Experiment with variations
3. Build the practice projects
4. Share your learnings
5. Help others learn Copilot

**Happy Practicing! 🚀**
