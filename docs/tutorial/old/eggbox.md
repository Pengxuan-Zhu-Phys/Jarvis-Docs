# Tutorial: Write Your First Jarvis-HEP Scan Card

This tutorial is for a HEP researcher who wants to write a Jarvis-HEP YAML file from scratch.

We use the simple `EggBox` calculator as a stand-in for a real physics package. The goal is not the EggBox function itself. The goal is to learn the authoring pattern you will reuse for your own model.

## What You Will Learn

By the end of this page, you will know how to write a YAML card that:

- defines a scan
- samples two parameters
- runs one external calculator
- reads one observable from the calculator output
- builds a log-likelihood from that observable

The same pattern works for real HEP tools such as spectrum generators, dark matter codes, or collider recasting pipelines.

## The Black-Box Model

Jarvis-HEP treats your physics code as a black box.

For each sampled point, Jarvis-HEP does the following:

1. writes one input file
2. runs your code
3. reads one output file
4. stores the requested observables
5. evaluates the `LogLikelihood`

For the EggBox example, the external program reads `input.json` and writes `output.json`.

The function is:

```text
z(x, y) = (sin(x) * cos(y) + 2)^5
```

We pretend that an experiment measures:

```text
z = 100 +/- 10
```

So the scan goal is to find parameter points where the predicted `z` value is compatible with that target.

![Eggbox surface](../assets/eggbox_vis.png)

## Before You Write YAML

Think in this order:

1. Which parameters do I scan?
2. Which program computes my observables?
3. Which output values do I want to save?
4. Which likelihood should Jarvis-HEP use?

For this tutorial, the answers are:

- sampled parameters: `x`, `y`
- calculator: `EggBox`
- observable: `z`
- likelihood: `LogGauss(z, 100, 10)`

## Step 1: Create The `Scan` Section

Start with the run name and result directory.

```yaml
Scan:
  name: "EggBox_Random_Tutorial"
  save_dir: "&J/Results"
```

This tells Jarvis-HEP to create:

```text
&J/Results/EggBox_Random_Tutorial/
```

Inside that directory, Jarvis-HEP will create:

- `SAMPLE/`
- `LOG/`
- `DATABASE/`

Reference: [Task YAML Structure](../core/summary.md)

## Step 2: Add The `Sampling` Section

For a first tutorial, use `Random`. It is the simplest sampler to understand.

```yaml
Sampling:
  Method: "Random"
  Variables:
    - name: x
      description: "First scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
    - name: y
      description: "Second scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
  Point number: 2000
  LogLikelihood:
    - name: LogL_Z
      expression: "LogGauss(z, 100, 10)"
```

Important details:

- `Method` chooses the sampler.
- `Variables` defines the sampled parameters.
- `Point number` is the exact control key for `Random`.
- `LogLikelihood` uses the observable `z`, which will be produced later by the calculator.

Reference: [Samplers Overview](../core/samplers.md)

## Step 3: Add `EnvReqs`

Now declare the minimum runtime requirements.

```yaml
EnvReqs:
  OS:
    - name: linux
      version: ">=5.10.0"
    - name: Darwin
      version: ">=10.14"
  Python:
    version: ">=3.10"
    Dependencies: []
```

For a first card, this is usually enough.

Reference: [EnvReqs](../core/environment.md)

## Step 4: Declare The Calculator Workflow

This is the part that most users actually need to learn.

You must tell Jarvis-HEP:

- where the external code lives
- where each sample should run
- how to build the input file
- how to run the program
- how to read the output file

### The Calculator Section

```yaml
Calculators:
  make_paraller: 4
  path: "&J/Workshop/Program"
  Modules:
    - name: EggBox
      required_modules: []
      clone_shadow: true
      path: "&J/Workshop/Program/EggBox/@PackID"
      source: "&J/External/Inertial/EggBox"
      installation:
        - "cp -r ${source}/* ${path}"
      initialization:
        - "cp ${source}/input.json ${path}/input.json"
        - "rm -f ${path}/output.json"
      execution:
        path: "&J/Workshop/Program/EggBox/@PackID"
        commands:
          - "./eggbox.py"
        input:
          - name: EggBoxInput
            path: "&J/Workshop/Program/EggBox/@PackID/input.json"
            type: "Json"
            save: false
            actions:
              - type: "Dump"
                variables:
                  - {name: "xx", expression: "x * Pi"}
                  - {name: "yy", expression: "y * Pi"}
        output:
          - name: EggBoxOutput
            path: "&J/Workshop/Program/EggBox/@PackID/output.json"
            type: "Json"
            save: true
            variables:
              - {name: z}
```

### What Each Part Means

#### `make_paraller`

Number of worker slots for calculator execution. The key spelling is historical and must remain `make_paraller`.

#### `path`

Base directory for per-sample calculator workspaces.

#### `Modules`

List of calculator modules. Here we only have one module, `EggBox`.

#### `clone_shadow: true`

Run each sample in its own shadow directory. This is the safe default for external tools that write files.

#### `path: ".../@PackID"`

`@PackID` is replaced by the current sample work directory id.

#### `source`

Location of the external code template or installed package.

#### `installation`

Commands used to copy or prepare the program directory.

#### `initialization`

Commands run before each sample evaluation.

Here we:

- copy the template `input.json`
- remove any stale `output.json`

#### `execution.commands`

Actual commands used to run the external calculator.

#### `execution.input`

Defines how Jarvis-HEP writes the calculator input file.

In this example:

- the file type is `Json`
- the action type is `Dump`
- `x` and `y` are converted into the file fields `xx` and `yy`
- we multiply them by `Pi` before writing, to match the EggBox code convention

#### `execution.output`

Defines how Jarvis-HEP reads calculator output back into observables.

Here the JSON output contains a key `z`, so we expose it as the Jarvis observable `z`.

Reference: [Calculators](../core/calculator.md)

## Step 5: Put Everything Together

This is the complete tutorial card.

```yaml
Scan:
  name: "EggBox_Random_Tutorial"
  save_dir: "&J/Results"

Sampling:
  Method: "Random"
  Variables:
    - name: x
      description: "First scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
    - name: y
      description: "Second scan coordinate"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 5.0
  Point number: 2000
  LogLikelihood:
    - name: LogL_Z
      expression: "LogGauss(z, 100, 10)"

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
    - name: EggBox
      required_modules: []
      clone_shadow: true
      path: "&J/Workshop/Program/EggBox/@PackID"
      source: "&J/External/Inertial/EggBox"
      installation:
        - "cp -r ${source}/* ${path}"
      initialization:
        - "cp ${source}/input.json ${path}/input.json"
        - "rm -f ${path}/output.json"
      execution:
        path: "&J/Workshop/Program/EggBox/@PackID"
        commands:
          - "./eggbox.py"
        input:
          - name: EggBoxInput
            path: "&J/Workshop/Program/EggBox/@PackID/input.json"
            type: "Json"
            save: false
            actions:
              - type: "Dump"
                variables:
                  - {name: "xx", expression: "x * Pi"}
                  - {name: "yy", expression: "y * Pi"}
        output:
          - name: EggBoxOutput
            path: "&J/Workshop/Program/EggBox/@PackID/output.json"
            type: "Json"
            save: true
            variables:
              - {name: z}
```

## Step 6: Run The Scan

If your project workspace is already prepared, run:

```bash
Jarvis /path/to/your/project/bin/EggBox_Random_Tutorial.yaml
```

If you are working inside the source tree example, you can also compare with the shipped cards under `Jarvis-HEP/bin/EggBox/`.

## Step 7: Inspect The Results

After the run, inspect:

- `Results/EggBox_Random_Tutorial/LOG/`
- `Results/EggBox_Random_Tutorial/SAMPLE/`
- `Results/EggBox_Random_Tutorial/DATABASE/`

The most important products are:

- the main log file in `LOG/`
- the HDF5 database in `DATABASE/`
- any saved calculator output files under `SAMPLE/`

Reference: [IO File Types](../core/io_summary.md)

## How To Adapt This To Your Own Physics Case

Replace the tutorial ingredients one by one.

### Replace The Parameters

Change `x` and `y` to your model parameters:

- `M1`
- `M2`
- `TanBETA`
- couplings
- masses
- nuisance inputs

### Replace The Calculator

Change the `EggBox` module into your real code:

- `FlexibleSUSY`
- `SPheno`
- `micrOMEGAs`
- your own Python wrapper
- your own executable

### Replace The Input Mapping

Instead of dumping `xx` and `yy`, map your physics parameters into:

- JSON fields
- SLHA placeholders
- SLHA blocks and entries

### Replace The Output Mapping

Instead of reading `z`, read your real observables:

- Higgs masses
- relic density
- direct-detection cross sections
- flavor observables
- collider likelihood terms

### Replace The Likelihood

Instead of:

```yaml
expression: "LogGauss(z, 100, 10)"
```

write the likelihood you actually need, for example:

```yaml
expression: "LogGauss(mh1, 125.09, 3.0)"
```

or combine several terms:

```yaml
LogLikelihood:
  - {name: LogL_Higgs, expression: "LogGauss(mh1, 125.09, 3.0)"}
  - {name: LogL_DM, expression: "LogGauss(Omega_h2, 0.120, 0.012)"}
```

## Common Beginner Errors

- forgetting the exact key name `Point number`
- forgetting the exact key name `make_paraller`
- using a likelihood expression that depends on an observable you never read from output
- writing paths without understanding `&J`, `&SRC`, and `@PackID`
- copying a sampler-specific setting from the wrong sampler page

## What To Read Next

- [How To Write A Scan YAML](../core/yaml_overview.md)
- [Calculators](../core/calculator.md)
- [IO File Types](../core/io_summary.md)
- [Samplers Overview](../core/samplers.md)
