# GitHub Copilot: Complete Code Examples

Ready-to-use code examples demonstrating Copilot's capabilities across different scenarios and languages.

---

## Table of Contents

1. [JavaScript/Node.js Examples](#javascriptnodejs-examples)
2. [Python Examples](#python-examples)
3. [TypeScript/React Examples](#typescriptreact-examples)

---

## JavaScript/Node.js Examples

### 1. Express REST API Server

```javascript
// Complete Express server with authentication and CRUD operations
const express = require('express');
const cors = require('cors');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const mongoose = require('mongoose');

const app = express();

// IMPORTANT: Set JWT_SECRET environment variable before running
// Example: export JWT_SECRET="your-secret-key-here"
if (!process.env.JWT_SECRET) {
    console.error('ERROR: JWT_SECRET environment variable is not set!');
    process.exit(1);
}

// Middleware
app.use(cors());
app.use(express.json());

// MongoDB Connection
mongoose.connect('mongodb://localhost:27017/myapp', {
    useNewUrlParser: true,
    useUnifiedTopology: true
});

// User Model
const userSchema = new mongoose.Schema({
    username: { type: String, required: true, unique: true },
    email: { type: String, required: true, unique: true },
    password: { type: String, required: true },
    createdAt: { type: Date, default: Date.now }
});

const User = mongoose.model('User', userSchema);

// Authentication Middleware
const authenticateToken = (req, res, next) => {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1];

    if (!token) {
        return res.status(401).json({ error: 'Access denied' });
    }

    if (!process.env.JWT_SECRET) {
        return res.status(500).json({ error: 'Server configuration error' });
    }
    
    jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
        if (err) {
            return res.status(403).json({ error: 'Invalid token' });
        }
        req.user = user;
        next();
    });
};

// Routes
// Register
app.post('/api/register', async (req, res) => {
    try {
        const { username, email, password } = req.body;

        // Validation
        if (!username || !email || !password) {
            return res.status(400).json({ error: 'All fields required' });
        }

        // Check if user exists
        const existingUser = await User.findOne({ 
            $or: [{ email }, { username }] 
        });

        if (existingUser) {
            return res.status(400).json({ error: 'User already exists' });
        }

        // Hash password
        const hashedPassword = await bcrypt.hash(password, 10);

        // Create user
        const user = new User({
            username,
            email,
            password: hashedPassword
        });

        await user.save();

        res.status(201).json({ 
            message: 'User created successfully',
            userId: user._id
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Login
app.post('/api/login', async (req, res) => {
    try {
        const { email, password } = req.body;

        // Find user
        const user = await User.findOne({ email });
        if (!user) {
            return res.status(400).json({ error: 'Invalid credentials' });
        }

        // Check password
        const validPassword = await bcrypt.compare(password, user.password);
        if (!validPassword) {
            return res.status(400).json({ error: 'Invalid credentials' });
        }

        // Generate token
        if (!process.env.JWT_SECRET) {
            return res.status(500).json({ error: 'Server configuration error' });
        }
        
        const token = jwt.sign(
            { userId: user._id, email: user.email },
            process.env.JWT_SECRET,
            { expiresIn: '24h' }
        );

        res.json({ 
            token,
            user: {
                id: user._id,
                username: user.username,
                email: user.email
            }
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Protected route example
app.get('/api/profile', authenticateToken, async (req, res) => {
    try {
        const user = await User.findById(req.user.userId).select('-password');
        res.json(user);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Error handling middleware
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong!' });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});

module.exports = app;
```

### 2. Async Data Processing Pipeline

```javascript
// Process large datasets asynchronously with error handling
class DataProcessor {
    constructor(options = {}) {
        this.batchSize = options.batchSize || 100;
        this.concurrency = options.concurrency || 5;
        this.retryAttempts = options.retryAttempts || 3;
    }

    async processInBatches(data, processFn) {
        const results = [];
        const errors = [];

        for (let i = 0; i < data.length; i += this.batchSize) {
            const batch = data.slice(i, i + this.batchSize);
            
            try {
                const batchResults = await this.processBatch(batch, processFn);
                results.push(...batchResults);
                console.log(`Processed batch ${i / this.batchSize + 1}`);
            } catch (error) {
                errors.push({ batch: i / this.batchSize + 1, error });
            }
        }

        return { results, errors };
    }

    async processBatch(batch, processFn) {
        const chunks = this.chunkArray(batch, this.concurrency);
        const results = [];

        for (const chunk of chunks) {
            const promises = chunk.map(item => 
                this.processWithRetry(item, processFn)
            );
            const chunkResults = await Promise.allSettled(promises);
            results.push(...chunkResults.filter(r => r.status === 'fulfilled')
                                       .map(r => r.value));
        }

        return results;
    }

    async processWithRetry(item, processFn, attempt = 1) {
        try {
            return await processFn(item);
        } catch (error) {
            if (attempt < this.retryAttempts) {
                console.log(`Retry attempt ${attempt} for item`, item);
                await this.delay(1000 * attempt);
                return this.processWithRetry(item, processFn, attempt + 1);
            }
            throw error;
        }
    }

    chunkArray(array, size) {
        const chunks = [];
        for (let i = 0; i < array.length; i += size) {
            chunks.push(array.slice(i, i + size));
        }
        return chunks;
    }

    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
}

// Usage example
async function example() {
    const processor = new DataProcessor({
        batchSize: 50,
        concurrency: 3,
        retryAttempts: 2
    });

    const data = Array.from({ length: 1000 }, (_, i) => ({ id: i }));

    const processItem = async (item) => {
        // Simulate API call
        await new Promise(resolve => setTimeout(resolve, 100));
        return { ...item, processed: true };
    };

    const { results, errors } = await processor.processInBatches(data, processItem);
    console.log(`Processed ${results.length} items with ${errors.length} errors`);
}
```

---

## Python Examples

### 1. Web Scraper with Error Handling

```python
import requests
from bs4 import BeautifulSoup
import csv
import time
from typing import List, Dict
import logging

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class WebScraper:
    """
    A robust web scraper with rate limiting and error handling
    """
    
    def __init__(self, base_url: str, delay: float = 1.0):
        self.base_url = base_url
        self.delay = delay
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        })
    
    def fetch_page(self, url: str, retries: int = 3) -> requests.Response:
        """
        Fetch a page with retry logic
        """
        for attempt in range(retries):
            try:
                response = self.session.get(url, timeout=10)
                response.raise_for_status()
                time.sleep(self.delay)
                return response
            except requests.RequestException as e:
                logger.warning(f"Attempt {attempt + 1} failed: {e}")
                if attempt == retries - 1:
                    raise
                time.sleep(self.delay * (attempt + 1))
    
    def parse_product_list(self, html: str) -> List[Dict]:
        """
        Parse product information from HTML
        """
        soup = BeautifulSoup(html, 'html.parser')
        products = []
        
        for item in soup.find_all('div', class_='product'):
            try:
                product = {
                    'name': item.find('h2', class_='title').text.strip(),
                    'price': self.extract_price(item.find('span', class_='price').text),
                    'rating': self.extract_rating(item.find('div', class_='rating')),
                    'url': item.find('a')['href']
                }
                products.append(product)
            except (AttributeError, KeyError) as e:
                logger.error(f"Error parsing product: {e}")
                continue
        
        return products
    
    def extract_price(self, price_text: str) -> float:
        """
        Extract numeric price from text
        """
        import re
        price_match = re.search(r'[\d,]+\.?\d*', price_text)
        if price_match:
            return float(price_match.group().replace(',', ''))
        return 0.0
    
    def extract_rating(self, rating_element) -> float:
        """
        Extract rating from element
        """
        if rating_element:
            rating_text = rating_element.get('data-rating', '0')
            return float(rating_text)
        return 0.0
    
    def scrape_multiple_pages(self, start_page: int, end_page: int) -> List[Dict]:
        """
        Scrape multiple pages and aggregate results
        """
        all_products = []
        
        for page in range(start_page, end_page + 1):
            try:
                url = f"{self.base_url}?page={page}"
                logger.info(f"Scraping page {page}")
                
                response = self.fetch_page(url)
                products = self.parse_product_list(response.text)
                all_products.extend(products)
                
                logger.info(f"Found {len(products)} products on page {page}")
            except Exception as e:
                logger.error(f"Error scraping page {page}: {e}")
                continue
        
        return all_products
    
    def save_to_csv(self, products: List[Dict], filename: str):
        """
        Save products to CSV file
        """
        if not products:
            logger.warning("No products to save")
            return
        
        keys = products[0].keys()
        
        with open(filename, 'w', newline='', encoding='utf-8') as f:
            writer = csv.DictWriter(f, fieldnames=keys)
            writer.writeheader()
            writer.writerows(products)
        
        logger.info(f"Saved {len(products)} products to {filename}")

# Usage example
if __name__ == "__main__":
    scraper = WebScraper("https://example.com/products", delay=2.0)
    products = scraper.scrape_multiple_pages(1, 5)
    scraper.save_to_csv(products, "products.csv")
```

### 2. Data Analysis and Visualization

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from typing import Tuple

class DataAnalyzer:
    """
    Comprehensive data analysis toolkit
    """
    
    def __init__(self, data_path: str):
        self.df = pd.read_csv(data_path)
        self.clean_data()
    
    def clean_data(self):
        """
        Clean and prepare data for analysis
        """
        # Remove duplicates
        self.df = self.df.drop_duplicates()
        
        # Handle missing values
        numeric_columns = self.df.select_dtypes(include=[np.number]).columns
        self.df[numeric_columns] = self.df[numeric_columns].fillna(
            self.df[numeric_columns].median()
        )
        
        # Remove outliers using IQR method
        for col in numeric_columns:
            Q1 = self.df[col].quantile(0.25)
            Q3 = self.df[col].quantile(0.75)
            IQR = Q3 - Q1
            lower_bound = Q1 - 1.5 * IQR
            upper_bound = Q3 + 1.5 * IQR
            self.df = self.df[
                (self.df[col] >= lower_bound) & 
                (self.df[col] <= upper_bound)
            ]
    
    def generate_summary_statistics(self) -> pd.DataFrame:
        """
        Generate comprehensive summary statistics
        """
        summary = self.df.describe()
        
        # Add additional metrics
        summary.loc['median'] = self.df.median(numeric_only=True)
        summary.loc['mode'] = self.df.mode(numeric_only=True).iloc[0]
        summary.loc['skewness'] = self.df.skew(numeric_only=True)
        summary.loc['kurtosis'] = self.df.kurtosis(numeric_only=True)
        
        return summary
    
    def correlation_analysis(self, threshold: float = 0.5) -> Tuple[pd.DataFrame, plt.Figure]:
        """
        Perform correlation analysis and create heatmap
        """
        # Calculate correlation matrix
        corr_matrix = self.df.corr(numeric_only=True)
        
        # Create heatmap
        fig, ax = plt.subplots(figsize=(12, 10))
        sns.heatmap(
            corr_matrix,
            annot=True,
            fmt='.2f',
            cmap='coolwarm',
            center=0,
            ax=ax
        )
        plt.title('Correlation Matrix Heatmap')
        
        # Find strong correlations
        strong_corr = corr_matrix[
            (corr_matrix > threshold) | (corr_matrix < -threshold)
        ].stack().reset_index()
        strong_corr.columns = ['Variable 1', 'Variable 2', 'Correlation']
        strong_corr = strong_corr[
            strong_corr['Variable 1'] != strong_corr['Variable 2']
        ]
        
        return strong_corr, fig
    
    def time_series_analysis(self, date_column: str, value_column: str):
        """
        Analyze time series data
        """
        # Convert to datetime
        self.df[date_column] = pd.to_datetime(self.df[date_column])
        self.df = self.df.sort_values(date_column)
        
        # Create time-based features
        self.df['year'] = self.df[date_column].dt.year
        self.df['month'] = self.df[date_column].dt.month
        self.df['day_of_week'] = self.df[date_column].dt.dayofweek
        
        # Calculate moving averages
        self.df['MA_7'] = self.df[value_column].rolling(window=7).mean()
        self.df['MA_30'] = self.df[value_column].rolling(window=30).mean()
        
        # Plot
        fig, ax = plt.subplots(figsize=(15, 6))
        ax.plot(self.df[date_column], self.df[value_column], label='Original')
        ax.plot(self.df[date_column], self.df['MA_7'], label='7-day MA')
        ax.plot(self.df[date_column], self.df['MA_30'], label='30-day MA')
        ax.legend()
        plt.title(f'{value_column} Over Time')
        
        return fig
    
    def export_insights(self, filename: str):
        """
        Export analysis insights to file
        """
        with open(filename, 'w') as f:
            f.write("Data Analysis Report\n")
            f.write("=" * 50 + "\n\n")
            
            f.write("Summary Statistics:\n")
            f.write(self.generate_summary_statistics().to_string())
            f.write("\n\n")
            
            f.write(f"Total Records: {len(self.df)}\n")
            f.write(f"Columns: {', '.join(self.df.columns)}\n")
```

---

## TypeScript/React Examples

### 1. Custom Hooks and Context

```typescript
// useAuth.ts - Authentication hook with context
import { createContext, useContext, useState, useEffect, ReactNode } from 'react';

interface User {
    id: string;
    email: string;
    name: string;
}

interface AuthContextType {
    user: User | null;
    isLoading: boolean;
    isAuthenticated: boolean;
    login: (email: string, password: string) => Promise<void>;
    logout: () => Promise<void>;
    register: (email: string, password: string, name: string) => Promise<void>;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
    const [user, setUser] = useState<User | null>(null);
    const [isLoading, setIsLoading] = useState(true);

    useEffect(() => {
        // Check for stored token on mount
        const token = localStorage.getItem('token');
        if (token) {
            validateToken(token);
        } else {
            setIsLoading(false);
        }
    }, []);

    const validateToken = async (token: string) => {
        try {
            const response = await fetch('/api/validate', {
                headers: { Authorization: `Bearer ${token}` }
            });
            
            if (response.ok) {
                const userData = await response.json();
                setUser(userData);
            } else {
                localStorage.removeItem('token');
            }
        } catch (error) {
            console.error('Token validation failed:', error);
        } finally {
            setIsLoading(false);
        }
    };

    const login = async (email: string, password: string) => {
        setIsLoading(true);
        try {
            const response = await fetch('/api/login', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ email, password })
            });

            if (!response.ok) {
                throw new Error('Login failed');
            }

            const { token, user: userData } = await response.json();
            localStorage.setItem('token', token);
            setUser(userData);
        } finally {
            setIsLoading(false);
        }
    };

    const logout = async () => {
        localStorage.removeItem('token');
        setUser(null);
    };

    const register = async (email: string, password: string, name: string) => {
        setIsLoading(true);
        try {
            const response = await fetch('/api/register', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ email, password, name })
            });

            if (!response.ok) {
                throw new Error('Registration failed');
            }

            // Automatically log in after registration
            await login(email, password);
        } finally {
            setIsLoading(false);
        }
    };

    const value = {
        user,
        isLoading,
        isAuthenticated: !!user,
        login,
        logout,
        register
    };

    return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
};

export const useAuth = () => {
    const context = useContext(AuthContext);
    if (context === undefined) {
        throw new Error('useAuth must be used within an AuthProvider');
    }
    return context;
};
```

### 2. Advanced Form Handling

```typescript
// useForm.ts - Comprehensive form hook
import { useState, useCallback, ChangeEvent, FormEvent } from 'react';

interface ValidationRule {
    required?: boolean;
    minLength?: number;
    maxLength?: number;
    pattern?: RegExp;
    custom?: (value: any) => string | null;
}

interface ValidationRules {
    [key: string]: ValidationRule;
}

export function useForm<T extends Record<string, any>>(
    initialValues: T,
    validationRules: ValidationRules,
    onSubmit: (values: T) => Promise<void> | void
) {
    const [values, setValues] = useState<T>(initialValues);
    const [errors, setErrors] = useState<Partial<Record<keyof T, string>>>({});
    const [touched, setTouched] = useState<Partial<Record<keyof T, boolean>>>({});
    const [isSubmitting, setIsSubmitting] = useState(false);

    const validate = useCallback((name: keyof T, value: any): string | null => {
        const rules = validationRules[name as string];
        if (!rules) return null;

        if (rules.required && !value) {
            return 'This field is required';
        }

        if (rules.minLength && value.length < rules.minLength) {
            return `Minimum length is ${rules.minLength}`;
        }

        if (rules.maxLength && value.length > rules.maxLength) {
            return `Maximum length is ${rules.maxLength}`;
        }

        if (rules.pattern && !rules.pattern.test(value)) {
            return 'Invalid format';
        }

        if (rules.custom) {
            return rules.custom(value);
        }

        return null;
    }, [validationRules]);

    const validateAll = useCallback((): boolean => {
        const newErrors: Partial<Record<keyof T, string>> = {};
        let isValid = true;

        Object.keys(validationRules).forEach(key => {
            const error = validate(key as keyof T, values[key]);
            if (error) {
                newErrors[key as keyof T] = error;
                isValid = false;
            }
        });

        setErrors(newErrors);
        return isValid;
    }, [values, validate, validationRules]);

    const handleChange = (e: ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
        const { name, value, type } = e.target;
        const finalValue = type === 'checkbox' ? (e.target as HTMLInputElement).checked : value;

        setValues(prev => ({ ...prev, [name]: finalValue }));

        if (touched[name as keyof T]) {
            const error = validate(name as keyof T, finalValue);
            setErrors(prev => ({ ...prev, [name]: error || undefined }));
        }
    };

    const handleBlur = (e: ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
        const { name } = e.target;
        setTouched(prev => ({ ...prev, [name]: true }));

        const error = validate(name as keyof T, values[name as keyof T]);
        setErrors(prev => ({ ...prev, [name]: error || undefined }));
    };

    const handleSubmit = async (e: FormEvent) => {
        e.preventDefault();
        
        // Mark all fields as touched
        const allTouched = Object.keys(values).reduce((acc, key) => ({
            ...acc,
            [key]: true
        }), {});
        setTouched(allTouched);

        if (!validateAll()) {
            return;
        }

        setIsSubmitting(true);
        try {
            await onSubmit(values);
        } catch (error) {
            console.error('Form submission error:', error);
        } finally {
            setIsSubmitting(false);
        }
    };

    const reset = () => {
        setValues(initialValues);
        setErrors({});
        setTouched({});
        setIsSubmitting(false);
    };

    return {
        values,
        errors,
        touched,
        isSubmitting,
        handleChange,
        handleBlur,
        handleSubmit,
        reset,
        setValues,
        setErrors
    };
}
```

---

## Additional Examples Available

This file contains foundational examples. For more advanced examples including:

- Java Spring Boot applications
- Database query builders and ORMs
- GraphQL APIs
- WebSocket implementations
- Microservices patterns
- Testing frameworks
- CI/CD configurations

Refer to the main course materials and use GitHub Copilot to generate these examples based on your specific needs!

---

**Pro Tip**: Use these examples as templates. Copy them into your project and let Copilot help you customize them for your specific requirements!
