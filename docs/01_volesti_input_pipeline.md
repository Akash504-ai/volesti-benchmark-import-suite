
# volesti H-Polytope Input Pipeline

## Internal Representation

volesti represents convex polytopes internally as: Ax ≤ b
Where:
- A ∈ R^(m × d)
- b ∈ R^m
- d = dimension
- m = number of inequalities
## .ine File Format
volesti supports cdd-style .ine files structured as:<name> H-representation begin m n real row_1 row_2 

Each row has the form: `b  a1  a2  ...  ad`

This thing encodes: b + a1 x1 + a2 x2 + ... + ad xd ≥ 0

Constructor Transformation : The constructor responsible for parsing .ine data is: `HPolytope(std::vector<std::vector<NT>> const& Pin)`

Dimension Extraction : `_d = Pin[0][1] - 1;`

Matrix Construction : `b(i - 1) = Pin[i][0]; A(i - 1, j - 1) = -Pin[i][j];`

so, each row: [b a1 a2 ... ad]

becomes internally: (-a1)x1 + (-a2)x2 + ... + (-ad)xd ≤ b

Which matches the canonical form: Ax ≤ b

My Observation:volesti ultimately operates strictly in Ax ≤ b form.
The .ine format is only a wrapper that converts data into this internal representation.
