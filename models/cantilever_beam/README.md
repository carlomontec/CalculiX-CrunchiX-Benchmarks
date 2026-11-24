# Cantilever Beam: Linear Static Benchmark

**Model**: solid steel cantilever, 20 x 20 x 200 mm (L/h = 10)
**Analysis**: linear static (`*STATIC`), fixed at z = 0, uniform pressure
0.2 MPa on one lateral face
**Elements**: `C3D20R`

A small, simple case for quick checks of a build or a solver. `remesh.py`
generates the mesh and sets (`all.msh`, `NFIX.nam`, `NTIP.nam`, `TOP.dlo`)
with CGX at the levels `coarse`, `medium` and `fine`.

```bash
python3 models/cantilever_beam/remesh.py --level medium
python3 runner/run_benchmark.py --case cantilever_beam --level medium --threads 1 2 4
```

## License

By Carlo Monjaraz-Tec. `remesh.py`: GPL-2.0. `beam_static.inp` and this
README: CC BY 4.0. See [`LICENSES/`](../../LICENSES/).
