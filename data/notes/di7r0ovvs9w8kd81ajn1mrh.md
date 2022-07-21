
```
_, unique_indices, unique_inverse = np.unique(points, return_inverse=True, return_index=True, axis=0)
faceVertexIndices = Vt.IntArray((unique_indices[unique_inverse])[faceVertexIndices].tolist())
```

- `unique_indices` The indices of the first occurrences of the - unique values in the original array.
- `unique_inverse` The indices to reconstruct the original array from the unique array.
- `unique_indices[unique_inverse]`: same shape as original array, but with each element be the index of the first occurrences of the unique value
