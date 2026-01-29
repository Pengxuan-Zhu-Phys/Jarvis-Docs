# Jarvis-HEP's Documentation

Jarvis-HEP (Just a Robust and Versatile Interface Suite for High Energy) is a `Python` project to deploy parameter scan, packages link and data visulization. 

## Getting Start

- [**Installation**](tutorial/Installation.md)

- [**Eggbox tutorial**](tutorial/eggbox.md) — a minimal end-to-end scan example

## Core concepts

- [**YAML configuration**](core/yaml_overview.md) — structure, blocks, and common patterns:
  
    - [**Sampler**](core/samplers.md): defines how parameter points are proposed and explored
    - [**EnvReqs**](core/environment.md): runtime environment and dependency requirements
    - [**Library**](core/library.md): backends and external packages for calculations
    - [**Calculator**](core/calculator_contract.md): input → execution → output contract


- **Concepts & Conventions**: 
    - [**IO Files**](core/io_files.md): standardized input, output, and intermediate data formats
    - [**Symbolic Expressions**](core/symbolic_expressions.md): analytical definitions and symbolic parameter relations
    - [**Utils**](core/utils.md): shared helper functions and common utilities

## Projects

- **Jarvis-HEP**: 
  
  GitHub: [https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP](https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP)

- **JarvisPLOT**:   
  
  [JarvisPLOT Documentation](jarvisplot/jarvisplot.md)

  GitHub: [https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT](https://github.com/Pengxuan-Zhu-Phys/Jarvis-PLOT)