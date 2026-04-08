# Samplers

🧭
**How to use this page**

- This page is a high-level guide for choosing a **sampler family** and navigating to the right method page.
- For the exact YAML keys/options of a specific method, open that sampler’s dedicated page.
- The shared structure of the `Sampling` block is documented in **Task YAML Structure**.


## Overview

Jarvis-HEP provides samplers for parameter scans, global fits, and profile-likelihood studies. Sampler configuration is defined in the `Sampling` section of your YAML card. This page focuses on choosing a sampler and understanding high-level differences—exact method-specific YAML keys are documented on each sampler's dedicated page.

## Choosing a sampler

Use this as a quick decision guide (goal → recommended starting point):

- **Quick workflow test / plumbing check** → `Random`
- **Broad space-filling scan (coverage first)** → `Bridson` (or `Grid` if you need axis-aligned evaluation)
- **Posterior / Bayesian sampling** → MCMC-family (posterior chains) or `Dynesty` / `MultiNest` (nested sampling)
- **Evidence / model comparison** → `Dynesty` or `MultiNest`
- **Global optimisation / best-fit discovery** → `Diver`
- **ML-guided exploration** → `DNN`
- **Nuisance profiling only (inner optimisation step)** → `Profile1D`

Most first runs should begin with `Random` or `Bridson` to validate the model and parameterisation, then switch to a more specialised sampler once the workflow is stable.

## Sampler categories

Jarvis-HEP groups samplers into:

- **Main samplers**: choose the physics parameter points that drive the scan.
- **Nuisance samplers**: optional inner steps used to optimise/profile nuisance parameters around each main point.

Each group below includes a one-line “When to use” hint.

## Main samplers

### Basic exploratory samplers

When to use: fast, simple coverage to map a parameter space before committing to heavier inference.

- [Random Sampler](../samplers/random.md) 
- [Grid](../samplers/grid.md)
- [Bridson](../samplers/bridson.md)

### ML-guided sampler

When to use: guided exploration (surrogate/ML-assisted) to focus evaluations where they matter.

- [DNN](../samplers/dnn.md)

### Evolutionary sampler

When to use: global optimisation / rugged landscapes where evolutionary search can outperform local proposals.

- [Diver](../samplers/diver.md)

### Nested samplers

When to use: you need evidence (marginal likelihood) and robust handling of multi-modality.

- [Dynesty](../samplers/dynesty.md)
- [MultiNest](../samplers/multinest.md)

### MCMC family

When to use: posterior sampling (uncertainties) when you are willing to run and check convergence diagnostics.

MCMC methods mainly differ in how proposals are generated and how efficiently they explore correlated and/or multi-modal targets.

#### Random-walk / baseline Metropolis

When to use: a simple, robust starting point for low–medium dimensional problems.

- [MCMC](../samplers/mcmc.md) — baseline random-walk Metropolis
- [ToyMCMC](../samplers/toymcmc.md) — lightweight/testing variant

#### Adaptive Metropolis (auto-tunes the proposal)

When to use: strong parameter correlations, or when hand-tuning proposal scales/covariances is painful.

- [AMMCMC](../samplers/ammcmc.md) — adaptive Metropolis
- [RobustAM](../samplers/robustam.md) — robust adaptive Metropolis
- [DRAM](../samplers/dram.md) — delayed rejection + adaptive Metropolis

#### Differential Evolution / population-based

When to use: multi-modal targets or rugged landscapes; more compute, often more robust exploration.

- [DEMCMC](../samplers/demcmc.md) — differential evolution MCMC
- [DREAM](../samplers/dream.md) — differential evolution adaptive MCMC
- [DREAMLite](../samplers/dreamlite.md) — lighter DREAM variant

#### Ensemble / parallel-tempering style

When to use: mode hopping (multi-modality) and/or when you can exploit parallel resources.

- [EnsembleMCMC](../samplers/ensemblemcmc.md) — ensemble sampler
- [PTEnsemble](../samplers/ptensemble.md) — parallel-tempered ensemble
- [PTMCMC](../samplers/tpmcmc.md) — tempering / parallel tempering MCMC

#### Gradient-based (Hamiltonian / Langevin)

When to use: higher-dimensional, smooth continuous targets; typically efficient per effective sample when gradients are available.

- [MALA](../samplers/mala.md) — Metropolis-adjusted Langevin algorithm
- [HMC](../samplers/hmc.md) — Hamiltonian Monte Carlo
- [NUTS](../samplers/nuts.md) — No-U-Turn Sampler (auto-tuned HMC)

#### Other / specialised

When to use: only if you know why you want the specific move strategy.

- [SliceMCMC](../samplers/slicemcmc.md) — slice sampling moves
- [ESS](../samplers/ess.md) — elliptical slice sampling

## Nuisance samplers

When to use: optimize nuisance parameters at each physics point rather than sampling them together with the parameters of interest.

- A **main sampler** chooses the physics parameter points that drive the scan.
- A **nuisance sampler** is an additional inner step run around each main point to optimise/profile nuisance parameters.
- A nuisance sampler does **not** replace the main sampler; it augments the main scan.

### Nuisance profiling

- [Profile1D](../samplers/nuisance-profile1d.md) — walks a single nuisance parameter's 1‑D range (with optional LogLikelihood and PassCondition hooks) to drive profile scans before the main analysis runs.

## Typical outputs and diagnostics

What to inspect after a run (keep it practical; method pages cover details):

- **Random / Bridson / Grid**: coverage, parameter-space spread, missed regions (if any), and basic sanity checks of the likelihood.
- **MCMC-family**: trace plots, acceptance rate, autocorrelation / mixing, and effective sample size (ESS) where available.
- **Nested samplers**: weights / posterior samples, live-point behaviour, stability of evidence-related outputs.
- **Diver / evolutionary**: best-fit improvement over time, population stability/diversity, and repeatability across seeds.
- **Profile1D**: profiled nuisance best-fit values and the resulting log-likelihood behaviour (e.g. smoothness / stability of the profile).

## Common structure across samplers

Every sampler page documents the exact keys for that method, but the shared skeleton typically looks like:

```yaml
Sampling:
  Method: "SamplerName"
  Variables:
    - name: parameter_name
      description: "Parameter description"
      distribution:
        type: Flat
        parameters:
          min: 0.0
          max: 1.0

  LogLikelihood:
    - name: LogL_Total
      expression: "..."
```

Then add the sampler-specific control keys required by the chosen method.

[**Variables** details](../core/variables.md) 

## Important reminder

Sampler control keys are **not interchangeable** across methods.

Even when two samplers use similar concepts (e.g. “bounds”, “step size”, “population”), the exact key names, defaults, and meanings may differ.

Use the dedicated sampler page as the source of truth (for example: `Point number` is specific to `Random`, and `Radius` is specific to `Bridson`).