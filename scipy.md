# Scipy cheatsheet

## Sparse matrices

- `coo_matrix` (coordinate sparse matrix).
  - Three different arrays are stored: `data`, `row`, and `col`.
  - Pros:
    - Fast incremental construction
    - Fast item-wise arithmetics
    - Fast conversion to CSR/CSC formats
  - Cons:
    - Slow access
    - Slow vector/matrix arithmetics
- `csr_matrix` (compressed sparse row matrix).
  - Three different arrays are stored internally:
    - `data`. contains all non-zero entries in row-major order
    - `indptr`.
      - points to row starts.
      - `row_start = indptr[row]`
      - `row_end = indptr[row + 1]`
      - `values = data[row_start:row_end]`
      - `column_indices = indices[row_start:row_end]`
    - `indices`. array of column indices
  - Pros:
    - Efficient item access and slicing
    - Fast arithmetics
    - Fast vector products
  - Cons:
    - Expensive editing
- `csc_matrix` (compressed sparse column matrix).
  - Pros:
    - Efficient item access and slicing
    - Fast arithmetics
    - Fast vector products
  - Cons:
    - Expensive editing
