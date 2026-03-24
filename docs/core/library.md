# Dependent Library Modules

<aside>
📌
<strong>
What is LibDeps?
</strong>

- LibDeps is the <strong>library dependency</strong> block in a Jarvis card. It declares external libraries that must be prepared before the main Jarvis workflow runs.
- These libraries are typically neither calculators nor operas. They provide build-time or run-time support for other components.
</aside>

**The three questions LibDeps answers**

1. Where should dependency libraries live? (a shared root)
2. How should each library be installed or updated? (a reproducible recipe)
3. In what order should libraries be prepared? (explicit dependencies)

---

## 1. Motivation and use cases

In many HEP workflows, the main analysis cannot start unless several external packages have already been compiled and installed in known locations. Traditionally this is handled manually, which is hard to reproduce.

LibDeps moves that setup logic into the Jarvis card itself, making the project more **self-describing**:

- dependency setup becomes part of the project definition
- repeated runs are easier to reproduce
- dependency relationships are explicit and trackable
- installation commands can reuse shared variables (for example `${LibDeps:path}`)

---

## 2. Top-level layout (conceptual model)

LibDeps has two layers:

- **LibDeps (container)**: shared root directory and shared build settings
- **Modules (list)**: each module is one installable dependency bundle

A minimal conceptual structure:

```
LibDeps:
  path: "<shared library root>"
  make_paraller: <optional parallel build value>
  Modules:
    - name: "<library name>"
      required_modules: [ ... ]
      installed: <true/false>
      installation:
        path: "<install location>"
        source: "<source package or upstream input>"
        commands:
          - "<shell command 1>"
          - "<shell command 2>"
```

---

## 3. Field reference (LibDeps level)

### 3.1 `path`

The shared root directory for dependency libraries.

```
path: "&J/deps/library"
```

In this example, all dependency-related files (source tarballs, installation outputs, manifests, etc.) live under:

```
&J/deps/library
```

This value can be referenced inside commands as:

```
${LibDeps:path}
```

### 3.2 `make_paraller`

Default compilation parallelism.

```
make_paraller: 16
```

Typical usage:

```
make -j${LibDeps:make_paraller}
```

> Note: the examples use the key name `make_paraller`. If your code/config already uses this spelling, keep it consistent. If you later standardize it to `make_parallel`, consider updating both parser and docs together and providing a compatibility strategy.
> 

---

## 4. Module layout (Modules level)

Each module typically includes:

- `name`: module identifier
- `required_modules`: explicit dependencies (installation order)
- `installed`: a user-facing declared flag (not the only source of truth)
- `installation`: installation description (path, source, commands)

Module example (abstract template):

```
- name: "<module name>"
  required_modules:
    - "<optional prerequisite module>"
  installed: False
  installation:
    path: "<final install prefix or in-place directory>"
    source: "<tarball path | git repo | upstream input>"
    commands:
      - "<step 1>"
      - "<step 2>"
      - "<step 3>"
```

Notes:

- Use `required_modules` to encode the *installation order* constraints between modules.
- Prefer `${LibDeps:path}`, `${path}`, `${source}` for readability and portability.
- `installation.path` is the canonical “where this module ends up” location (either a prefix for `make install`, or the directory you build in-place).

### 4.1 `name`

The logical name Jarvis uses to track a module and resolve dependencies.

### 4.2 `required_modules`

Declares other library modules that must be prepared first.

- Delphes:

```
required_modules: []
```

- Pythia8 (depends on HepMC):

```
required_modules:
  - "HepMC"
```

### 4.3 `installed`

A declared status flag in the YAML:

```
installed: False
```

In practice, Jarvis typically does not rely only on this flag. It may also compare manifests and effective installation state.

### 4.4 `installation`

The installation section has three parts:

- `path`: where the library is installed (prefix or in-place directory)
- `source`: where the source tarball / upstream package comes from
- `commands`: ordered shell commands used for installation

---

## 5. Variable substitution in commands

Inside `installation.commands`, Jarvis allows placeholders such as:

- `${LibDeps:path}`
- `${LibDeps:make_paraller}`
- `${path}` (the current module’s `installation.path`)
- `${source}` (the current module’s `installation.source`)

Example:

```
installation:
  path: "&J/deps/library/HepMC"
  source: "&J/deps/library/source/HepMC-2.06.09.tar.gz"
```

Command:

```
- "./configure --with-momentum=GEV --with-length=MM --prefix=${path}"
```

This is decoded during config loading into a concrete command that points to the module’s installation path.

Benefits:

- the card stays readable and reusable
- the runtime still gets fully resolved command metadata

---

## 6. Worked examples (Delphes / HepMC / Pythia8)

### 6.0 Installation order (practical guidance)

LibDeps modules *declare* intended prerequisites via `required_modules`. In the current implementation of `LibraryModule`, this field is stored as metadata only. Actual enforcement of dependency ordering, topological sorting, or scheduling (if any) must be handled by higher-level orchestration.

That said, users should still install libraries in a sensible order.

**Order implied by the provided examples on this page**

1. **Delphes** (standalone in the examples)
2. **HepMC** (standalone in the examples)
3. **Pythia8** (declares `required_modules: ["HepMC"]`)

**What is strictly required by the Pythia8 example**

- **HepMC must be installed before Pythia8** for the provided example, because Pythia8 is configured with `--with-hepmc2=${LibDeps:path}/HepMC`.

**Optional project-level convention (recommended variant)**

If your project treats Delphes as part of the baseline environment to be prepared before generators, you can *choose* to encode that as an additional declared prerequisite.

```
- name: "Pythia8"
  required_modules:
    - "HepMC"    # required by the example configure flags
```

### **Example 1: Delphes (detailed walkthrough)**

```
- name: "Delphes"
  required_modules: []
  installed: False
  installation:
    path: "&J/deps/library/Delphes"
    source: "&J/deps/library/source/Delphes-3.5.0.tar.gz"
    commands:
      - "cd ${LibDeps:path}"
      - "rm -rf Delphes*"
      - "cp ${source} ./"
      - "tar -xzf Delphes-3.5.0.tar.gz"
      - "mv ./Delphes-3.5.0 ${path}"
      - "cd ${path}"
      - "source @{ROOT path}/bin/thisroot.sh"
      - "./configure"
      - "make -j${LibDeps:make_paraller}"
```

**Module configuration breakdown**

- `name: "Delphes"` – This is the unique identifier Jarvis uses to track this library module. Other modules can reference it in their `required_modules` list.
- `required_modules: []` – An empty list means Delphes has no dependencies on other library modules. It can be installed first in the sequence.
- `installed: False` – This flag in the YAML indicates the module is not yet installed. Jarvis will check this along with manifest records to determine whether installation is needed.
- `installation.path` – Points to `&J/deps/library/Delphes`, which is where the fully compiled Delphes library will reside after installation.
- `installation.source` – Points to the source tarball at `&J/deps/library/source/Delphes-3.5.0.tar.gz`. This is the upstream package that will be unpacked and built.

**Installation commands (step-by-step explanation)**

1. **Navigate to shared library root**`cd ${LibDeps:path}`This changes the working directory to the shared root directory for all dependency libraries (for example, `&J/deps/library`). All subsequent operations start from this common base.
2. **Clean previous installations**`rm -rf Delphes*`Removes any existing Delphes-related directories or files from the shared root. This ensures a clean slate and prevents conflicts between old and new versions.
3. **Copy source tarball to working directory**`cp ${source} ./`Copies the source tarball (resolved from `installation.source`, which is `&J/deps/library/source/Delphes-3.5.0.tar.gz`) into the current directory (`${LibDeps:path}`). This prepares the tarball for extraction.
4. **Extract the tarball**`tar -xzf Delphes-3.5.0.tar.gz`Unpacks the compressed tarball. The `-x` flag extracts, `-z` handles gzip compression, and `-f` specifies the file. This creates a directory named `Delphes-3.5.0` containing the source code.
5. **Rename to final installation path**`mv ./Delphes-3.5.0 ${path}`Renames (or moves) the extracted directory `Delphes-3.5.0` to the target installation path `${path}` (which resolves to `&J/deps/library/Delphes`). This standardizes the directory name and places it in the expected location.
6. **Enter the installation directory**`cd ${path}`Changes into the Delphes installation directory where the build will take place.
7. **Set up ROOT environment**`source @{ROOT path}/bin/thisroot.sh`Sources the ROOT environment setup script. Delphes depends on ROOT libraries and headers, so this command ensures that ROOT's include paths, library paths, and other environment variables are available during the build. The `@{ROOT path}` is a Jarvis variable that points to the ROOT installation.
8. **Configure the build**`./configure`Runs Delphes's configuration script, which checks system dependencies, detects compiler settings, and prepares the Makefile. Because the ROOT environment is already loaded, the configure script can locate ROOT headers and libraries.
9. **Compile Delphes**`make -j${LibDeps:make_paraller}`Compiles the Delphes source code using `make`. The `-j` flag enables parallel compilation, and `${LibDeps:make_paraller}` resolves to the parallelism value set at the LibDeps level (for example, `16`). This speeds up the build by using multiple CPU cores.

**What happens after these commands**

Once all commands complete successfully:

- Delphes binaries and libraries are available in `&J/deps/library/Delphes`
- Jarvis writes a module-specific YAML config snapshot (for example, `Delphes_config.yaml`) and later compares this saved configuration with the current module definition.
- Jarvis writes a module-specific **installation log file**, streaming stdout/stderr line-by-line into that log
- The `installed` flag (and any saved record comparison) is used to decide how to treat subsequent runs
- Other modules may *declare* Delphes in `required_modules` as a prerequisite, but enforcement of ordering depends on higher-level orchestration

**Why this example is useful**

This detailed breakdown shows:

- how variable substitution (`${LibDeps:path}`, `${path}`, `${source}`) keeps commands flexible and reusable
- how environment setup (sourcing ROOT) integrates with external dependencies
- how a typical autotools-style workflow (unpack → configure → make) is encoded in a declarative LibDeps module
- how parallel builds leverage shared LibDeps settings to speed up compilation

### **Example 2: HepMC (detailed walkthrough)**

```
- name: "HepMC"
  required_modules: []
  installed: False
  installation:
    path: "&J/deps/library/HepMC"
    source: "&J/deps/library/source/HepMC-2.06.09.tar.gz"
    commands:
      - "cd ${LibDeps:path}"
      - "rm -rf HepMC*"
      - "cp ${source} ./"
      - "tar -xzf HepMC-2.06.09.tar.gz"
      - "cd ${LibDeps:path}/HepMC-2.06.09"
      - "./bootstrap"
      - "./configure --with-momentum=GEV --with-length=MM --prefix=${path}"
      - "make -j${LibDeps:make_paraller}"
      - "make install"
```

**Module configuration breakdown**

- `name: "HepMC"` – The module identifier other packages can depend on (for example Pythia8).
- `required_modules: []` – No LibDeps-level prerequisites declared here.
- `installation.path` – The *final install prefix* (`&J/deps/library/HepMC`). This is where headers/libs will be installed after `make install`.
- `installation.source` – The source tarball to build from (`&J/deps/library/source/HepMC-2.06.09.tar.gz`).

**Installation commands (step-by-step explanation)**

1. **Go to the shared root**
    - `cd ${LibDeps:path}`
    - Ensures all modules start from the same base directory.
2. **Remove old HepMC artifacts**
    - `rm -rf HepMC*`
    - Avoids mixing old builds with the new tree.
3. **Stage the tarball**
    - `cp ${source} ./`
    - Copies the source package into the working directory.
4. **Unpack sources**
    - `tar -xzf HepMC-2.06.09.tar.gz`
    - Produces a source directory such as `HepMC-2.06.09`.
5. **Enter the source tree**
    - `cd ${LibDeps:path}/HepMC-2.06.09`
    - This example builds from the unpacked source directory.
6. **Generate the `configure` script and helpers**
    - `./bootstrap`
    - Common in autotools projects when the distributed tarball expects bootstrapping.
7. **Configure with explicit units and installation prefix**
    - `./configure --with-momentum=GEV --with-length=MM --prefix=${path}`
    - `--prefix=${path}` is the key: it tells HepMC where to install.
    - The unit flags set defaults used by downstream code.
8. **Compile with shared parallelism**
    - `make -j${LibDeps:make_paraller}`
    - Uses the LibDeps-level setting for consistent build performance.
9. **Install into the prefix**
    - `make install`
    - Copies headers, libraries, and other artifacts into `${path}`.

**What matters for downstream modules (like Pythia8)**

After install, `${LibDeps:path}/HepMC` is expected to contain standard subdirectories such as:

- `include/`
- `lib/` (or `lib64/`)

So downstream configure flags can point at this prefix (as Pythia8 does with `--with-hepmc2=...`).

### **Example 3: Pythia8 (detailed walkthrough; depends on HepMC)**

```
- name: "Pythia8"
  required_modules:
    - "HepMC"
  installed: False
  installation:
    path: "&J/deps/library/Pythia8"
    source: "&J/deps/library/source/pythia8230.tgz"
    commands:
      - "cd ${LibDeps:path}"
      - "mkdir -p ${LibDeps:path}/Source"
      - "cd ${LibDeps:path}/Source"
      - "rm -rf pythia8230"
      - "tar -zxvf pythia8230.tgz"
      - "cd ${LibDeps:path}/Source/pythia8230"
      - "./configure --with-hepmc2=${LibDeps:path}/HepMC --prefix=${path}"
      - "make -j${LibDeps:make_paraller}"
      - "make install"
```

**Dependency note**

- The provided YAML example declares only `HepMC` in `required_modules`.
- In the current `LibraryModule` implementation, `required_modules` is metadata only. It documents intended prerequisites, but it is not automatically enforced here.

**What is strictly required by the commands**

Pythia8’s configure step explicitly points to the HepMC installation:

```
./configure --with-hepmc2=${LibDeps:path}/HepMC --prefix=${path}
```

So users must ensure HepMC is installed at the expected prefix before running this module.

**Recommended variant (optional)**

If your project wants Delphes prepared before Pythia8 as a project convention, you can declare it as an additional prerequisite:

```
required_modules:
  - "Delphes"  # optional
  - "HepMC"    # required by the example configure flags
```

**Module configuration breakdown**

- `required_modules: ["HepMC"]` – Documents the intended prerequisite for this module (the current `LibraryModule` stores this as metadata; ordering enforcement depends on higher-level orchestration).
- `installation.path` – The final prefix `&J/deps/library/Pythia8`.
- `installation.source` – The Pythia8 tarball `&J/deps/library/source/pythia8230.tgz`.

**Recommended variant (optional) — adding Delphes as a declared prerequisite**

If your project convention is to prepare Delphes before generator tooling, you can extend `required_modules`:

- `required_modules: ["Delphes", "HepMC"]`

This is a documentation-level declaration unless enforced elsewhere.

**Installation commands (step-by-step explanation)**

1. **Go to the shared root**
    - `cd ${LibDeps:path}`
2. **Ensure a source workspace exists**
    - `mkdir -p ${LibDeps:path}/Source`
    - Keeps unpacked source trees separated from final prefixes.
3. **Enter source workspace**
    - `cd ${LibDeps:path}/Source`
4. **Clean any previous unpacked tree**
    - `rm -rf pythia8230`
    - Prevents stale files from affecting the build.
5. **Unpack the tarball**
    - `tar -zxvf pythia8230.tgz`
    - Produces `pythia8230/`.
6. **Enter the source tree**
    - `cd ${LibDeps:path}/Source/pythia8230`
7. **Configure Pythia8 against HepMC and set install prefix**
    - `./configure --with-hepmc2=${LibDeps:path}/HepMC --prefix=${path}`
    - `--with-hepmc2=...` must point to the *installed* HepMC prefix that contains headers/libs.
    - `--prefix=${path}` controls where Pythia8 will be installed.
8. **Compile**
    - `make -j${LibDeps:make_paraller}`
9. **Install**
    - `make install`
    - Places the final artifacts into `${path}` (for example `${LibDeps:path}/Pythia8`).

**Practical check before building Pythia8**

Before running configure, users should verify that HepMC is actually present at the expected path:

- `${LibDeps:path}/HepMC/include/`
- `${LibDeps:path}/HepMC/lib/` (or `lib64/`)

---

## 7. What Jarvis does at runtime (high level)

When Jarvis loads a card containing LibDeps, it does not execute the raw YAML strings directly. It first normalizes and decodes the block:

- resolve shared and per-module paths
- substitute placeholders
- decode each module’s command list into a runnable sequence
- construct one `LibraryModule` object per declared module

**Execution model (current behavior)**

- For a given module, commands are executed **sequentially**, one by one, by looping through `installation["commands"]`.
- Internally, Jarvis uses an async subprocess helper to run each command and **stream stdout/stderr line-by-line** into a module-specific installation log. This is for non-blocking I/O and live log capture. It does **not** imply concurrent installation of multiple commands or multiple modules in this file.

**Saved installation record (current behavior)**

- After installation (or when checking an existing installation), Jarvis writes and reads a **module-specific config snapshot / installation record** (a YAML file like `<module_name>_config.yaml`).
- If a matching installation record is found, Jarvis treats the module as already installed and **prompts the user** to decide whether to reinstall. It does not silently skip rebuilds.

Overall, the LibDeps block provides a reproducible, user-visible installation recipe, while higher-level orchestration decides how (and in what order) modules are scheduled.

---

## 8. Writing tips (best practices)

1. keep dependencies under a shared root (`LibDeps:path`)
2. prefer `${LibDeps:path}`, `${path}`, and `${source}` over repeated hard-coded paths
3. declare inter-library dependencies explicitly with `required_modules`
4. write `commands` in the exact order required by the upstream build system
5. treat LibDeps as part of the project definition, not as an ad hoc local setup script