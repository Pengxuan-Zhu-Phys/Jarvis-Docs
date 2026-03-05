# Samplers Overview

This page lists all samplers currently available in Jarvis-HEP and points to one dedicated page per sampler.

## Sampler List

- Basic samplers
  - [Bridson](../samplers/bridson.md)
  - [Random](../samplers/random.md)
  - [Grid](../samplers/grid.md)
  - [DNN](../samplers/dnn.md)
  - [Diver](../samplers/diver.md)
- Nested samplers
  - [Dynesty](../samplers/dynesty.md)
  - [MultiNest](../samplers/multinest.md)
- MCMC family
  - [MCMC](../samplers/mcmc.md)
  - [ToyMCMC](../samplers/toymcmc.md)
  - [AMMCMC](../samplers/ammcmc.md)
  - [RobustAM](../samplers/robustam.md)
  - [DRAM](../samplers/dram.md)
  - [DEMCMC](../samplers/demcmc.md)
  - [DREAM](../samplers/dream.md)
  - [DREAMLite / DREAM-lite](../samplers/dreamlite.md)
  - [EnsembleMCMC](../samplers/ensemblemcmc.md)
  - [TPMCMC](../samplers/tpmcmc.md)
  - [PTEnsemble](../samplers/ptensemble.md)
  - [SliceMCMC](../samplers/slicemcmc.md)
  - [ESS](../samplers/ess.md)
  - [MALA](../samplers/mala.md)
  - [HMC](../samplers/hmc.md)
  - [NUTS](../samplers/nuts.md)
- Nuisance sub-sampler
  - [Profile1D](../samplers/nuisance-profile1d.md)

## `Sampling.Method` Values

Use one of the following method names in YAML:

- `Bridson`
- `Dynesty`
- `MultiNest`
- `Grid`
- `Random`
- `DNN`
- `Diver`
- `MCMC`
- `ToyMCMC`
- `AMMCMC`
- `RobustAM`
- `DRAM`
- `DEMCMC`
- `DREAM`
- `DREAMLite` or `DREAM-lite`
- `EnsembleMCMC`
- `TPMCMC`
- `PTEnsemble`
- `SliceMCMC`
- `ESS`
- `MALA`
- `HMC`
- `NUTS`
