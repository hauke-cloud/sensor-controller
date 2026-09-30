<!-- llm-readme-management spec=1 commit=fef6d12ecbe8f2509b3a5457b2aa2627a90d19ad template=terraform model=qwen3.8-27b-q4 digest=f68682b55584 generated=2026-09-30T15:32:35Z -->
<a href="https://hauke.cloud" target="_blank"><img src="https://img.shields.io/badge/home-hauke.cloud-brightgreen" alt="hauke.cloud" style="display: block;" /></a>
<a href="https://github.com/hauke-cloud" target="_blank"><img src="https://img.shields.io/badge/github-hauke.cloud-blue" alt="hauke.cloud Github Organisation" style="display: block;" /></a>
<a href="https://github.com/hauke-cloud/llm-readme-management" target="_blank"><img src="https://img.shields.io/badge/template-terraform-orange" alt="Repository type - terraform" style="display: block;" /></a>


# Template Repository


<img src="https://raw.githubusercontent.com/hauke-cloud/.github/main/resources/img/organisation-logo-small.png" alt="hauke.cloud logo" width="109" height="123" align="right">


<llm header hint="Say whether this is a reusable module or a root module that owns real state.">

This repository is a template for hauke-cloud projects, bundling GitHub Actions housekeeping workflows, pre-commit hygiene hooks, and an inactive OpenTofu CI workflow template. It is neither a reusable Terraform module nor a root module that owns state; it contains no `.tf` files. It is intended as a starting point for internal operators bootstrapping a new hauke-cloud project.

</llm>


## :book: Description

<llm description>

This repository is a project scaffold for the `hauke-cloud` organisation, intended as a starting point for new internal projects. As it stands, it is an unmodified copy of the organisation's template repository: the README, the `.repository` metadata file, and the clone URL in the documentation all still reference the generic template rather than a sensor controller. No Terraform modules, no application code, and no Helm charts are present.

The repository provides the organisational infrastructure that a new `hauke-cloud` project would build upon before adding its own configuration and code:

- PR-title validation enforcing semantic types (`fix`, `feat`, `docs`, `ci`, `chore`) with an uppercase subject.
- Stale-issue and stale-PR labelling with automatic closure after 30 idle days.
- Thread locking on closed issues and merged PRs.
- Pre-commit hygiene hooks covering trailing whitespace, large files, merge-conflict markers, private-key and AWS-credential detection, and gitleaks secret scanning.
- An inactive OpenTofu CI workflow template (`terraform-deploy.yml.template`) that would run `tofu fmt`, `tofu validate`, `tofu plan`, `tflint`, and `tofu apply` once activated and paired with actual `.tf` files.
- A `.repository` metadata file consumed by organisation tooling, currently holding placeholder values for the Docker image namespace and Helm chart name.

</llm>


## :clipboard: Requirements

<llm requirements hint="Give the Terraform version from .terraform-version and the provider constraints from versions.tf, plus the credentials the providers need.">

- **Terraform 1.9** — pinned in `.terraform-version`.
- **OpenTofu 1.8.0** — pinned in `.opentofu-version`.
- **`pre-commit`** — mandatory before contributing; install hooks with `pre-commit install` (see `CONTRIBUTING.md`).

No provider constraints are declared (no `versions.tf` or other `.tf` files exist in the repository), and no cloud credentials, cluster access, or additional runtimes are required for anything currently present. The inactive `terraform-deploy.yml.template` workflow references AWS credentials for region `eu-central-1` and `tflint`, but that file is not an active GitHub Actions workflow and would not execute as-is.

</llm>


## 🚀 Getting started

<llm getting_started hint="terraform init, plan and apply, with the backend configuration the repository actually uses. Say plainly if apply touches real infrastructure.">

1. Clone the repository and enter it.

```bash
git clone https://github.com/hauke-cloud/sensor-controller.git
cd sensor-controller
```

2. Install the pre-commit hooks that the project mandates before any contribution.

```bash
pre-commit install
```

3. Run every hook against the existing files to confirm the toolchain is working.

```bash
pre-commit run --all-files
```

This repository contains no Terraform or OpenTofu configuration files (no `*.tf`), so there is no module to initialise, plan, or apply. The `terraform-deploy.yml.template` workflow is inactive—the `.template` extension prevents GitHub Actions from recognising it—and would fail if renamed, because it references a `.tflint.hcl` file and AWS credentials that do not exist in the repository. The `.terraform-version` (1.9) and `.opentofu-version` (1.8.0) pins are informational only.

</llm>


## :airplane: Usage

<llm usage hint="For a reusable module, the central example is a module block with source, version and the required variables filled in from variables.tf. For a root module, show the workflow instead.">

This repository is a starting template for hauke.cloud projects. It contains no Terraform configuration, application code, or deployable artefact. The active tooling consists of pre-commit hygiene hooks and GitHub Actions for PR-title validation, stale-issue labelling, and thread locking.

**Install pre-commit hooks**

Before making any changes, install the git hooks locally:

```bash
pre-commit install
```

**Run checks before committing**

Run all configured hooks (trailing whitespace, large files, merge conflicts, private-key and AWS-credential detection, end-of-file fixes, gitleaks) across the working tree:

```bash
pre-commit run --all-files
```

To update hook revisions to their latest tagged releases:

```bash
pre-commit autoupdate
```

**Open a pull request**

Contributions go through pull requests from a fork; direct pushes to `main` are blocked by the `no-commit-to-branch` hook. PR titles must follow the semantic format enforced by the `pr-title.yml` workflow. The type must be one of `fix`, `feat`, `docs`, `ci`, or `chore`; the subject must start with an uppercase letter. Prefixing the title with `[WIP]` suspends validation for work-in-progress branches.

**Terraform workflow (template only)**

The file `.github/workflows/terraform-deploy.yml.template` defines an OpenTofu CI pipeline (`tofu fmt -check -diff`, `tofu init`, `tofu validate`, `tofu plan -out=tfplan`, `tflint --config .tflint.hcl`, `tofu apply -auto-approve tfplan`) but is inactive because of its `.template` extension. It also references a `.tflint.hcl` file that does not exist in this repository. Until Terraform configuration is added and the template is renamed to `.yml`, no infrastructure is planned or applied.

</llm>


## :wrench: Configuration

<llm configuration hint="A table of the variables in variables.tf: name, type, default, required. Point at variables.tf for the full set and mention outputs.tf if it exists.">

This repository contains no Terraform configuration. There is no `variables.tf`, no `outputs.tf`, and no `*.tf` file of any kind. The `.terraform-version` (1.9) and `.opentofu-version` (1.8.0) pins at the repository root are informational only; no module consumes them.

The only configuration-like artifact is the `.repository` metadata file, which the organisation's tooling reads. All of its fields currently hold placeholder values inherited from the template:

| Name | Default | Description |
|---|---|---|
| `title` | `Template Repository` | Display title for the project |
| `description` | `Template repository for hauke.cloud projects` | Short description |
| `logo` | `""` | Logo path (empty) |
| `docker_image_registry` | `ghrc.io` | Container registry host (note: appears to be a typo of `ghcr.io`) |
| `docker_image_namespace` | `hauke-cloud/example` | Image namespace |
| `docker_image_version` | `latest` | Image tag |
| `helm_repository` | `https://hauke-cloud.github.io/helm-charts` | Helm chart repository URL |
| `helm_chart` | `example` | Helm chart name |

None of these values have been updated for a "sensor controller" project. Any organisation tooling that reads `.repository` will pick up the `example` placeholders until the file is edited.

The inactive workflow template (`.github/workflows/terraform-deploy.yml.template`) references `secrets.AWS_ACCESS_KEY_ID`, `secrets.AWS_SECRET_ACCESS_KEY`, and the AWS region `eu-central-1`, but that workflow is not active (the `.template` extension prevents GitHub Actions from executing it) and no Terraform backend or provider is defined to consume those credentials.

There are no CLI flags, Helm `values.yaml` files, or other runtime configuration inputs in this repository.

</llm>


## :hammer: Development

<llm development hint="Cover terraform fmt, validate, tflint and terraform-docs where the repository configures them.">

There are no tests or build steps in this repository.

**Lint and format.** The mandatory tool is `pre-commit`. Install the hooks after cloning:

```bash
pre-commit install
```

Run all hooks before pushing:

```bash
pre-commit run --all-files
```

The hooks check trailing whitespace, large files, merge-conflict markers, private keys, AWS credentials, end-of-file newlines, and secrets via gitleaks. The `no-commit-to-branch` hook blocks direct commits to `main`; all changes must go through a pull request.

**What CI enforces.** The active `pr-title.yml` workflow validates every PR title: the type must be `fix`, `feat`, `docs`, `ci`, or `chore`, and the subject must start with an uppercase letter. A `[WIP]` prefix suspends the check. Non-conforming titles fail the check until renamed.

The stale-bot closes issues and PRs after 30 idle days; label work-in-progress PRs with `wip` to exempt them.

**Terraform tooling.** The repository pins Terraform 1.9 (`.terraform-version`) and OpenTofu 1.8.0 (`.opentofu-version`) but contains no `.tf` files. The inactive template workflow (`terraform-deploy.yml.template`) expects `tofu fmt -check -diff`, `tofu validate`, and `tflint --config .tflint.hcl`. If you add Terraform configuration you must also create the missing `.tflint.hcl` and rename the template to `terraform-deploy.yml` before those checks activate.

No generated files require regeneration.

</llm>


## 📄 License

This Project is licensed under the GNU General Public License v3.0

- see the [LICENSE](LICENSE) file for details.


## :coffee: Contributing

To become a contributor, please check out the [CONTRIBUTING](CONTRIBUTING.md) file.


## :email: Contact

For any inquiries or support requests, please open an issue in this
repository or contact us at [contact@hauke.cloud](mailto:contact@hauke.cloud).
