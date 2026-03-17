# Samplers Overview

Jarvis-HEP provides a broad sampler collection for HEP scans, global fits, and profile-likelihood studies. All sampler-specific configuration still lives inside the same `Sampling` section of your YAML card.

## How To Choose A Sampler

A practical rule for first-time users is:

- Start with `Random` if you want the simplest possible scan.
- Use `Bridson` when you want broad space-filling coverage.
- Use `Dynesty` or `MultiNest` when you need nested sampling.
- Use an MCMC-family method only when you already know the chain behavior you want.
- Use `Diver` when you want an evolutionary search.
- Use `DNN` for machine-learning-guided exploration.
- Use `Profile1D` only for nuisance profiling, not for the main parameter scan.

## Sampler Families

### Basic exploratory samplers

- [Bridson](../samplers/bridson.md)
- [Random](../samplers/random.md)
- [Grid](../samplers/grid.md)

### Machine-learning sampler

- [DNN](../samplers/dnn.md)

### Evolutionary sampler

- [Diver](../samplers/diver.md)

### Nested samplers

- [Dynesty](../samplers/dynesty.md)
- [MultiNest](../samplers/multinest.md)

### MCMC family

- [MCMC](../samplers/mcmc.md)
- [ToyMCMC](../samplers/toymcmc.md)
- [AMMCMC](../samplers/ammcmc.md)
- [RobustAM](../samplers/robustam.md)
- [DRAM](../samplers/dram.md)
- [DEMCMC](../samplers/demcmc.md)
- [DREAM](../samplers/dream.md)
- [DREAMLite](../samplers/dreamlite.md)
- [EnsembleMCMC](../samplers/ensemblemcmc.md)
- [TPMCMC](../samplers/tpmcmc.md)
- [PTEnsemble](../samplers/ptensemble.md)
- [SliceMCMC](../samplers/slicemcmc.md)
- [ESS](../samplers/ess.md)
- [MALA](../samplers/mala.md)
- [HMC](../samplers/hmc.md)
- [NUTS](../samplers/nuts.md)

### Nuisance sampler

- [Profile1D](../samplers/nuisance-profile1d.md)

## What Is Common Across All Samplers

Every sampler page documents the exact keys for that method, but the common structure is always:

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

Then you add the sampler-specific keys required by that method.

## Important Reminder

Do not copy the control keys from one sampler into another. For example:

- `Point number` is meaningful for `Random`
- `Radius` is meaningful for `Bridson`
- `Bounds` is used by many MCMC and nested samplers, but the contents differ by method

The detailed pages under [Sampler Details](../samplers/index.md) are written to be self-contained, so you can configure each method without opening another sampler page.
