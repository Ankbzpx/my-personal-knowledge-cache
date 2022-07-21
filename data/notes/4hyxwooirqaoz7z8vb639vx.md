
## Types

### Vector
Vector of dim n

### Matrix
Matrix of shape n x m

Note `RowVectorXd` is Convenience typedefs for 1 x n matrix

### Array
General purpose array, support coefficient-wise operation with no linear algebra meaning

```
Eigen::VectorXd S; // VectorXd
s.array(); // Array
```

## Partial reduction
- `rowwise()`: treat as 1 x cols()
- `colwise()`: treat as rows() x 1

### Example 1
```
V: n x 3 (MatrixXd)
TT: 4 x 4 transformation matrix

(V.rowwise().homogeneous()*TT).rowwise().hnormalized())
```

### Example 2
```
P: n x 3 (MatrixXd)
v: 1 x 3 (RowVector3d)

P.rowwise() - v: n x 3
```

### Example 3
```
P: n x 3 (MatrixXd)
v: n (VectorXd)

(P.array().colwise() * v.array()).matrix()
```

## Concatenation
Create matrix/vector of target size, then assign block

```
A: n x 3
B: n x 3

Eigen::MatrixXd C(2 * m, 3);
C << A, B;
```