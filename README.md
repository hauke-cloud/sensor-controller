<!-- llm-readme-management spec=1 commit=fef6d12ecbe8f2509b3a5457b2aa2627a90d19ad template=terraform model=qwen3.8-27b-q4 digest=f68682b55584 generated=2026-10-12T11:57:46Z -->
<a href="https://hauke.cloud" target="_blank"><img src="https://img.shields.io/badge/home-hauke.cloud-brightgreen" alt="hauke.cloud" style="display: block;" /></a>
<a href="https://github.com/hauke-cloud" target="_blank"><img src="https://img.shields.io/badge/github-hauke.cloud-blue" alt="hauke.cloud Github Organisation" style="display: block;" /></a>
<a href="https://github.com/hauke-cloud/llm-readme-management" target="_blank"><img src="https://img.shields.io/badge/template-terraform-orange" alt="Repository type - terraform" style="display: block;" /></a>


# Template Repository


<img src="https://raw.githubusercontent.com/hauke-cloud/.github/main/resources/img/organisation-logo-small.png" alt="hauke.cloud logo" width="109" height="123" align="right">


<llm header hint="Say whether this is a reusable module or a root module that owns real state.">

This OpenTofu template is the starting point for a new root-module project in the hauke.cloud organisation. It pins tool versions, defines pre-commit hooks, and ships an inactive CI pipeline targeting AWS. You clone it, add your own Terraform configuration, and replace the placeholder metadata; it owns no state of its own.

</llm>


## :book: Description

<llm description>

This repository is the unmodified hauke.cloud organisation template, cloned as a starting point for a new OpenTofu/Terraform project. Despite the name *sensor-controller*, it contains no Terraform configuration, no application code, and no sensor-related logic. It provides the scaffolding—version pins, CI workflows, and pre-commit hooks—that a developer in the hauke-cloud organisation expects when beginning a new infrastructure project.

Within the wider `hauke-cloud` setup, it follows the same template structure, pre-commit hygiene rules, and CI conventions shared by the organisation's other project repositories.

- Pins OpenTofu 1.8.0 and Terraform 1.9 for local tool version managers.
- Runs three active GitHub Actions workflows: PR-title validation, stale issue/PR auto-close, and thread locking.
- Stores an inactive OpenTofu deploy pipeline (`.template` file) that, once renamed to a real workflow, runs `tofu fmt`, `tofu validate`, `tofu plan`, `tflint`, and `tofu apply` against AWS (`eu-central-1`).
- Enforces pre-commit hooks for secret scanning (gitleaks), AWS credential detection, and whitespace/EOF fixes.
- Declares Docker and Helm publishing coordinates in `.repository` (currently placeholder values).

</llm>


## :clipboard: Requirements

<llm requirements hint="Give the Terraform version from .terraform-version and the provider constraints from versions.tf, plus the credentials the providers need.">

- **pre-commit** — Required by `CONTRIBUTING.md`. Install the git hooks before your first commit:

  ```bash
  pre-commit install
  ```

  Hook revisions are pinned to `pre-commit-hooks` v4.4.0 and `gitleaks` v8.18.0.

- **OpenTofu 1.8.0** — Pinned in `.opentofu-version`.

- **Terraform 1.9** — Pinned in `.terraform-version`.

- **AWS credentials** — `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` (region `eu-central-1`), stored as GitHub secrets. These are needed only if the OpenTofu deploy workflow (`.github/workflows/terraform-deploy.yml.template`) is renamed to an active workflow.

- **jq** and **tflint** — Also required by that CI template once it is activated.

</llm>


## 🚀 Getting started

<llm getting_started hint="terraform init, plan and apply, with the backend configuration the repository actually uses. Say plainly if apply touches real infrastructure.">

1. Clone the repository.

```bash
git clone https://github.com/hauke-cloud/sensor-controller.git
cd sensor-controller
```

2. Install the pre-commit git hooks so they run on every commit.

```bash
pre-commit install
```

3. Run all hooks against every file to confirm the setup works.

```bash
pre-commit run --all-files
```

This repository is an unmodified hauke.cloud organisation template. It contains no Terraform configuration files (no `*.tf`), so there is nothing to `tofu init`, `tofu plan`, or `tofu apply` at this point. The OpenTofu CI pipeline exists only as an inactive template (`.github/workflows/terraform-deploy.yml.template`) and would additionally require AWS credentials (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`) and a `.tflint.hcl` file before it could execute.

Once you add Terraform modules to this directory, the conventional workflow is:

```bash
tofu init
tofu plan -out=tfplan
tofu apply -auto-approve tfplan
```

`tofu apply` provisions real AWS infrastructure in `eu-central-1`; review the plan output before approving.

</llm>


## :airplane: Usage

<llm usage hint="For a reusable module, the central example is a module block with source, version and the required variables filled in from variables.tf. For a root module, show the workflow instead.">

This repository is a starting point for new OpenTofu/Terraform projects in the hauke.cloud organisation. You clone it, replace the placeholder metadata, and add your own Terraform configuration.

**Clone and adapt**

```bash
git clone https://github.com/hauke-cloud/sensor-controller.git
cd sensor-controller
```

Update the `.repository` file (currently carries `example` placeholders for Docker image and Helm chart names) and the `README.md` to reflect your project.

**Pre-commit hooks**

The repository ships with pre-commit hooks for security scanning (gitleaks), AWS credential detection, and whitespace fixes. Install them once after cloning:

```bash
pre-commit install
```

Run all hooks manually against every file in the repository:

```bash
pre-commit run --all-files
```

Update hook revisions when upstream releases new versions:

```bash
pre-commit autoupdate
```

**CI pipeline (inactive)**

The OpenTofu deployment pipeline lives in `.github/workflows/terraform-deploy.yml.template`. Because of the `.template` suffix, GitHub Actions does not pick it up. To activate it, rename the file to `terraform-deploy.yml` and add a `.tflint.hcl` configuration file. Once active, the pipeline runs `tofu init`, `tofu validate`, `tofu fmt -check -diff`, `tflint`, and `tofu plan` on pull requests to `main`, `dev`, and `feature/*` branches, and `tofu apply -auto-approve` on pushes to `main` and `dev`. It requires the GitHub secrets `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` (region `eu-central-1`).

</llm>


## :wrench: Configuration

<llm configuration hint="A table of the variables in variables.tf: name, type, default, required. Point at variables.tf for the full set and mention outputs.tf if it exists.">

This repository contains no Terraform code. There is no `variables.tf`, no `outputs.tf`, no `.tfvars` file, and no `*.tf` files anywhere in the tree. The OpenTofu/Terraform classification comes solely from the version-pinning files and the inactive CI template; `tofu init` or `tofu validate` would operate on an empty directory.

The only configuration inputs that exist are:

| Name | Type | Default | Required | Description |
|---|---|---|---|---|
| `.opentofu-version` | file (plain text) | `1.8.0` | No | Pins the OpenTofu version for tool version managers (e.g. `tfenv`). |
| `.terraform-version` | file (plain text) | `1.9` | No | Pins the Terraform version for tool version managers. |
| `AWS_ACCESS_KEY_ID` | GitHub secret | — | Yes (for the inactive CI template) | Static AWS credential consumed by `aws-actions/configure-aws-credentials@v1`. |
| `AWS_SECRET_ACCESS_KEY` | GitHub secret | — | Yes (for the inactive CI template) | Static AWS credential; region is hardcoded to `eu-central-1` in the template. |
| `GITHUB_TOKEN` | GitHub secret (auto) | provided by GitHub | Yes | Standard token available to all workflows. |

The CI template (`.github/workflows/terraform-deploy.yml.template`) also references a `.tflint.hcl` lint configuration file that is **not** present in the repository; the lint step will fail until that file is added.

The active PR-title workflow (`pr-title.yml`) enforces a conventional-commit format: allowed types are `fix`, `feat`, `docs`, `ci`, `chore`; scope is optional; the subject must match `^[A-Z].+$`; a `[WIP]` prefix opts a PR out of validation.

Because no Terraform module exists, there is no `variables.tf` to point you to for a full variable set. The repository is an unmodified organisation template; any real configuration surface will appear once Terraform code is added.

</llm>


## :hammer: Development

<llm development hint="Cover terraform fmt, validate, tflint and terraform-docs where the repository configures them.">

Before pushing, install the pre-commit hooks so they run on every commit:

```bash
pre-commit install
```

Run them manually against the whole tree at any time:

```bash
pre-commit run --all-files
```

Update hook revisions when upstream releases new versions:

```bash
pre-commit autoupdate
```

The hooks (defined in `.pre-commit-config.yaml`) scan for leaked secrets via gitleaks, detect AWS credential patterns, and fix trailing whitespace and missing final newlines. A commit that trips any of these is rejected locally before it reaches the remote.

**PR title.** The active `pr-title.yml` workflow validates every pull-request title against a conventional-commit pattern. Allowed types are `fix`, `feat`, `docs`, `ci`, and `chore`; the scope is optional; the subject must begin with a capital letter. Prefixing the title with `[WIP]` opts the PR out of the check. A non-conforming title fails the PR immediately.

**OpenTofu pipeline (inactive).** The file `.github/workflows/terraform-deploy.yml.template` contains `tofu fmt -check -diff`, `tofu validate`, and `tflint --config .tflint.hcl` steps, but the `.template` suffix means GitHub Actions does not pick it up. Until an operator renames it to a real workflow and adds the missing `.tflint.hcl`, none of those checks run in CI.

**Tests.** The repository contains no test suite and no test runner. There is nothing to execute locally or in CI.

</llm>


## 📄 License

This Project is licensed under the GNU General Public License v3.0

- see the [LICENSE](LICENSE) file for details.


## :coffee: Contributing

To become a contributor, please check out the [CONTRIBUTING](CONTRIBUTING.md) file.


## :email: Contact

For any inquiries or support requests, please open an issue in this
repository or contact us at [contact@hauke.cloud](mailto:contact@hauke.cloud).
