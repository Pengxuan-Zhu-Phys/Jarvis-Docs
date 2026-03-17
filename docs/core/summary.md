# Task YAML Structure

A Jarvis-HEP scan is described by one YAML file. For a normal physics scan, you should think of that file as a contract between five pieces:

1. `Scan`: where results will be written.
2. `Sampling`: which sampler to use, which parameters to scan, and what likelihood to optimize.
3. `Calculators` or `Operas`: how observables are produced.
4. `EnvReqs`: which runtime environment is required.
5. Optional helper sections: `Utils`, `LibDeps`, and `Runtime`.

## Minimum Working Structure

For practical use, a beginner scan card usually needs:

- `Scan`
- `Sampling`
- `EnvReqs`
- At least one non-empty backend:
  - `Calculators.Modules`, or
  - `Operas.Modules`

`Calculators` is still present in current schemas, so Jarvis-HEP normalizes an empty `Calculators` section even for `Operas`-only cards. The runtime requirement is simpler: at least one of `Calculators.Modules` or `Operas.Modules` must be non-empty.

## A Typical Scan Card

```yaml
Scan:
  name: "MyFirstScan"
  save_dir: "&J/Results"
  sample_directory:
    limit: 200
    width: 6
    archive_samples: true

Sampling:
  Method: "Random"
  Variables:
    - name: m0
      description: "Scalar mass parameter"
      distribution:
        type: Flat
        parameters:
          min: 100.0
          max: 3000.0
    - name: m12
      description: "Gaugino mass parameter"
      distribution:
        type: Flat
        parameters:
          min: 100.0
          max: 3000.0
  Point number: 2000
  LogLikelihood:
    - name: LogL_Higgs
      expression: "LogGauss(mh, 125.09, 3.0)"

EnvReqs:
  OS:
    - name: linux
      version: ">=5.10.0"
    - name: Darwin
      version: ">=10.14"
  Python:
    version: ">=3.10"
    Dependencies: []

Calculators:
  make_paraller: 4
  path: "&J/Workshop/Program"
  Modules:
    - name: Spectrum
      required_modules: []
      clone_shadow: true
      path: "&J/Workshop/Program/Spectrum/@PackID"
      source: "&J/External/Program/Spectrum"
      installation:
        - "mkdir -p ${path}"
      initialization:
        - "cp ${source}/input.template ${path}/input.json"
      execution:
        path: "&J/Workshop/Program/Spectrum/@PackID"
        commands:
          - "${source}/run_spectrum.sh ./input.json"
        input:
          - name: SpectrumInput
            path: "&J/Workshop/Program/Spectrum/@PackID/input.json"
            type: "Json"
            save: false
            actions:
              - type: "Dump"
                variables:
                  - {name: "m0"}
                  - {name: "m12"}
        output:
          - name: SpectrumOutput
            path: "&J/Workshop/Program/Spectrum/@PackID/output.json"
            type: "Json"
            save: true
            variables:
              - {name: mh, entry: "Higgs.mass"}
```

## Top-Level Sections

### `Scan`

`Scan` controls where the run lives on disk.

- `name`: result directory name.
- `save_dir`: parent directory of the run.
- `sample_directory`: optional bucket/archiving controls for `SAMPLE/`.

Runtime defaults for `sample_directory` are:

- `limit: 200`
- `width: 6`
- `archive_samples: true`
- `archive_prune: false`

If you omit `sample_directory`, Jarvis-HEP fills these values automatically.

### `Sampling`

`Sampling` is the center of the card.

Always define:

- `Method`
- `Variables`
- `LogLikelihood`

Then add the method-specific control keys for your sampler, for example:

- `Point number` for `Random`
- `Bounds` for many MCMC and nested methods
- `Radius` and `MaxAttempt` for `Bridson`

See [Samplers Overview](samplers.md) and the detailed sampler pages under [Sampler Details](../samplers/index.md).

### `Calculators`

Use `Calculators` when you need to run external programs, scripts, or compiled HEP tools. Typical examples are:

- `FlexibleSUSY`
- `SPheno`
- `micrOMEGAs`
- collider recasting tools
- custom Python/C++ wrappers

The exact key name is `make_paraller`, not `make_parallel`.

Full reference: [Calculators](calculator.md)

### `Operas`

Use `Operas` when your observable pipeline can run inside Python through registered Jarvis-Operas operators. This is the lightest way to prototype a scan because you do not need per-sample shadow directories or file adapters.

Full reference: [Operas](operas.md)

### `EnvReqs`

`EnvReqs` lets you state platform requirements before the scan starts.

Typical beginner usage includes:

- supported operating systems
- minimum Python version
- Python packages
- optional default dependency merge

Full reference: [EnvReqs](environment.md)

### `Utils`

`Utils` defines helper functions that can be reused inside symbolic expressions. The main current feature is `interpolations_1D`, which is useful for exclusion curves, tabulated efficiencies, and detector-response approximations.

Full reference: [Utils](utils.md)

### `LibDeps`

`LibDeps` is an optional setup layer for shared libraries or assets that should be installed once before calculator modules run.

Full reference: [Libraries](library.md)

### `Runtime`

`Runtime` is optional advanced tuning. The most relevant block is `Runtime.Subprocess`, which controls subprocess scheduler behavior such as concurrency and logging policy.

For first scans, you can normally omit it.

## Recommended Authoring Order

Write a new scan YAML in this order:

1. Fill `Scan`.
2. Choose one sampler and complete `Sampling`.
3. Decide whether your physics workflow belongs in `Calculators` or `Operas`.
4. Define the observables that your `LogLikelihood` needs.
5. Add `EnvReqs`.
6. Add `Utils` only if your likelihood or module expressions need interpolation functions.
7. Add `LibDeps` only if your modules depend on shared installed resources.

## Common Mistakes

- Using the wrong key spelling, such as `make_parallel` instead of `make_paraller`.
- Forgetting that key names are case-sensitive, for example `Point number` and `LogLikelihood`.
- Writing a `LogLikelihood` expression that depends on observables your modules never produce.
- Mixing up `&J` and `&SRC`.
- Treating sampler-specific options as universal. Different methods use different control keys.

## Where To Continue

- Start from [YAML Overview](yaml_overview.md) for a full walk-through.
- Read [Samplers Overview](samplers.md) before choosing a method.
- Use [Calculators](calculator.md) or [Operas](operas.md) to build the physics workflow.
