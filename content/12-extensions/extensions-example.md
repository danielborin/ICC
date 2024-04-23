# Example Extension

Let's rewrite our Mandelbrot generator using Numba to see how it differs.

## NumPy array syntax

Here's an example of a python implementation using NumPy array operations:


```{literalinclude} ../../examples/extensions/python/mandel.py
:language: python
```

We can test this as:

```{literalinclude} ../../examples/extensions/python/test_mandel.py
:language: python
```

## Python with explicit loops

Here's a version where the loops are explicitly written out:

```python
import numpy as np

def mandelbrot2(N,
                xmin=-2.0, xmax=2.0,
                ymin=-2.0, ymax=2.0,
                max_iter=10):

    x = np.linspace(xmin, xmax, N)
    y = np.linspace(ymin, ymax, N)

    c = np.zeros((N, N), dtype=np.complex128)

    for i in range(N):
        for j in range(N):
            c[i, j] = x[i] + 1j * y[j]

    z = np.zeros((N, N), dtype=np.complex128)

    # note: we need to use a numba type here
    m = np.zeros((N, N), dtype=np.int32)

    for n in range(max_iter):

        for i in range(N):
            for j in range(N):
                if m[i, j] == 0:
                    z[i, j] = z[i, j] * z[i, j] + c[i, j]

                    if np.abs(z[i, j]) > 2:
                        m[i, j] = n

    return m
```


## Numba version

To get a Numba optimized version of the python with explicit loops we just add:

```python
from numba import njit
```

and then right before the function definition:

```python
@njit()
```

