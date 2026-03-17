# Installation

This page helps you get Jarvis running in a few minutes.

## What you install

Jarvis is a small ecosystem of Python packages on PyPI:

- `Jarvis-HEP`: core runner + scan orchestration
- `Jarvis-Operas`: in-process operators / advanced orchestration (optional)
- `JarvisPLOT`: plotting utilities (optional)

Most users should install from PyPI (you typically do not need a GitHub checkout).

## Requirements

- Python >= 3.10

Tip: A clean virtual environment is recommended.

## Quick install (most users)

1. Install:

```bash
python3 -m pip install -U Jarvis-HEP
```

1. Check it works:

```bash
Jarvis --version
Jarvis --help
```

- Example output: Jarvis --version (logo + version)
    
    ```bash
    Jarvis --version 
     ███ ███         ██╗ █████╗ ██████╗ ██╗   ██╗██╗███████╗ 
    █████████        ██║██╔══██╗██╔══██╗██║   ██║██║██╔════╝ 
    █══███══█        ██║███████║██████╔╝██║   ██║██║███████╗ 
    ╚╗█████╔╝   ██   ██║██╔══██║██╔══██╗╚██╗ ██╔╝██║╚════██║ 
     ╚█═══█╝    ╚█████╔╝██║  ██║██║  ██║ ╚████╔╝ ██║███████║ 
      █████      ╚════╝ ╚═╝  ╚═╝╚═╝  ╚═╝  ╚═══╝  ╚═╝╚══════╝ 
    ________________________________________________________
    === Jarvis-HEP ===
         Just a Robust and Versatile Interface Suite for HEP  
              Author:   Pengxuan Zhu, Erdong Guo. 
              Version:  1.6.9
    
    Resources:
    	Online docs:	https://pengxuan-zhu-phys.github.io/Jarvis-Docs/
    	Homepage:	https://github.com/Pengxuan-Zhu-Phys/Jarvis-HEP
    
    References:
    	Jarvis-HEP:
    		[1] Core Jarvis-HEP framework paper
    			Title:	Jarvis-HEP: Just a Robust and Versatile Interface Suite for HEP
    			Author:	Erdong Guo, Paul Jackson, Jin-Min Yang, Martin J. White, Pengxuan Zhu
    	Built-in Scanners:
    		[1] MultiNest: multimodal nested sampling
    			Title:	MULTINEST: an efficient and robust Bayesian inference tool for cosmology and particle physics
    			Author:	F. Feroz, M. P. Hobson, M. Bridges
    			arXiv:	0809.3437
    			DOI:	10.1111/j.1365-2966.2009.14548.x
    		[2] Nested sampling: foundational evidence algorithm
    			Title:	Nested Sampling for General Bayesian Computation
    			Author:	John Skilling
    			DOI:	10.1214/06-BA127
    		[3] Dynamic: dynamic nested sampling
    			Title:	dynesty: A Dynamic Nested Sampling Package for Estimating Bayesian Posteriors and Evidences
    			Author:	Joshua S. Speagle
    			arXiv:	1904.02180
    			DOI:	10.1093/mnras/staa278
    		[4] Bridson: Poisson-disk sampling
    			Title:	Fast Poisson Disk Sampling in Arbitrary Dimensions
    			Author:	Robert Bridson
    			DOI:	10.1145/1278780.1278807
    		[5] Diver:
    			Title:	Comparison of statistical sampling methods with ScannerBit, the GAMBIT scanning module
    			Author:	GAMBIT Scanner Workgroup
    			arXiv:	1705.07959
    		[6] MCMC: Foundational Metropolis Monte Carlo algorithm
    			Title:	Equation of State Calculations by Fast Computing Machines
    			Author:	Nicholas Metropolis et al.
    			DOI:	10.1063/1.1699114
    		[7] MCMC: Generalized acceptance rule for MCMC (Metropolis-Hastings)
    			Title:	Monte Carlo Sampling Methods Using Markov Chains and Their Applications
    			Author:	W. K. Hastings
    			DOI:	10.1093/biomet/57.1.97
    ```
    
- Example output: Jarvis -h (full help text)
    
    ```
    Jarvis -h 
    usage: Jarvis [-h] [-d] [-v] [--plot]
                  [--skip-library-installation] [--check-modules]
                  [--skip-draw-flowchart] [--convert] [--monitor]
                  [--mkproject MKPROJECT]
                  [--packproject [PACKPROJECT]]
                  [--profile {share,repro,full}]
                  [--max-concurrency MAX_CONCURRENCY]
                  [--per-task-timeout-sec PER_TASK_TIMEOUT_SEC]
                  [--progress-interval-sec PROGRESS_INTERVAL_SEC]
                  [--log-policy LOG_POLICY]
                  [file]
    
    Jarvis Program Help Center
    
    positional arguments:
      file                  path of the input file, in yaml format
    
    options:
      -h, --help            show this help message and exit
      -d, --debug           Run Jarvis-HEP in debug mode, debug mode is testing the program linking
      -v, --version         Print Jarvis-HEP logo, author information and runtime package version
      --plot                Using the plotting mode
      --skip-library-installation
                            Skipping the library installation
      --check-modules       For the Calculator test
      --skip-draw-flowchart
                            Skipping the flowchart drawing
      --convert             Convert the sample.hdf5 file into csv format
      --monitor             Starts a real-time resource monitoring session for the current Jarvis-HEP job. 
                            	Note:
                            	- This monitor updates resource usage statistics in real-time every few seconds.
                            	- Press 'q' in the monitor window to stop monitoring and exit the session.
      --mkproject MKPROJECT
                            Create a standalone Jarvis project folder in the current directory
      --packproject [PACKPROJECT]
                            Package a standalone Jarvis project directory (default: current directory)
      --profile {share,repro,full}
                            Packaging profile for --packproject: share | repro | full
      --max-concurrency MAX_CONCURRENCY
                            Override Runtime.Subprocess.max_concurrency
      --per-task-timeout-sec PER_TASK_TIMEOUT_SEC
                            Override Runtime.Subprocess.per_task_timeout_sec
      --progress-interval-sec PROGRESS_INTERVAL_SEC
                            Override Runtime.Subprocess.progress_interval_sec
      --log-policy LOG_POLICY
                            Override Runtime.Subprocess.log_policy: logger | file | quiet | tee-limited
    ```
    

## Full install (optional)

If you also want Operas + plotting:

```bash
python3 -m pip install -U Jarvis-HEP Jarvis-Operas JarvisPLOT
```

You should then have:

- `Jarvis`
- `jopera`
- `jplot`

CLI reference: [Command Line Tools](https://www.notion.so/Command-Line-Tools-1a4adeac12864a929506f532e4016e98?pvs=21)

## (Optional) Create a clean environment

Example using `pyenv` + `virtualenv`:

```bash
pyenv install 3.10.12
pyenv virtualenv 3.10.12 jarvis-hep
pyenv activate jarvis-hep
python3 -m pip install -U Jarvis-HEP Jarvis-Operas JarvisPLOT
```

Any environment manager is fine (conda/uv/venv, etc.) as long as it gives you an isolated Python environment.