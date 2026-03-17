# LibDeps

`LibDeps` is an optional setup layer for shared libraries, tool installations, or data assets that should be prepared before the scan workflow uses them.

This section is useful when multiple calculator modules depend on the same installed resource.

## When To Use `LibDeps`

Use `LibDeps` when:

- several modules share one installed package or data directory
- installation is expensive and should not be repeated in every module
- you want to keep "shared setup" separate from "per-sample execution"

If each module is self-contained, you can omit `LibDeps` entirely.

## Section Shape

```yaml
LibDeps:
  path: "&J/Workshop/Library"
  make_paraller: 2
  Modules:
    - name: MySharedTool
      required_modules: []
      installed: false
      installation:
        path: "&J/Workshop/Library/MySharedTool"
        source: "&J/External/Program/MySharedTool"
        commands:
          - "mkdir -p ${path}"
          - "cp -r ${source}/* ${path}"
```

## Top-Level Keys

### `path`

Base directory for shared library installation tasks.

### `make_paraller`

Worker count for library setup tasks.

The exact key spelling is `make_paraller`.

### `Modules`

List of shared dependency definitions.

## Module Keys

### `name`

Unique module name.

### `required_modules`

List of upstream library modules that must finish first.

### `installed`

Boolean hint describing whether the dependency is already installed.

For authored YAML, the safest value is usually:

```yaml
installed: false
```

Jarvis-HEP maintains installation state through generated config files in the library workspace.

### `installation`

Object describing how to install the shared dependency.

Supported keys:

- `path`: installation target directory
- `source`: source directory or package location
- `commands`: shell commands to run

Commands support placeholder substitution such as `${path}` and `${source}`.

## Example

```yaml
LibDeps:
  path: "&J/Workshop/Library"
  make_paraller: 1
  Modules:
    - name: HiggsBoundsTables
      required_modules: []
      installed: false
      installation:
        path: "&J/Workshop/Library/HiggsBoundsTables"
        source: "&J/External/Data/HiggsBoundsTables"
        commands:
          - "mkdir -p ${path}"
          - "cp -r ${source}/* ${path}"
```

## Practical Advice

- Keep `LibDeps` for shared setup only.
- Put per-sample files in `Calculators`, not `LibDeps`.
- Start with `installed: false` in new cards.
- Prefer deterministic copy/build commands so the installation can be reproduced later.
