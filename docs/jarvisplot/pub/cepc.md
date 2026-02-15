# Example: CEPC

Source file:

- `JarvisPLOT/bin/cepc.yaml`

## Why this example matters

Unlike profile-map examples, CEPC focuses on distribution plots:

- many CSV background/signal sources
- histogram-centric layers (`method: hist`)
- derived columns via `add_column`

## Run

```bash
jplot ./bin/cepc.yaml
```

## What to observe

- how JarvisPLOT handles many input tables in one file
- how figure structure remains clear even for multi-source hist workflows
