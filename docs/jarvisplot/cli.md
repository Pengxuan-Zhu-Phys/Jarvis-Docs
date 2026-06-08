# CLI Reference

## Command

```bash
jplot <file.yaml> [options]
```

`<file.yaml>` is the plotting config file. In normal use this is the only argument you need.

## Options

| Option | Purpose |
|--------|---------|
| `-v`, `--version` | Print the installed JarvisPLOT version |
| `-d`, `--debug` | Run in debug mode with detailed pipeline logs |
| `--parse-data` | Inspect the datasets (CSV/HDF5), sanitize column names, and write the discovered `Variables`/`ColumnMap` back into the YAML |
| `--out PATH` | Where to write the parsed YAML (used with `--parse-data`) |
| `--inplace` | Rewrite the input YAML in place (used with `--parse-data`) |
| `--print` | Disable the Jarvis logo panel (`axlogo`) in the output figure |
| `--rebuild-cache` | Ignore and rebuild `<workdir>/.cache` before plotting |

## Typical usage

```bash
# Normal run
jplot ./bin/GM95excess.yaml

# Inspect what went wrong in a pipeline
jplot ./bin/SUSYRun2_EWMSSM.yaml --debug

# Force a clean redraw after the source data changed
jplot ./bin/EggBox_Dynesty_06.yaml --rebuild-cache

# Discover HDF5 column names and write them back into the YAML
jplot ./bin/SUSYRun2_EWMSSM.yaml --parse-data --out parsed.yaml
```

## Guidance

- Start with a plain run; only add flags when you need them.
- Use `--debug` when a transform or layer does not behave as expected.
- Use `--rebuild-cache` only when you intentionally want to invalidate cached
  preprocessing (for example, the source file was regenerated). See
  [Cache](profiling-preprofiling-cache.md) for what is cached and when it is reused.
