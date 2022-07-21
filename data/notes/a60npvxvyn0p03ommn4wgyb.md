
For non-zero $\bm{v}$ that satisfy
$$
\bm{A} \bm{v} = \lambda \bm{v} = \lambda \bm{I} \bm{v} \\
$$
So
$$
(\bm{A} - \lambda \bm{I}) \bm{v} = \bm{0}
\\
\det{(\bm{A} - \lambda \bm{I})} = 0
$$

$\bm{v}$ is a eigenvector of $\bm{A}$ with corresponding eigenvalue $\lambda$. Note eigenvectors only scales during transformation


## Eigenvalue properties

**Complex eigenvalues**: correspond to rotation in transformation, no eigenvectors

**Eigenvalue euqal to 1**: fix in place during transformation

## Eigenbasis
Choose a set of eigenvectors that span whole space, use eigenvectors as basis vector

$$
\bm{Q}^{-1} \bm{A} \bm{Q} = \bm{\Lambda} \\
\bm{A} = \bm{Q} \bm{\Lambda} \bm{Q}^{-1}  
$$

> Eigen basis $T_{ne}$ that transform eigen coordinate system into normal coordinate system

See: [[linear-algebra.decomposition.eigendecomposition]]

Transformation applied on eigen basis is guaranteed to be diagonal

To compute chain multiplication of same matrix n times, its easier to change it to eigenbasis, transform n times then change it back. See [[Change of basis|linear-algebra.matrix-vector-dot-product#change-of-basis]]

$$
\bm{A}^n = \bm{Q} \bm{\Lambda}^n \bm{Q}^{-1}
$$

## Relation to rank

> Reference: https://math.stackexchange.com/questions/1349907/what-is-the-relation-between-rank-of-a-matrix-its-eigenvalues-and-eigenvectors

For square matrix $\bm{A} \in \mathbb{R}^{n \times n}$, $n = \text{rank} + \text{nullity}$, kernel of $\bm{A}$ is the eigenspace with 0 eigenvalue
