# LogLikelihood

`LogLikelihood` defines the objective terms evaluated for each sampled point.

It appears under the `Sampling` block and is shared by samplers that need a likelihood or score value.

## Minimal form

```yaml
Sampling:
  LogLikelihood:
    - name: L_total
      expression: "-0.5 * chi2"
```

Each entry contains:

- `name`: stable name for the likelihood term.
- `expression`: symbolic expression evaluated after scan variables and workflow outputs are available.

## Multiple terms

Use multiple entries when you want to keep contributions inspectable:

```yaml
Sampling:
  LogLikelihood:
    - name: L_higgs
      expression: "-0.5 * chi2_higgs"
    - name: L_flavor
      expression: "-0.5 * chi2_flavor"
    - name: L_total
      expression: "L_higgs + L_flavor"
```

Keep the final combined term explicitly named when downstream analysis expects a single likelihood column.

## Expression inputs

Likelihood expressions can use:

- scan variables from `Sampling.Variables`
- calculator or opera outputs mapped into the sample record
- derived symbolic quantities documented in [Symbolic Expressions](symbol.md)

## Related pages

- [Task YAML Structure](yaml_overview.md)
- [Sampler Summary](samplers.md)
- [Scan Parameters](variables.md)
- [I/O formats](io_summary.md)

