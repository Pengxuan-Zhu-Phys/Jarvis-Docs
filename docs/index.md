#![Jarvis-HEP](assets/title_docs.png)
<p align="center">
<img alt="Static Badge" src="https://img.shields.io/badge/arXiv-2603.12345-yellow?style=plastic&logo=arxiv&logoColor=red">
<img alt="GitHub Actions Workflow Status" src="https://img.shields.io/github/actions/workflow/status/Pengxuan-Zhu-Phys/Jarvis-HEP/ci.yml?style=plastic&logo=github">
<img alt="PyPI - Downloads" src="https://img.shields.io/pypi/dd/Jarvis-HEP?style=plastic&logo=pypi">
<img alt="PyPI - License" src="https://img.shields.io/pypi/l/Jarvis-HEP?style=plastic&logo=pypi">
<img alt="GitHub Release" src="https://img.shields.io/github/v/release/Pengxuan-Zhu-Phys/Jarvis-HEP?style=plastic&logo=github">
</p>

Jarvis-HEP is a Python framework for parameter scanning, workflow orchestration, and structured data output in high-energy physics.

This documentation is organized around one main goal:

> help a physicist write and run a scan YAML quickly

## Quick Start

1. [Installation](tutorial/Installation.md)
2. [From 0 to 1: commen workflow in Jarvis-HEP](tutorial/Common_workflow.md)
3. [Command Line Tools](tutorial/cli.md)
4. [Write Your First Scan Card](tutorial/eggbox.md)
5. [Task YAML Structure](core/summary.md)
6. [How To Write A Scan YAML](core/yaml_overview.md)

## Install From PyPI

Minimum installation:

```bash
python3 -m pip install -U Jarvis-HEP
```

Full ecosystem installation:

```bash
python3 -m pip install -U Jarvis-HEP Jarvis-Operas jarvisplot
```

## Core Pages

- [Task YAML Structure](core/summary.md)
- [How To Write A Scan YAML](core/yaml_overview.md)
- [Samplers Overview](core/samplers.md)
- [Command Line Tools](core/cli.md)
- [Calculators](core/calculator.md)
- [Operas](core/operas.md)
- [EnvReqs](core/environment.md)
- [Symbolic Expressions](core/symbol.md)
- [IO File Types](core/io_summary.md)

## Optional Related Docs

- [Jarvis-Operas User Guide](package/jarvis-operas.md)
- [Jarvis-PLOT CLI](jarvisplot/cli.md)
- [Published Works](examples/examples.md)

## Project Links

- Jarvis-HEP: [GitHub](https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP)
- Jarvis-Operas: [GitHub](https://github.com/Pengxuan-Zhu-Phys/Jarvis-Operas)
- Jarvis-PLOT: [GitHub](https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT)

## Feedback

If you encounter bugs or unclear behavior, please open an issue:

- [Jarvis-HEP Issues](https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP/issues)

## License

All Jarvis-HEP tools are released under the [MIT license](https://choosealicense.com/licenses/mit/).
