# Coordinates and Expressions

Two small concepts appear everywhere in a Jarvis-PLOT file: the **coordinate dict** (how a
column maps to a visual axis) and the **expression** (the string that computes a value from
columns). Learn them once and they read the same in every layer and transform.

## The coordinate dict

In a layer's `coordinates`, each visual axis (`x`, `y`, `z`, `c`, …) is given a small dict.
In a layer, the only field you need is `expr`:

```yaml
coordinates:
  x: {expr: mass}                  # column `mass` → x axis
  y: {expr: "np.log10(xsec)"}      # an expression → y axis
  z: {expr: LogL}                  # scalar field for image/contour/voronoi methods
  c: {expr: chi2}                  # per-point color for scatter
```

Which keys a method expects:

| Visual key | Meaning | Used by |
|------------|---------|---------|
| `x`, `y` | position | almost every method |
| `z` | scalar field | `pcolormesh`, `contour`, `contourf`, `voronoi`, `tripcolor`, … |
| `c` | per-point color | `scatter` |
| `left`, `right`, `bottom` | ternary composition | `tri*` methods on `axtri` |
| `u`, `v` | vector components | `quiver` |

### Full coordinate dict (transforms)

Inside a **transform** (e.g. `profile`, `make_density_core`), a coordinate carries more
fields, because the transform produces *new* columns on a grid:

```yaml
x:
  expr: "np.log10(mass)"   # how to compute the input value (required)
  name: xx                 # name of the output column the transform writes
  lim: [0.1, 10]           # range used for binning/gridding
  scale: log               # linear | log
```

| Field | Used in layer | Used in transform | Meaning |
|-------|:-------------:|:-----------------:|---------|
| `expr` | ✔ | ✔ | Expression computing the value (required) |
| `name` | — | ✔ | Output column name produced by the transform |
| `lim` | — | ✔ | `[min, max]` range for binning/gridding |
| `scale` | — | ✔ | `linear` or `log` axis treatment |

So a typical pattern is: a `profile`/`density` transform reads raw columns via `expr` and
writes tidy grid columns via `name` (e.g. `xx`, `yy`, `z0`); the layer's `coordinates` then
reference those grid columns by `expr: xx`.

## Expressions

Any `expr`, `filter`, or `add_column.expr` string is evaluated against the current DataFrame.
Column names are variables; standard math and NumPy are available.

```yaml
expr: "mass / 1000"                     # arithmetic
expr: "np.log10(xsec)"                  # NumPy via np.*
expr: "np.sqrt(x**2 + y**2)"
expr: "exp(LogL)"                       # bare math functions also work
filter: "(x > 0) & (y < 100)"           # boolean masks for `filter`
filter: "np.isfinite(z0)"
```

### Available functions

| Group | Names |
|-------|-------|
| Basic | `exp`, `log`, `ln`, `sqrt`, `abs`, `Abs`, `root` |
| Trig | `sin`, `cos`, `tan`, `sec`, `csc`, `cot`, `sinc`, `asin`, `acos`, `atan`, `atan2`, … |
| Hyperbolic | `sinh`, `cosh`, `tanh`, `asinh`, `acosh`, `atanh`, … |
| Reductions | `Min`, `Max` |
| Constants | `Pi`, `E`, `Inf` |
| Statistics | `Gauss(x, mean, sigma)`, `Normal(x, mean, sigma)`, `LogGauss(x, mean, sigma)`, `Heaviside(x)` |
| NumPy | any `np.*` function (e.g. `np.where`, `np.maximum`, `np.clip`) |

For boolean filters use the elementwise operators `&` (and), `|` (or), `~` (not), and wrap
each comparison in parentheses: `(a > 0) & (b < 1)`.

### User-defined helpers

Interpolators declared in the top-level [`Functions`](functions-interpolators.md) block, and
operators registered with Jarvis-Operas, become callable inside expressions by name. For
example, a `Functions` entry named `FASER2` can be used as `FASER2(x)` in any `expr`.
