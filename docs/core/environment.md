# EnvReqs

`EnvReqs` describes what environment a scan expects before it starts. Jarvis-HEP checks these requirements at load time.

## Section Shape

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
  Check_default_dependencies:
    required: true
    default_yaml_path: "&SRC/card/environment_default.yaml"
  CERN_ROOT:
    required: false
    version: ">=6.28"
    get_path_command: "root-config --prefix"
    Dependencies: []
```

## Top-Level Keys

### `OS`

List of supported operating systems.

Each item contains:

- `name`: operating system name such as `linux` or `Darwin`
- `version`: version condition, usually written as `">=..."`

Example:

```yaml
OS:
  - name: linux
    version: ">=5.10.0"
  - name: Darwin
    version: ">=10.14"
```

Jarvis-HEP compares the current machine against these rules and stops if none match.

### `Python`

Python runtime requirement.

Supported keys:

- `version`: minimum Python version, for example `">=3.10"`
- `Dependencies`: Python package requirements

Each dependency item contains:

- `name`: package name as seen by `importlib.metadata`
- `required`: `true` or `false`
- `version`: minimum version, written as `">=..."`

Example:

```yaml
Python:
  version: ">=3.10"
  Dependencies:
    - name: numpy
      required: true
      version: ">=1.24"
    - name: scipy
      required: true
      version: ">=1.10"
```

If a required package is missing or too old, Jarvis-HEP stops and prints a `pip install -U ...` command.

### `Check_default_dependencies`

Optional pre-merge of a shared environment card.

Supported keys:

- `required`: boolean
- `default_yaml_path`: path to the default environment YAML

If `required: true`, Jarvis-HEP loads the YAML at `default_yaml_path` and merges its `EnvReqs` content before the final validation step.

Example:

```yaml
Check_default_dependencies:
  required: true
  default_yaml_path: "&SRC/card/environment_default.yaml"
```

### `CERN_ROOT`

Optional CERN ROOT requirement block.

Supported keys:

- `required`: boolean
- `version`: minimum ROOT version
- `get_path_command`: shell command used to discover the ROOT prefix
- `Dependencies`: extra ROOT-related checks

Each `Dependencies` item contains:

- `name`
- `required`
- `check_command`
- `expected_output`

Example:

```yaml
CERN_ROOT:
  required: true
  version: ">=6.28"
  get_path_command: "root-config --prefix"
  Dependencies:
    - name: root-config
      required: true
      check_command: "which root-config >/dev/null && echo found || echo missing"
      expected_output: "found"
```

## Practical Recommendations

For most users, this is enough:

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

Then add `Check_default_dependencies` if your group maintains a shared default environment card.

Only add `CERN_ROOT` if the scan really depends on ROOT-based tooling.

## Example

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
      - name: scipy
        required: true
        version: ">=1.10"
      - name: xslha
        required: true
        version: ">=0.2"
  Check_default_dependencies:
    required: true
    default_yaml_path: "&SRC/card/environment_default.yaml"
```
