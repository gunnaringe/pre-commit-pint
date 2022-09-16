[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)

# pre-commit-pint

Pre commit checks for linting with [pint](https://github.com/cloudflare/pint),
which is a brilliant Prometheus rule linter by Cloudflare.

## Config

It will automatically pick up the default pint config.

Severity must be set to 'fatal' or 'bug' for `pint lint` to return a non-zero exit code,
thus failing the check.

#### Example `.pint.hcl`
```hcl
# Allows parsing k8s manifests directly
parser {
  relaxed = [ "(.*)" ]
}

rule {
  # This block will apply to all alerting rules with severity="critical" label set.
  match {
    kind = "alerting"

    label "severity" {
      value = "critical"
    }
  }

  # All severity="critical" alerts must have a runbook link as annotation.
  annotation "runbook" {
    severity = "bug"
    value    = "https://runbook.example.com/.+"
    required = true
  }
}
```
