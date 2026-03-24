# Scan Parameters (Sampler Variables Schema)

<aside>
ℹ️

This page defines the schema for scan parameters (input variables) shared by all samplers.

</aside>

### Example (recommended format)

```yaml
Sampling:
  Variables:
    - name: x1
      description: "A flat distributed variable x1"
      distribution:
        type: Flat
        parameters:
          min: 0
          max: 10

    - name: x2
      description: "A log-distributed variable x2"
      distribution:
        type: Log
        parameters:
          min: 0.1
          max: 10

    - name: x3
      description: "A normal-distributed variable x3"
      distribution:
        type: Normal
        parameters:
          mean: 10
          stddev: 5

    - name: x4
      description: "A log-normal variable x4"
      distribution:
        type: Log-Normal
        parameters:
          mean: 10
          stddev: 5

    - name: x5
      description: "A logit-distributed variable x5"
      distribution:
        type: Logit
        parameters:
          location: 10
          scale: 5
```

### Structure (explained)

`Sampling.Variables` is a list. Each list item defines **one** input variable.

#### Fields for each variable

- **`name`** *(string)*
    - A short identifier for the variable.
    - Keep it unique within the scan.
- **`description`** *(string)*
    - A short sentence describing what the variable represents.
- **`distribution`** *(object)*
    - Groups the distribution settings.
    - **`distribution.type`** *(string)*
        - The distribution family to sample from.
    - **`distribution.parameters`** *(object)*
        - The parameter set required by the chosen `type`.

### One-dimensional distributions (definitions)

Below are the **definitions** (and required parameters) for each supported 1D distribution.

- **`Flat` (uniform)**
    - Parameters: `min`, `max`
    - Definition:
        
        $$
        x \sim \mathcal{U}(\mathrm{min},\mathrm{max})
        $$
        
        Meaning: sample uniformly on [`min`, `max`].
        
- **`Log` (log-uniform)**
    - Parameters: `min`, `max` (requires `min > 0`)
    - Definition:
        
        $$
        \ln x \sim \mathcal{U}(\ln(\mathrm{min}),\ln(\mathrm{max}))
        $$
        
        Equivalently:
        
        $$
        x = \exp(u),\quad u \sim \mathcal{U}(\ln(\mathrm{min}),\ln(\mathrm{max}))
        $$
        
- **`Normal` (Gaussian)**
    - Parameters: `mean`, `stddev`
    - Definition:
        
        $$
        x \sim \mathcal{N}(\mathrm{mean},\mathrm{stddev})
        $$
        
        Rule of thumb: approximately 68.3% of the probability mass lies within [`mean` − `stddev`, `mean` + `stddev`]
        
- **`Log-Normal`**
    - Parameters: `mean`, `stddev`
    - Definition:
        
        $$
        \ln(x) \sim \mathcal{N}(\mathrm{mean},\mathrm{stddev})
        $$
        
        Equivalently:
        
        $$
        x = \exp(u),\quad u \sim \mathcal{N}(\mathrm{mean},\mathrm{stddev})
        $$
        
- **`Logit`**
    - Parameters: `location`, `scale`
    - Definition (inverse CDF from a uniform variate):
        
        $$
        u \sim \mathcal{U}(0,1),\quad x = \mathrm{location} + \mathrm{scale}\cdot\ln\left(\frac{u}{1-u}\right)
        $$
        
        Equivalent distribution:
        
        $$
        x \sim \mathrm{Logistic}(\mathrm{location},\mathrm{scale})
        $$
        
- **Mapping support**
    - The following distributions support mapping from a standard random variable $u \in [0,1]$: `Flat`, `Log`, `Normal`, `Log-Normal`, `Logit`.
    - The following distributions do **not** support this mapping in the current implementation: Binomial, Poisson, Beta, Exponential, Gamma.

<aside>
📌

Parameter interpretation follows the Jarvis implementation.

</aside>