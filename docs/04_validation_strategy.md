

# Validation Strategy
A structured import layer must validate data before constructing the HPolytope.

1. Dimension Consistency
   
- A.cols() == dimension
- A.rows() == b.size()

2. Non-Empty Constraint Set
- At least one inequality
- dimension > 0

3. Bounds Conversion
LP bounds: l_i ≤ x_i ≤ u_i

Convert into inequalities:

x_i ≤ u_i
-x_i ≤ -l_i

Each bound becomes additional rows in matrix A.

4. Equality Handling
Equalities:Ax = b

Convert into:

Ax ≤ b
-Ax ≤ -b

5. Feasibility Check
Optionally:

- Attempt Chebyshev ball computation
- Detect immediate infeasibility
- Verify numerical sanity

## Final Objective

Ensure all datasets are converted into:HPolytope(d, A, b);
