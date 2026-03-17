# Calculators

Use `Calculators` when Jarvis-HEP has to drive an external executable, compiled HEP package, shell wrapper, or file-based physics toolchain.

## Current Standard

Do not redesign the calculator YAML shape for each new package.

The maintained standard is the current single-mode contract already used by Jarvis-HEP runtime:

1. `installation`
2. `initialization`
3. `execution.input`
4. `execution.commands`
5. `execution.output`

The fixed reusable template in this change cycle is the CSV-first onboarding card:

```yaml
Scan:
  name: "<PACKAGE_NAME>_calculator_validation"
  save_dir: "&J/outputs"

Sampling:
  Method: "CSV"
  CSV:
    path: "&J/data/<package>_points.csv"
    uuid_column: "uuid"
    variables: ["<Param1>", "<Param2>"]
  LogLikelihood:
    - {name: "LogL_placeholder", expression: "0"}

EnvReqs:
  OS:
    - name: linux
      version: ">=5.10.0"
    - name: Darwin
      version: ">=10.14"
  Check_default_dependences:
    required: true
    default_yaml_path: "&SRC/card/environment_default.yaml"

Calculators:
  make_paraller: 4
  path: "&J/calculators/runtime/program"
  Modules:
    - name: "<PACKAGE_NAME>"
      required_modules: ["Parameters"]
      clone_shadow: true
      path: "&J/calculators/runtime/program/<PACKAGE_NAME>/@PackID"
      source: "&J/deps/program/<package>"
      installation:
        - "cp ${source}/<package>.tar.gz ${path}"
        - "cd ${path}"
        - "tar -zxvf <package>.tar.gz"
        - "make -j${Calculators:make_paraller}"
      initialization:
        - "cp ${source}/<input_template>.in ${path}/<run_input>.in"
        - "rm -f ${path}/<run_output>.slha"
      execution:
        path: "&J/calculators/runtime/program/<PACKAGE_NAME>/@PackID"
        commands:
          - "./run <run_input>.in"
        input:
          - name: run_input
            path: "&J/calculators/runtime/program/<PACKAGE_NAME>/@PackID/<run_input>.in"
            type: "SLHA"
            actions:
              - type: "Replace"
                variables:
                  - {name: "<Param1>", placeholder: ">>>PARAM1<<<"}
                  - {name: "<Param2>", placeholder: ">>>PARAM2<<<"}
            save: true
        output:
          - name: run_output
            path: "&J/calculators/runtime/program/<PACKAGE_NAME>/@PackID/<run_output>.slha"
            type: "SLHA"
            save: true
            variables:
              - {name: "<Observable1>", block: MASS, entry: 25}
              - {name: "<Observable2>", block: MASS, entry: 35}
```

Use this card to validate one or a few fixed points before you embed the same `Calculators` block into a larger `Random`, `Bridson`, `Dynesty`, `MultiNest`, or MCMC scan.

## Why This Is The Standard

This template follows the current code path exactly:

- `ConfigLoader.analysis_calculator()` normalizes the calculator section into `installation`, `initialization`, and `execution`
- `CalculatorModule.execute()` runs the phases in fixed order:
  - initialize
  - write input files
  - execute commands
  - read output files
- output observables then feed back into `Sampling.LogLikelihood`

No parallel YAML design is introduced here. This is only the documented standard way to use the existing format.

## Section Breakdown

### `Scan`

Use a dedicated validation scan name and keep outputs under `&J/outputs`.

This gives you:

- one isolated run directory
- one isolated `DATABASE/`
- one isolated `SAMPLE/`
- one isolated runtime image/flowchart

### `Sampling`

For calculator onboarding, prefer:

```yaml
Sampling:
  Method: "CSV"
```

Why:

- fixed points make debugging reproducible
- you can validate installation and I/O before adding sampler complexity
- failures map directly back to one known parameter point

For this onboarding card, keep:

```yaml
required_modules: ["Parameters"]
```

Reason:

- CSV points live under `Sampling.CSV.variables`
- they do not populate `Sampling.Variables`
- in the current runtime, the calculator should depend explicitly on the `Parameters` layer so it lands after layer 1

### `EnvReqs`

Keep the normal environment check path.

The most common choice is:

```yaml
Check_default_dependences:
  required: true
  default_yaml_path: "&SRC/card/environment_default.yaml"
```

### `Calculators.make_paraller`

Keep the exact legacy spelling:

```yaml
make_paraller
```

It controls the worker count exposed to calculator modules and can also be reused inside commands such as:

```yaml
"make -j${Calculators:make_paraller}"
```

### `Calculators.path`

The current standard runtime root is:

```yaml
"&J/calculators/runtime/program"
```

Do not use the old `Workshop/Program` path family for new cards.

### Module Keys

Each calculator module in the maintained standard uses:

- `name`
- `required_modules`
- `clone_shadow`
- `path`
- `source`
- `installation`
- `initialization`
- `execution`

Use `clone_shadow: true` unless you have a strong reason not to. Most external HEP tools write into their working directory and are safest in per-instance shadow directories.

### `installation`

This phase prepares the runtime package directory.

Typical operations:

- copy a tarball
- unpack it
- compile it
- copy helper scripts

The current command normalization also means:

- `${...}` placeholders are resolved before runtime
- unresolved placeholders fail fast
- standalone `cd <path>` updates the inherited `cwd` for later commands in the same list

### `initialization`

This phase resets the per-sample working directory before the actual physics run.

Typical operations:

- copy a template input file
- remove stale outputs
- write an empty JSON seed file

### `execution.input`

This describes how Jarvis writes sample-specific inputs.

Current input file types:

- `SLHA`
- `Json`

For `SLHA`, the current write actions are:

- `Replace`
- `SLHA`
- `File`

For `Json`, the current write action is:

- `Dump`

Common usage:

- placeholder replacement into a template file
- symbolic expressions derived from input parameters
- copying a whole file path produced by an upstream module

Reference: [IO File Types](io_summary.md)

### `execution.commands`

This is the actual external run.

Examples:

- `./run suspect2_lha.in`
- `${source}/models/.../run_model.x --slha-input-file=...`
- `python3 gmcalc_point.py --input ... --output ...`

Current runtime token support:

- `@PackID` in paths
- `@SampleID` in runtime command `cmd` or `cwd`

`@SampleID` is runtime-only. Do not rely on it inside the install-stage commands.

### `execution.output`

This maps tool outputs back into Jarvis observables.

Current output file types:

- `SLHA`
- `xSLHA`
- `Json`
- `File`

This is the point where the external package becomes useful to the rest of the Jarvis workflow. Names declared here are the names used later by:

- `Sampling.LogLikelihood`
- nuisance blocks
- downstream calculator modules
- persisted database rows

## Full Lifecycle

Once the validation card runs, the lifecycle is:

1. Jarvis reads the CSV row and creates one sample.
2. Jarvis allocates a calculator instance path such as `.../susyhit_standard/001`.
3. `installation` prepares that runtime instance:
   - copy source archive
   - unpack
   - build
4. `initialization` resets per-sample input/output state.
5. `execution.input` writes the actual input file for the current sample.
6. `execution.commands` runs the external package.
7. `execution.output` reads the result file and extracts observables.
8. `Sampling.LogLikelihood` consumes those observables.
9. Jarvis writes one database row and archives the sample files.

That is the fixed lifecycle you should preserve for new packages.

## Validated Example

The real validated package in this change cycle is:

- `Workshop/Bridson_Higgsino_LLP_muTB_M2/bin/SUSYHIT_Calculator_Validated_CSV.yaml`

The fixed validation point is:

- `Workshop/Bridson_Higgsino_LLP_muTB_M2/data/susyhit_standard_points.csv`

What was validated:

1. `susyhit.tar.gz` copied into a fresh runtime directory
2. tarball unpacked successfully
3. `make -j4` completed successfully
4. template input `suspect2_lha_llp_temp02.in` copied into `suspect2_lha.in`
5. placeholder replacement wrote `Mu`, `Tb`, `M1`, and `M2`
6. `./run suspect2_lha.in` completed with `rc=0`
7. `susyhit_slha.out` was read back into Jarvis observables
8. `LogL_mass_gap` was evaluated from `mN1` and `mC1`

Observed output row:

```text
Mu=122.867508
Tb=2.66993399
ratio=-2.5278640466666666
M1=1500.0
M2=-3791.79607
mN1=-127.529348
mN2=127.625969
mC1=128.611966
mh1=115.003489
BRN22N1a=0.997330562
LogL_mass_gap=-1.0826179999999965
```

Observed persisted sample files:

- `suspect2_lha.in@SUSYHIT`
- `susyhit_slha.out@SUSYHIT`

## How To Fill This Template For A New Package

1. Copy the standard onboarding card.
2. Point `Sampling.CSV.path` at one or a few known-good test points.
3. Set `source` to the package payload inside `&J/deps/program/...`.
4. Replace the install commands with the real package preparation steps.
5. Replace the initialization commands with the package's real template-reset steps.
6. Map the package input file format under `execution.input`.
7. Map the package output file format under `execution.output`.
8. Keep one simple `Sampling.LogLikelihood` expression that uses at least one mapped output observable.
9. Run the card and confirm:
   - runtime directory contains the built tool
   - sample archive contains saved input/output files
   - database CSV contains mapped observables
   - the likelihood expression uses those observables successfully
10. Only then transplant the same `Calculators` block into the final production scan card.

## Current Non-Standard Or Not Yet Validated

- `modes` is not part of the maintained standard workflow in this document.
- Python ternary expressions such as `a if cond else b` are not the safe default for `Sampling.LogLikelihood`; use SymPy-compatible expressions.
- If you skip the CSV onboarding workflow, you lose the cleanest way to debug unpack/build/input/output problems separately from sampler behavior.
