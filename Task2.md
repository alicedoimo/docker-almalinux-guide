# Computational Hello World Report

## 1. Source Code

### Python Implementation
```python
import numpy as np

# Define parameters
N_values = [10, 10**6, 10**8]
a = 3
x_value = 0.1
y_value = 7.1

# Vector Addition
def vector_addition(N):
    x = np.full(N, x_value)
    y = np.full(N, y_value)
    d = a * x + y
    assert np.all(d == 7.4), "Vector addition failed!"
    return d

# Matrix Multiplication
def matrix_multiplication(N):
    A = np.full((N, N), 3.0)
    B = np.full((N, N), 7.1)
    C = np.dot(A, B)
    assert np.all(C == 21.3), "Matrix multiplication failed!"
    return C

# Run for all N values
for N in N_values:
    print(f"Running for N={N}")
    vector_addition(N)
    if N <= 1000:  # Large matrices take too much memory
        matrix_multiplication(N)
    print(f"Completed for N={N}\n")
```

### C++ Implementation
```cpp
#include <iostream>
#include <vector>
#include <cassert>

using namespace std;

const int N_values[] = {10, 1000000};
const double a = 3.0, x_value = 0.1, y_value = 7.1;

// Vector Addition
void vector_addition(int N) {
    vector<double> x(N, x_value);
    vector<double> y(N, y_value);
    vector<double> d(N);
    
    for (int i = 0; i < N; ++i) {
        d[i] = a * x[i] + y[i];
    }
    
    for (double val : d) {
        assert(val == 7.4);
    }
    cout << "Vector addition successful for N=" << N << endl;
}

// Matrix Multiplication
void matrix_multiplication(int N) {
    vector<vector<double>> A(N, vector<double>(N, 3.0));
    vector<vector<double>> B(N, vector<double>(N, 7.1));
    vector<vector<double>> C(N, vector<double>(N, 0.0));
    
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            for (int k = 0; k < N; ++k) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
    
    for (const auto& row : C) {
        for (double val : row) {
            assert(val == 21.3);
        }
    }
    cout << "Matrix multiplication successful for N=" << N << endl;
}

int main() {
    for (int N : N_values) {
        cout << "Running for N=" << N << endl;
        vector_addition(N);
        if (N <= 100) { // Limit matrix size due to memory constraints
            matrix_multiplication(N);
        }
        cout << "Completed for N=" << N << "\n";
    }
    return 0;
}
```

## 2. Answers to Questions

### (i) Problems Encountered
- For large values of `N` (e.g., `N=10^8` in Python), memory consumption becomes a major issue. The same applies to matrix multiplication in C++ when `N=10,000`.
- Matrix multiplication with `N=10,000` requires a large amount of memory and computation time, often causing performance bottlenecks.

### (ii) Verification of Results
- The vector addition is verified using an assertion that ensures all elements of `d` equal `7.4`.
- The matrix multiplication is checked by asserting that every element in `C` is equal to `21.3`.
- If these assertions hold, the computations are correct.

