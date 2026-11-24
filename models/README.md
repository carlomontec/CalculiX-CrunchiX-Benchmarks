# Benchmark Models

Each folder holds one model: the ccx input deck, a `remesh.py` that builds
the mesh with CalculiX GraphiX (cgx) at a chosen level, and a README. Meshes
are generated, not stored; the node, element and equation counts recorded
with every result show that a run used the same mesh.

| Model | Analysis | Levels | Largest level |
|:---|:---|:---|---:|
| [`compressor_rotor`](compressor_rotor/) | Centrifugal preload + cyclic-symmetry frequencies | coarse, fine | 283,024 equations |
| [`naca_blade`](naca_blade/) | Linear static, twisted and tapered blade | coarse, medium, fine, ultra_fine | 715,968 equations |
| [`cantilever_beam`](cantilever_beam/) | Linear static | coarse, medium, fine | small |

## Requirements

- Python 3.9 or newer.
- CalculiX GraphiX (`cgx` or `cgx_glfw`) on the `PATH` or next to this
  repository, to generate meshes.
- One or more ccx binaries. The runner finds the usual `build_*` folders of a
  [CalculiX-CrunchiX-MultiSolver](https://github.com/carlomontec/CalculiX-CrunchiX-MultiSolver)
  checkout (`--ccx-dir`, default `../CalculiX-CrunchiX-MultiSolver`), or takes
  a binary given with `--bin SOLVER=PATH`.
- The runner reads the JSON result export of the MultiSolver edition
  (`CCX_JSON=1`). Official ccx has no JSON export, so it cannot be benchmarked
  with the runner yet; reading monitor values from the `.dat` file instead is
  planned.

## Running

From the repository root:

```bash
# all solvers found, thread sweep
python3 runner/run_benchmark.py --case naca_blade --level fine --threads 1 2 4 8

# a specific binary
python3 runner/run_benchmark.py --case naca_blade --level fine --threads 6 \
    --solvers MUMPS --bin MUMPS=/path/to/build_mumps/CalculiX
```

The runner prints a summary and writes a JSON file to `runs/` (not tracked).
To publish a run, see "Adding results" in the [top-level README](../README.md).

The runner sets `OMP_NUM_THREADS`, `MKL_NUM_THREADS`,
`CCX_NPROC_EQUATION_SOLVER` and `VECLIB_MAXIMUM_THREADS` to the thread count,
and `CCX_JSON=1` for the JSON result export of the MultiSolver edition.

## Licenses

The compressor rotor geometry and deck come from the CalculiX GraphiX examples
by Klaus Wittig and are GPL-2.0-or-later; see
[compressor_rotor/README.md](compressor_rotor/README.md). All other models are
by Carlo Monjaraz-Tec: scripts GPL-2.0, decks and documentation CC BY 4.0.
