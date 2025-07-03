# playwright-python-framework
playwright-python-framework_readymade to start automation
# onPhase-Automation

## Overview

This project presents a Python-based test automation framework tailored for end-to-end testing of web applications.
Utilizing Playwright and Python 3.14, the framework ensures structured management of tests, fixtures, and helpers.
It employs UV for efficient dependency management.

## Prerequisites

Before you begin, ensure your system meets the following requirements:

- [Python 3.14](https://www.python.org/downloads/) installed
- [UV](https://astral.sh/uv) for dependency management
- Adequate permissions to install packages and run scripts on your system
- Verify the QA Confluence page for our [latest guidelines](https://onphase.atlassian.net/wiki/spaces/QA/pages/2744549377/Guidelines)
and [Automation Knowledge Base](https://onphase.atlassian.net/wiki/spaces/QA/pages/2756280331/QA+Automation)

## Installation

First, install UV using the appropriate command for your operating system:

On macOS and Linux:
```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
```

On Windows:
```shell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Then clone the repository and install the dependencies:

```shell
git clone https://docuphase.visualstudio.com/QA%20Automation/_git/onPhase-Automation
cd onPhase-Automation
```

To Install dependencies:
```shell
uv sync
```

To install Playwright dependencies:
```shell
uv run playwright install
```

To install Pre-Commit:
```shell
uv run pre-commit install
```

## Project Structure

```
onPhase-Automation/
├── .azure-pipelines/     # Azure DevOps pipeline configurations
├── configs/              # Configuration files
├── data/                 # Test data and resources
│   └── visual_regression/
│       ├── ground_truth/
│       ├── obtained/
│       └── differences/
├── fixtures/             # Pytest fixtures
├── helpers/              # Helper functions and utilities
├── page_objects/         # Page object models
├── tests/                # Test files
│   ├── Portal/          # Portal-specific tests
│   └── performance/     # Performance test files
├── .env.example         # Environment variables template
├── conftest.py          # Pytest configuration
├── pyproject.toml       # Project dependencies and configuration
└── README.md            # Project documentation
```

## Configuration

The project requires a `.env` file in the root directory for configuration. You can use the provided `.env.example` as a template:

```shell
cp .env.example .env
```

Then edit the `.env` file with your actual configuration values. Contact the QA team for the actual values to use in your environment. Never commit the `.env` file with real credentials to version control.

## Usage

### Running Tests

To run all tests:
```shell
uv run pytest
```

To run specific test files or directories:
```shell
uv run pytest tests/Portal/e2e/login
```

To run tests in parallel:
```shell
uv run pytest -n auto
```

To run tests with specific markers:
```shell
uv run pytest -m "not visual_regression"
```

To generate an HTML report:
```shell
uv run pytest --html=report.html
```

## Visual Regression Testing

The framework includes comprehensive visual regression testing capabilities using Playwright. Visual regression tests are automatically marked with the `visual_regression` marker.

### Running Visual Regression Tests

To run all visual regression tests:
```shell
uv run pytest -m visual_regression
```

To update baseline images:
```shell
uv run pytest -m visual_regression --update-baseline
```

To run in parallel:
```shell
uv run pytest -m visual_regression -n auto
```

### Directory Structure

Visual regression tests use the following directory structure:
- `data/visual_regression/ground_truth`: Baseline images
- `data/visual_regression/obtained`: Test result images
- `data/visual_regression/differences`: Diff images for failed comparisons

### S3 Integration

The framework supports syncing baseline images with S3. See the [S3 Hash Image Manager documentation](helpers/README.md) for details.

## Performance Tests
We utilize [locust](https://docs.locust.io/en/stable/), an open-source performance testing framework, to benchmark and analyze the system performance under load.

### Running Locust Tests
#### Web UI Mode:
The Web UI mode allows for real-time interaction with the running tests. It provides a convenient interface to start or stop tests, view real-time statistics, and adjust test parameters on the fly. To start Locust in Web UI mode, use the following command:

```shell
uv run locust -f tests/Portal/performance/locustSample.py --config tests/Portal/performance/locust.conf
```

#### Headless Mode:
For automated testing environments, such as continuous integration pipelines, you can run Locust in headless mode. In this mode, Locust uses the configuration specified in locust.conf. The configuration file contains default parameters, but you can override these either through the command line or directly in the code.

There are two ways to run in headless mode:
1. Set `headless = true` in the **locust.conf** file
2. Add `--headless` argument to the cli
   - example: `uv run locust -f tests/Portal/performance/locustSample.py --config tests/Portal/performance/locust.conf --headless`

### Performance Test Configuration
You can configure the following parameters in your `.env` file:
- `LOCUST_USERS`: Number of concurrent users (default: 100)
- `LOCUST_SPAWN_RATE`: Users spawned per second (default: 10)
- `LOCUST_RUN_TIME`: Duration of the test (default: 5m)

## Ruff Linting
This project uses ruff for linting to ensure code quality. Pre-commit hooks are set to run ruff before each commit. If the linting process identifies issues, the commit will be halted, and you will need to fix the linting errors.

### Linting Commands

To check for linting issues:
```shell
uv run ruff check
```

To automatically fix many common linting issues:
```shell
uv run ruff check --fix
```

To view the linting configuration:
```shell
uv run ruff show-config
```

The linting rules are configured in `pyproject.toml`. For more information about the rules being enforced, refer to the [Ruff documentation](https://docs.astral.sh/ruff/).
