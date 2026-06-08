#![Jarvis-HEP](assets/title_docs.png)
<p align="center">
<img alt="Static Badge" src="https://img.shields.io/badge/arXiv-2604.25557-yellow?style=plastic&logo=arxiv&logoColor=red">
<img alt="GitHub Actions Workflow Status" src="https://img.shields.io/github/actions/workflow/status/Pengxuan-Zhu-Phys/Jarvis-HEP/ci.yml?style=plastic&logo=github&logoColor=white">
<img alt="GitHub Release" src="https://img.shields.io/github/v/release/Pengxuan-Zhu-Phys/Jarvis-HEP?style=plastic&logo=github&logoColor=white">
<img alt="PyPI - Downloads" src="https://img.shields.io/pypi/dd/Jarvis-HEP?style=plastic&logo=pypi&logoColor=white">
<img alt="PyPI - License" src="https://img.shields.io/pypi/l/Jarvis-HEP?style=plastic&logo=pypi&logoColor=white">
<img alt="PyPI - Version" src="https://img.shields.io/pypi/v/Jarvis-HEP?style=plastic&logo=pypi&logoColor=white">
</p>



📌 
**Goal:** Help a physicist write and run a scan YAML quickly.

# What is Jarvis-HEP?

Jarvis-HEP is a Python framework designed for parameter scanning and workflow orchestration in high-energy physics, providing tools for installation, task YAML structure, samplers, calculators, and advanced orchestration methods. It includes a roadmap for users, related packages for plotting and orchestration, and support for reporting issues. All tools are released under the MIT license. 


# Quick Start

1. **Installation**: [Installation](tutorial/Installation.md)
2. **Overall**: [From 0 to 1: commen workflow in Jarvis-HEP](tutorial/Common_workflow.md)
3. **CLI basics**: [Command Line Tools](tutorial/cli.md)
4. **Eggbox**: a mini black box example [Eggbox: a minimal black box example](tutorial/eggbox.md)

# Core documentation

- **Task YAML Structure**: [Task YAML Structure](core/yaml_overview.md) — field-by-field reference for writing and editing a task card.
- **Samplers**: [Samplers](core/samplers.md) — when to use each sampling method, what inputs it expects, and how it affects scan behavior.
- **Sampler variables schema**: [Scan Parameters (Sampler Variables Schema)](core/variables.md) — the variable-definition reference used by samplers, including supported distributions and required parameters.
- **Calculators**: [Calculators](core/calculator.md) — how Jarvis wraps external programs, prepares inputs, runs commands, and reads outputs.
- **Libraries**: [Library dependencies](core/library.md) — how Jarvis declares, prepares, and tracks external library bundles required before calculators or operas run.
- **Operas**: [Operas](core/operas.md) — in-process workflow tools and advanced orchestration patterns when you want to go beyond the basic external black-box mode.
- **Environment requirements**: [Environment Requirements](core/environment.md) — how Jarvis checks platforms, dependencies, and runtime prerequisites before a scan starts.
- **Symbolic expressions**: [Symbolic Expression](core/symbol.md)  — expressions used in mappings, derived variables, and likelihood definitions.
- **I/O formats**: [IO files](core/io_summary.md) — how Jarvis reads and writes JSON, text, structured files, and variable mappings.

## Optional Related Docs

- [Jarvis-Operas User Guide](package/jarvis-operas.md)
- [Jarvis-PLOT User Guide](jarvisplot/cli.md)
- [Published Research Using Jarvis](examples/examples.md)

## Project Links
<p align="left">
  <a href="https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP">
    <img alt="GitHub Badge" src="https://img.shields.io/badge/Github-Jarvis--HEP-blue?style=plastic&logo=github&logoColor=white">
  </a>
  <a href="https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT">
    <img alt="GitHub Badge" src="https://img.shields.io/badge/Github-Jarvis--PLOT-green?style=plastic&logo=github&logoColor=white">
  </a>
  <a href="https://github.com/Pengxuan-Zhu-Phys/Jarvis-Operas">
    <img alt="GitHub Badge" src="https://img.shields.io/badge/Github-Jarvis--Operas-yellow?style=plastic&logo=github&logoColor=white">
  </a>
</p>

## Feedback

If you encounter bugs or unclear behavior, please open an issue:

- [Jarvis-HEP Issues](https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP/issues)

## License

All Jarvis-HEP tools are released under the [MIT license](https://choosealicense.com/licenses/mit/).
