Mecaga searches double-couple focal mechanisms from P-wave first-motion
polarities. This release integrates scientific contract
**`MECAGA-SCI-ENU-LH-U+ v1.2`** into the calculation and SVG rendering paths and
replaces the historical `delth.f` dependency with a built-in C99 translation.

See `RELEASE_NOTES.md` for the v2.0.0 release scope and compatibility notes.

Core contract:

- right-handed ENU coordinates `[East, North, Up]`;
- lower-hemisphere Lambert equal-area projection, North up and East right;
- `M = s n^T + n s^T` and `A_P = r^T M r`;
- `A_P > 0 → U → compression → T lobe → filled black`;
- `A_P < 0 → D → dilatation → P lobe → open white`;
- `|A_P| <= 1e-12` is nodal;
- reference score `(mismatch + 0.5 * nodal) / known`.

Build:

```sh
./configure
make
make test
```

No Fortran compiler or runtime is required. The `-R` ray-recalculation path uses
the built-in C implementation. See `docs/BUILDING.md` and the normative
specification under `docs/scientific_spec/v1.2/`.

Enumerate every equal-best node on the exact `256 x 256 x 256` GA gene grid with:

```sh
./build/mecaga_grid_ties \
  -Pexamples/v1.2/reverse_20.txt \
  -Omecaga_equal_best_solutions.tsv \
  -Jmecaga_equal_best_summary.json
```

This is exhaustive on the finite gene grid, not on the continuous SDR space.

See `docs/DELTH_C_PORT.md` for the C ray-port validation.
See `docs/OUTPUT_FORMATS.md` for stdout, JSON, TSV, and figure products.

## Scientific-specification compatibility

The executable in this package implements the v1.2 four-column, unit-weight
contract. The mecagri v1.3 handoff has been independently audited and is
included under `docs/scientific_spec/v1.3/` as the accepted cross-project
target. v1.3 changes no ENU, SDR, moment-tensor, polarity, P/T/B, projection,
or rendering rule; it adds optional signed linear observation weights.
Mecaga 2.0.0 must not report v1.3 runtime metadata until that input and
objective are implemented and pass the supplied cross-checks. See
`docs/SCIENTIFIC_SPEC_COMPATIBILITY_v1.3.md`.

## License, attribution, and citation

Mecaga is distributed under the BSD 3-Clause License. The original C
implementation was authored by Yoshihiro Ito at NIED in 2003. The C ray
calculation retains scientific attribution to Akira Hasegawa and the historical
`delth.f` routine; the original Fortran source is not included. See `LICENSE`,
`NOTICE`, `AUTHORS.md`, and `docs/PROVENANCE.md`.

For scientific citation, use `CITATION.cff` and report the software version,
scientific-specification ID/version, input-format ID, objective ID, and random
seed.

## Fixed synthetic event metadata

The four v1.2 regression inputs contain finite longitude, latitude, depth, and magnitude values fixed from RNG seed `20260721`. The 20- and 50-observation variants of each mechanism family share one synthetic event. These coordinates are descriptive test metadata; the normative ray geometry remains the supplied back-azimuth and takeoff angle. See `examples/v1.2/fixed_event_metadata.json`.

## Notebook validation

`notebook/mecaga_ga_nodal_plane_comparison.ipynb` follows the same presentation and independent-Python cross-check style as the companion mecagri hierarchical-grid package. It builds and runs the C GA for all four samples, verifies fixed event metadata and observation-level geometry/amplitudes/classifications, then exhaustively enumerates all 16,777,216 gene-grid nodes. All equal-best nodes are saved to TSV and displayed as SDR and P/T-axis distributions, with an individual-solution browser. An executed notebook, HTML export, and reference TSVs are included.

```sh
conda env create -f environment.yml
conda activate mecaga-notebook
jupyter lab notebook/mecaga_ga_nodal_plane_comparison.ipynb
```


## English-language distribution

The public archive uses English for source comments, runtime messages, documentation, scientific specifications, examples, notebooks, and filenames. `tests/test_english_only.py` enforces this rule. See `docs/ENGLISH_LANGUAGE_POLICY.md`.
