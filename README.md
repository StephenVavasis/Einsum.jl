# Einsum.jl
Einstein summation notation in julia. Similar to numpy's [`einsum`](http://docs.scipy.org/doc/numpy-1.10.0/reference/generated/numpy.einsum.html) function.

### Example:

```julia
using Einsum
A = zeros(5,6,7); # need to preallocate destination
X = randn(5,2);
Y = randn(6,2);
Z = randn(7,2);
@einsum A[i,j,k] = X[i,r]*Y[j,r]*Z[k,r]
```

The `@einsum` macro automatically generates code that looks much like the following (note that we "sum out" over the index `r`, since it only occurs on the right hand side of the equation):

```julia
for k = 1:size(A,3)
    for j = 1:size(A,2)
        for i = 1:size(A,1)
            s = 0
            for r = 1:size(X,2)
                s += X[i,r] * Y[j,r] * Z[k,r]
            end
            A[i,j,k] = s
        end
    end
end
```

To see exactly what is generated, use [`macroexpand`](http://docs.julialang.org/en/release-0.4/manual/metaprogramming/#macros):

```julia
macroexpand(:(@einsum A[i,j,k] = X[i,r]*Y[j,r]*Z[k,r]))
```

### Related Packages:

* https://github.com/Jutho/TensorOperations.jl