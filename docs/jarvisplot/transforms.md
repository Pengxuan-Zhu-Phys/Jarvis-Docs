# Transforms

Transforms are the core data pipeline language in JarvisPLOT.
They are executed in listed order.

## Why this design

By chaining small transforms, users can iteratively refine data logic without rewriting plotting code.
It also allows JarvisPLOT to cache expensive intermediate behavior.

## Supported transforms

- `filter`
- `add_column`
- `sortby`
- `profile`

## Example

```yaml
transform:
  - add_column:
      name: LogLike_total
      expr: LogL_01 + LogL_02
  - sortby: LogLike_total
  - filter: "X2tot < 10"
  - profile:
      bin: 150
      coordinates:
        x: {expr: Lambda4, scale: linear, name: xx, lim: [-1.5, 1.5]}
        y: {expr: Lambda2, scale: linear, name: yy, lim: [-1.5, 1.5]}
        z: {expr: X2tot, scale: linear, name: z0}
      objective: min
      grid_points: rect
```

## `filter`

Use for fast row selection via boolean expressions.
JarvisPLOT evaluates expressions in dataframe context.

## `add_column`

Use for derived features (for example combined likelihood terms).
This keeps reusable derived logic in YAML instead of ad-hoc scripts.

## `sortby`

Use before profile-like reductions when order matters.
Commonly paired with objective-based profiling workflows.

## `profile`

Main high-level reduction transform for profile-like maps.
Key parameters:

- `bin`
- `coordinates`
- `objective`
- `grid_points`

In optimized runs, JarvisPLOT may prebuild a preprofiling stage before runtime profiling.
