# Jenny Stats

A home for Janet data and statistics packages. Pure Janet, no
dependencies beyond the Janet runtime.

Jenny is just Janet.

| package | what | version |
|---|---|---|
| [janet-kit](https://github.com/jennystats/janet-kit) | runtime type guards (`defn/typed`) and utility helpers | 0.1.1 |
| [janet-json](https://github.com/jennystats/janet-json) | JSON encode/decode in plain Janet | - |
| [janet-num](https://github.com/jennystats/janet-num) | small C kernels for numeric hot paths: packed buffers, elementwise and reduction ops, matrix routines, special functions | 0.1.1 |
| [janet-stats](https://github.com/jennystats/janet-stats) | statistics library: d/p/q distributions, hypothesis tests, regression, survival, resampling, Bayes | 0.1.0 |

## The stack

Each package is self-contained and composes with the others:

- **janet-kit** - the foundation: `defn/typed` gives every function
  runtime type guards with docstring support.
- **janet-num** - the compute tier: C kernels via core ffi (no native
  jpm module), called on packed buffers. Install its `libjn.so` with
  the pinned installer.
- **janet-stats** - the statistics tier: pure Janet, self-contained,
  R-convention semantics. Precision boundaries are measured against
  R cell by cell and documented in the contract; `janet-num` adds
  packed-vector lanes for large inputs.

## Contract-driven

Every package ships a measurable API contract (`specs/`):
preconditions, return shapes, error taxonomy, and precision
boundaries - each pinned by regression rows verified against
R 4.5.0. No silent wrong values: where a boundary exists, the
contract and the error say so.

## License

Each repository carries its own `LICENSE`, which is generally AGPL-3.0.
