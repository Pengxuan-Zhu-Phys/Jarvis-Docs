# How To Write A Scan YAML

This page is written for a physicist who wants to go from "I know my model and my observables" to "I can write a Jarvis-HEP scan card myself".

## Mental Model

A Jarvis-HEP run does three things:

1. Draw a parameter point.
2. Evaluate one workflow that turns that point into observables.
3. Compute a `LogLikelihood` from those observables.

If you can describe those three steps in YAML, you can run a scan.

## Step 1: Define The Run Location

Start with `Scan`.

```yaml
Scan:
  name: "MSSM_Run"
  save_dir: "&J/Results"
```

This creates a result directory like:

```text
&J/Results/MSSM_Run/
```

Inside it, Jarvis-HEP creates:

- `SAMPLE/`: per-sample working files
- `LOG/`: run logs
- `DATABASE/`: HDF5 output and converted CSV/schema products

If you want explicit sample bucket control, add:

```yaml
Scan:
  name: "MSSM_Run"
  save_dir: "&J/Results"
  sample_directory:
    limit: 200
    width: 6
    archive_samples: true
```

## Step 2: Choose A Sampler

Now define `Sampling.Method`.

A practical beginner rule is:

- Use `Random` if you want a simple, robust first scan.
- Use `Bridson` if you want broad space-filling exploration.
- Use `Dynesty` or `MultiNest` if you need nested sampling.
- Use one of the MCMC-family samplers when you already know why chain-based exploration is the right tool.

Example:

```yaml
Sampling:
  Method: "Random"
```

## Step 3: Define The Scan Variables

Every sampled parameter goes into `Sampling.Variables`.

```yaml
Sampling:
  Method: "Random"
  Variables:
    - name: M1
      description: "Bino soft mass"
      distribution:
        type: Flat
        parameters:
          min: -3000.0
          max: 3000.0

    - name: TanBETA
      description: "tan(beta)"
      distribution:
        type: Flat
        parameters:
          min: 1.0
          max: 60.0
```

### Supported Distribution Patterns

Common patterns are:

- `Flat` with `min` and `max`
- `Log` with `min` and `max`
- `Normal` with `mean` and `stddev`
- sampler-dependent extensions described on the individual sampler pages

The safest beginner choice is `Flat`.

## Step 4: Add Sampler-Specific Controls

Different samplers use different control keys in the same `Sampling` block.

For `Random`, the main control is:

```yaml
Sampling:
  Point number: 5000
```

For `Bridson`, you would instead set keys such as `Radius` and `MaxAttempt`. For nested or MCMC samplers, you will usually define a `Bounds` block. Do not guess these names; use the sampler-specific reference page.

## Step 5: Write The Likelihood Objective

`Sampling.LogLikelihood` tells Jarvis-HEP what to optimize or record as the scan objective.

```yaml
Sampling:
  LogLikelihood:
    - name: LogL_Higgs
      expression: "LogGauss(mh, 125.09, 3.0)"
    - name: LogL_DM
      expression: "LogGauss(Omega_h2, 0.120, 0.012)"
```

The total log-likelihood is the sum of the listed expressions.

Important rule: every symbol in these expressions must come from one of three places:

- a sampled parameter name
- an observable produced by `Calculators` or `Operas`
- a built-in constant or function, such as `Pi`, `sqrt`, or `LogGauss`

Reference: [Symbolic Expressions](symbol.md)

## Step 6: Choose How Observables Are Produced

At this point you decide between two backends.

### Option A: `Calculators`

Use `Calculators` when your workflow depends on files and external executables.

Typical pattern:

1. Copy or prepare an input file.
2. Replace placeholders or dump JSON input values.
3. Run the external code.
4. Read the output file and map results into Jarvis observable names.

Reference: [Calculators](calculator.md)

### Option B: `Operas`

Use `Operas` when the computation already exists as a Python operator registered in Jarvis-Operas.

Typical pattern:

1. Map sampled parameters into operator inputs.
2. Call the operator directly in Python.
3. Map the operator return payload back to Jarvis observables.

Reference: [Operas](operas.md)

## Step 7: Add Environment Requirements

Most cards should declare a minimum platform contract.

```yaml
EnvReqs:
  OS:
    - name: linux
      version: ">=5.10.0"
    - name: Darwin
      version: ">=10.14"
  Python:
    version: ">=3.10"
    Dependencies:
      - name: numpy
        required: true
        version: ">=1.24"
```

If you want Jarvis-HEP to merge a shared dependency card before validation, add:

```yaml
EnvReqs:
  Check_default_dependences:
    required: true
    default_yaml_path: "&SRC/card/environment_default.yaml"
```

Reference: [EnvReqs](environment.md)

## Step 8: Add Optional Helper Functions

If your likelihood or module expressions need an interpolation function, add it under `Utils`.

```yaml
Utils:
  interpolations_1D:
    - name: XenonSD2019
      file: "&J/External/Data/xenon_sd_2019.csv"
      logX: false
      logY: true
      kind: cubic
```

Then you can use `XenonSD2019(mN1)` inside later expressions.

Reference: [Utils](utils.md)

## A Minimal Complete Example With `Operas`

This is the shortest full scan card pattern to learn from.

```yaml
Scan:
  name: "EggBox_Quickstart"
  save_dir: "&J/Results"

Sampling:
  Method: "MultiNest"
  Variables:
    - name: xx
      description: "First scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
    - name: yy
      description: "Second scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
  Bounds:
    nlive: 48
    rseed: 21
    run_nested:
      maxcall: 120
      print_progress: false
  LogLikelihood:
    - name: LogL_Z
      expression: "LogGauss(z, 100, 10)"

EnvReqs:
  OS:
    - name: linux
      version: ">=3.10.0"
    - name: Darwin
      version: ">=10.14"
  Python:
    version: ">=3.10"
    Dependencies: []

Operas:
  make_paraller: 4
  Modules:
    - name: EggBox
      operator: "helper.eggbox2d"
      call_mode: call
      required_modules: []
      input:
        - {name: x, expression: "xx * Pi"}
        - {name: y, expression: "yy * Pi"}
      output:
        - {name: z, entry: z}
```

## Authoring Checklist

Before you run a new YAML, check these points:

- `Scan.name` and `Scan.save_dir` are set.
- `Sampling.Method` is correct.
- every variable has a clear name, description, and valid distribution.
- sampler-specific control keys match the selected method.
- `LogLikelihood` depends only on available symbols.
- one backend is active: `Calculators` or `Operas`.
- every input/output path uses the correct root marker.
- `EnvReqs` matches the machine where you will run the scan.

## Debugging Checklist

If Jarvis-HEP rejects your YAML or the scan produces empty output, check these first:

- spelling and capitalization of keys
- missing observables in `LogLikelihood`
- unsupported sampler keys copied from another method
- wrong path roots (`&J` versus `&SRC`)
- missing external program output files
- `Operas` operators or aliases not installed in the runtime environment

## Next Pages

- [Calculators](calculator.md)
- [Operas](operas.md)
- [EnvReqs](environment.md)
- [IO File Types](io_summary.md)
- [Sampler Details](../samplers/index.md)
