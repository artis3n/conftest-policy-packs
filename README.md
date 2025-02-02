# Conftest Policy Packs

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/conftest-policy-packs)](https://artifacthub.io/packages/search?repo=conftest-policy-packs)
[![Conftest CI](https://github.com/artis3n/conftest-policy-packs/actions/workflows/ci.yml/badge.svg)](https://github.com/artis3n/conftest-policy-packs/actions/workflows/ci.yml)
![Github Contributors](https://img.shields.io/github/contributors/artis3n/conftest-policy-packs)
![GitHub last commit](https://img.shields.io/github/last-commit/artis3n/conftest-policy-packs)
![GitHub](https://img.shields.io/github/license/artis3n/conftest-policy-packs)

Centralized OPA policy workflow for Conftest-based Compliance-as-Code evaluations.

This is a central repository housing a snapshot of Rally Health's Rego policies for its Compliance-as-Code program.

View this project at it's [GitHub Page](https://rallyhealth.github.io/conftest-policy-packs/).
The policy documentation is available [here](https://rallyhealth.github.io/conftest-policy-packs/docs/policies.html).

This fork's GitHub Page is located [here](https://artis3n.github.io/conftest-policy-packs/) with policy documentation [here](https://artis3n.github.io/conftest-policy-packs/docs/policies.html).

Rally enforces these policies through a homegrown GitHub App running on AWS ECS to evaluate every commit in the organization.
The GitHub App is an internal wrapper around Conftest, primarily handling reporting the violation results back to developer workflows
in GitHub pull requests and to a central dashboard in Datadog.
Violations are reported as non-blocking status checks.
Results are added to developer PRs within 4-10 seconds.
This repository only houses the policies used in this process and demonstrates a CI/CD approach to creating and managing
Rego policies.

Policy messages in each Rego file use markdown syntax as Rally publishes policy messages to pull request status checks.

![policy violation markdown](/docs/images/violation-markdown-example.png)

<small>_(Note the details in that image, such as the policy ID and message, may not line up with the policies currently in this repo.)_</small>

<!-- toc -->
- [Usage](#usage)
  - [Downloading Policies](#downloading-policies)
  - [Policy Data](#policy-data)
  - [Policy Exceptions](#policy-exceptions)
- [Contributing](#contributing)
  - [Contributing A Policy](#contributing-a-policy)
  - [Quick Start](#quick-start)
    - [Run tests](#run-tests)
    - [Convert file into JSON for unit tests](#convert-file-into-json-for-unit-tests)
    - [Debug a policy](#debug-a-policy)
    - [Generate documentation](#generate-documentation)
- [Original Contributors](#original-contributors)
<!-- /toc -->

# Usage

Policy documentation can be found [here](/docs/policies.md).

## Downloading Policies

Follow Conftest's [instructions for sharing policies](https://www.conftest.dev/sharing/).

You can pull policies directly from this repo:

```bash
# Git SSH syntax
conftest pull git::git@github.com:rallyhealth/conftest-policy-packs.git//policies
# Git HTTPS syntax
conftest pull git::https://github.com/rallyhealth/conftest-policy-packs.git//policies
```

![Conftest pull](/docs/images/pulling-policies.png)

See `conftest pull --help` for more instructions on customizing the download, if needed.

These policies will soon be available on [CNCF Artifact Hub](https://artifacthub.io/).

## Policy Data

These policies are provided for general consumption.
Policy contents are written to be general purpose and org-specific values are relegated to the `data/` directory for import via conftest `--data`.
You should pull the policies with `conftest pull` and specify your own data files as appropriate for your organization.
Explanations for each value are provided in the YAML files under `data/`.

## Policy Exceptions

Rally currently handles exceptions in its homegrown GitHub App code wrapping Conftest.
Violations produced by Conftest are filtered out from a JSON mapping of approved exceptions to repos.
Exceptions are supported at a per-file level.

See [here](/exceptions/readme.md) for more information.

# Contributing

Please follow the [contribution instructions](/CONTRIBUTING.md).

Generally:

1. Fork the repository
1. Create your feature branch
1. Commit your changes following [semantic commit syntax](/CONTRIBUTING.md#commits)
1. Push to the branch
1. Open a pull request

## Contributing A Policy

Follow the requisite section in the [contribution instructions](/CONTRIBUTING.md#adding-rego-policies).

## Quick Start

### Run tests

```bash
make test
```

### Convert file into JSON for unit tests

```bash
conftest parse <file>
```

### Debug a policy

1. Use `trace()` in the policy
1. Run conftest with `--trace`
1. Recommended, pipe the output to grep (`| grep Note`) to only view the `trace()` output

Also use the [OPA playground](https://play.openpolicyagent.org/) to troubleshoot Rego code.

### Generate documentation

```bash
make docs
```

# Original Contributors

Rally's compliance-as-code program has seen early success internally thanks to the following individuals who contributed to the effort:

- Ari Kalfus
- Mia Kralowetz
- Nicholas Hung
- Karl Nilsen
- Benjamin Mangold
