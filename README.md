# Terraform Lint Action

A GitHub Action that runs [TFLint](https://github.com/terraform-linters/tflint) on your Terraform code and generates detailed summaries with file annotations.

## Features

- 🔍 Runs TFLint on Terraform code
- 📊 Generates detailed GitHub Step Summary
- 🎯 Creates file annotations for errors and warnings
- ⚙️ Supports custom TFLint configuration
- 🔒 Uses pinned SHA versions for security
- 📁 Configurable working directory

## Usage

### Basic Usage

```yaml
name: Terraform Lint
on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683
      
      - name: Run Terraform Lint
        uses: tahadekmak/terraform-lint-action@v1
```

### With Custom Configuration

```yaml
name: Terraform Lint
on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683
      
      - name: Run Terraform Lint
        uses: tahadekmak/terraform-lint-action@v1
        with:
          working-directory: './terraform'
          tflint-config: '.tflint.hcl'
```

### Multiple Directories

```yaml
name: Terraform Lint
on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-24.04
    strategy:
      matrix:
        directory: ['./infrastructure', './modules/vpc', './modules/eks']
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683
      
      - name: Run Terraform Lint on ${{ matrix.directory }}
        uses: tahadekmak/terraform-lint-action@v1
        with:
          working-directory: ${{ matrix.directory }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `working-directory` | Working directory to lint | No | `.` |
| `tflint-config` | Path to TFLint config file | No | `.tflint.hcl` |

## Outputs

This action generates:
- **GitHub Step Summary**: A formatted summary of linting results
- **File Annotations**: Inline annotations on files with issues
- **Exit Code**: Fails the workflow if linting issues are found

## Example TFLint Configuration

Create a `.tflint.hcl` file in your repository:

```hcl
plugin "terraform" {
  enabled = true
  preset  = "recommended"
}

plugin "aws" {
  enabled = true
  version = "0.21.1"
  source  = "github.com/terraform-linters/tflint-ruleset-aws"
}

rule "terraform_naming_convention" {
  enabled = true
}
```

## Requirements

- Terraform code in your repository
- GitHub Actions enabled
- (Optional) `.tflint.hcl` configuration file

## License

MIT

## Author

TahaDekmak
