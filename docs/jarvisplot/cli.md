# CLI Reference

## Command

```bash
jplot <file.yaml> [options]
```

## Positional argument

- `file`: input YAML path

## Options and intent

- `-h, --help`: print command usage
- `-v, --version`: print installed JarvisPLOT version
- `-d, --debug`: show detailed runtime logs (useful for pipeline diagnosis)
- `--parse-data`: parse dataset structure and write mapped fields back to YAML
- `--out YAML_FILE`: write parsed YAML to a new file (with `--parse-data`)
- `--inplace`: overwrite input YAML in place (with `--parse-data`)
- `--print`: disable logo panel (`axlogo`) in output
- `--rebuild-cache`: clear and rebuild `workdir/.cache` before plotting

## Typical usage

```bash
jplot ./bin/GM95excess.yaml
jplot ./bin/SUSYRun2_EWMSSM.yaml --debug
jplot ./bin/EggBox_Dynesty_06.yaml --rebuild-cache
```

## Practical guidance

- Start with plain run.
- Add `--debug` only when you need to inspect pipeline decisions.
- Use `--rebuild-cache` only when you intentionally want to invalidate cached preprocessing.
