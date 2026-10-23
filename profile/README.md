# Jenny Stats

A home for Janet data and statistics packages. The pure-Janet tiers
(kit, json, stats) have no dependencies beyond the Janet runtime; the
native tiers bind external C libraries - each pinned to a specific
upstream release and installed by a script in its repo (janet-num's
kernels, duckdb, xgboost, rayforce). Janet-package dependencies are
declared and tag-pinned in every manifest.

Jenny is just Janet.

| package | what | version |
|---|---|---|
| [janet&#8209;kit](https://github.com/jennystats/janet-kit) | runtime type guards (`defn/typed`) and utility helpers | 0.1.3 |
| [janet&#8209;json](https://github.com/jennystats/janet-json) | JSON encode/decode in plain Janet | 0.1.3 |
| [janet&#8209;num](https://github.com/jennystats/janet-num) | small C kernels for numeric hot paths: packed buffers, elementwise and reduction ops, matrix routines, special functions | 0.1.2 |
| [janet&#8209;stats](https://github.com/jennystats/janet-stats) | statistics library: d/p/q distributions, hypothesis tests, regression, survival, resampling, Bayes | 0.1.2 |
| [janet&#8209;duckdb](https://github.com/jennystats/janet-duckdb) | in-process DuckDB: prepared statements, full read/bind type matrix, column-major reads, packed f32 feed | 0.1.3 |
| [janet&#8209;xgboost](https://github.com/jennystats/janet-xgboost) | typed xgboost bindings via core ffi, two ingest routes, bitwise conformance with the official wheel | 0.1.2 |
| [janet&#8209;rayforce](https://github.com/jennystats/janet-rayforce) | the Rayforce columnar engine via core ffi: raw plan plane (graphs, ops, filters, groups, sorts), parquet reads | 0.1.0 |

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
- **janet-duckdb** - the engine tier: in-process DuckDB via core ffi.
  Prepared statements with the full read/bind type matrix, handles
  that re-read like values, and a C-to-C packed-f32 feed for the
  numeric kernels.
- **janet-xgboost** - the ML tier: typed xgboost bindings via core
  ffi. Two ingest routes (column arrays or the packed f32 seam),
  bitwise-identical predictions to the official wheel.
- **janet-rayforce** - the engine tier: the Rayforce columnar engine
  via core ffi - a raw plan plane (graphs, ops, filters, groups,
  sorts) with parquet reads, pinned and verified like the rest of
  the stack.

## Contract-driven

Every package ships a measurable API contract (`specs/`):
preconditions, return shapes, error taxonomy, and precision
boundaries - each pinned by regression rows. Verification is
per-package and named: janet-stats verified cell-by-cell against
R 4.5.0; janet-xgboost bitwise-conformed to the official xgboost
wheel; janet-duckdb and janet-rayforce pinned to their upstream
releases with documented behaviour boundaries. No silent wrong
values: where a boundary exists, the contract and the error say so.

## CI

Every package is CI-gated on GitLab: a shared template installs the
declared dependencies with both package managers (jpm and janet-pm)
and runs the package's test suite - per push and weekly.

## License

Each repository carries its own `LICENSE`, which is generally AGPL-3.0.
