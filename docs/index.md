#![Jarvis-HEP](assets/title_docs.png)

**Still in construction, coming soon**

Jarvis-HEP (Just a Robust and Versatile Interface Suite for High Energy) is a `Python` project to deploy parameter scan, packages link and data visulization. 

## Quick Start

- [**Installation**](tutorial/Installation.md)

- [**Eggbox tutorial**](tutorial/eggbox.md) — a minimal end-to-end scan example

## Core Concepts

- [**YAML configuration**](core/yaml_overview.md) — structure, blocks, and common patterns:
  
    - [**Sampler**](core/samplers.md): defines how parameter points are proposed and explored
    - [**EnvReqs**](core/environment.md): runtime environment and dependency requirements
    - [**Library**](core/library.md): backends and external packages for calculations
    - [**Calculator**](core/calculator_contract.md): input → execution → output contract


- **Concepts & Conventions**: 
    - [**IO Files**](core/io_files.md): standardized input, output, and intermediate data formats
    - [**Symbolic Expressions**](core/symbolic_expressions.md): analytical definitions and symbolic parameter relations
    - [**Utils**](core/utils.md): shared helper functions and common utilities

## [**High Energy Physics Package Wikis**](package/summary.md)
how external HEP codes are interfaced and configured within Jarvis-HEP

## [**Data Visualisation**](jarvisplot/jarvisplot.md)
visualisation logic built on top of Jarvis-HEP scan outputs

## [**Case Studies**](examples/examples.md)
published projects using Jarvis-HEP, with YAML configurations and notes on Jarvis-HEP’s role

## Projects

- **Jarvis-HEP** — main framework repository  
  
    - GitHub: [https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP](https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP)  
    - Download the latest version: [https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP/releases](https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP/releases)

- **JarvisPLOT** — companion data visualisation framework  
  
    - GitHub: [https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT](https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT)  
  

    - Installation via `PyPI` (Recommanded): 
      ``` 
      pip install jarvisplot
      ```

      
     - Or download the latest version in GitHub: [https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT/releases](https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT/releases)


## Citing Jarvis-HEP Tools

## Note 
Jarvis-HEP is a **User demand driven** project. 
If you encounter any bugs or unexpected behaviour and/or any functional requests, you are encouraged to report a bug and/or to write down your needs in our [Github Issues pages](https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP/issues).

## License 
All Jarvis-HEP Tools (including Jarvis-HEP and Jarvis-PLOT) are released under the [MIT](https://choosealicense.com/licenses/mit/) license. The generated output code of Jarvis-HEP can be freely used according to the MIT license, but as they rely on other (PyPI) released libraries also their License has to be taken into account.
