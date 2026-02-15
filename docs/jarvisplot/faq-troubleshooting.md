# FAQ and Troubleshooting

## Q1. Plotting is still slow on very large tables. Why?

Common reasons:

- cache directory changed or is not reused
- source fingerprint changed so cache was invalidated
- pre-profile transforms changed
- `--rebuild-cache` was enabled

Start by checking `project.workdir/.cache` reuse.

## Q2. I changed profile `bin`. Should preprofiling rerun?

Default behavior: no.

- preprofiling is reused
- runtime profiling reruns with new `bin`

This is intentional for fast iterative redraws.

## Q3. I see triangulation warnings with invalid values.

Typical cause is non-finite `z` (`NaN` or `Inf`) in triangulated coloring.
JarvisPLOT `tripcolor` masks triangles touching invalid `z`, leaving holes.

If needed, add an explicit filter:

```yaml
- filter: "np.isfinite(z0)"
```

## Q4. Relative dataset path is not where I expected.

Resolution rule:

- relative `DataSet.path` is based on `project.workdir`
- if `workdir` is missing, YAML directory is used

## Q5. `combine: separate` does not work.

Current compatibility keyword is:

- `combine: seperate`

## Q6. How do I force full cache refresh?

```bash
jplot your.yaml --rebuild-cache
```
