> Reference: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.118.8397&rep=rep1&type=pdf

- Traverse cells in volume checking bounds for isovalue, can be solved in close form as root of cubic polynomial
- Speed up traversal using spatial index (like R-tree)

## Fast Voxel Traversal Algorithm
> https://www.flipcode.com/archives/A%20faster%20voxel%20traversal%20algorithm%20for%20ray%20tracing.pdf

### Intersection culling
- Hierarchical bounding volumes
- Space partitioning (octree/gird)

### Algorithm
Break ray down into intervals of size $t$ spanning one voxel

$$
\bm{u} + t \bm{v}
$$

#### Initialization: 
- Find voxel the ray originated, or entry voxel if origin is outside voxel
- Compute minimum of voxel boundary intersect, as maximum distance of traversal that still inside the grid
- Compute delta $t$ along all axis that equals to unit voxel size

#### Incremental traversal
- Find axis with minimum boundary intersect
- Increment by the delta along that axis
- Move to next grid and repeat

## TODO
- [ ] DDA (Digital Differential Analyzer) algorithm