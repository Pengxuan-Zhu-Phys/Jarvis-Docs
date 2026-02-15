# Functions and Interpolators

`Functions` lets you define reusable interpolation helpers once and call them in expressions later.

## Why this exists

Without `Functions`, users often duplicate interpolation code in multiple transforms.
Centralizing helpers in YAML improves consistency and avoids copy-paste errors.

## Current common pattern

- `method: interp1d`
- source table from CSV
- options like `kind: linear` and `bounds: clamp`

## Example

```yaml
Functions:
  - name: FASER2
    source:
      type: csv
      path: ../Workshop/HinoLLP/Fns/FASER2_620.csv
      x: "np.log(y)"
      y: "x"
      sort_by: "y"
      drop_duplicates: "y"
    method: interp1d
    options:
      kind: linear
      bounds: clamp
```

## Runtime behavior

JarvisPLOT loads these function specs and registers callable helpers into expression evaluation context.
After registration, transforms and coordinate expressions can call them directly.
