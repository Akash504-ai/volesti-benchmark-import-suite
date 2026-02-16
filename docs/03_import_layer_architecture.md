# Proposed Import Layer Architecture

## Canonical Intermediate Representation

All datasets will be converted into:

```cpp
struct PolytopeData {
    unsigned int dimension;
    Eigen::MatrixXd A;
    Eigen::VectorXd b;
};
```
This separates: Parsing,Validation,Construction

Importer Interfaces ---> 
```cpp
class LPImporter {
public:
    PolytopeData from_mps(const std::string& filename);
};

class MetabolicImporter {
public:
    PolytopeData from_model(const std::string& filename);
};

class PolytopeValidator {
public:
    static bool check_dimension_consistency(const PolytopeData&);
    static bool check_nonempty_constraints(const PolytopeData&);
    static bool check_feasibility(const PolytopeData&);
};
```

Processing Pipeline--->

<img width="299" height="593" alt="image" src="https://github.com/user-attachments/assets/a280d8f4-0fe8-4fd9-9e74-2839b746f01c" />

## Design Principles
Separation of Concerns : Parsing, validation, and construction all of this are independent.

Explicit Validation : No assumptions about the input correctness.

Reproducibility : Conversion pipeline is deterministic and documented.

Extensibility : New dataset types can be supported by adding new importers.
