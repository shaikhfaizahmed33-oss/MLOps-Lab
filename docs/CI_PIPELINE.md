# CI Pipeline Documentation

## 1. Overview
This project uses GitHub Actions to implement a Continuous Integration (CI) pipeline for the MLOps Iris project. The workflow is defined in `.github/workflows/ci.yml`.
The pipeline automatically runs when code is pushed to the `main` or `develop` branches and when a pull request targets either of these branches.
The CI pipeline checks Python code quality, executes automated tests, generates test and coverage reports, and validates model accuracy against a predefined threshold.

## 2. CI Jobs
The workflow consists of three jobs.

### 2.1 Build and Lint
* **Job name:** `build-and-lint`
* **Purpose:** Verify that the Python source code compiles successfully and follows the configured linting standards.
* **Success condition:** Python compilation and linting complete without errors.

### 2.2 Test Suite
* **Job name:** `test-suite`
* **Dependency:** `needs: build-and-lint`
* **Purpose:** Execute the automated tests to verify the correctness of the data-processing pipeline and related functionality.
* **Success condition:** All tests pass. Generates `test-results.xml` and `coverage.xml`.

### 2.3 Model Quality Gate
* **Job name:** `model-quality-gate`
* **Dependency:** `needs: test-suite`
* **Purpose:** Verify that the model satisfies the minimum acceptable accuracy requirement (90%).
* **Success condition:** The model-quality tests pass, including the minimum accuracy assertion.

## 3. Job Dependency Order
The jobs are configured with the following dependency sequence:
`build-and-lint` -> `test-suite` -> `model-quality-gate`

## 4. Branch Protection Ruleset
A GitHub branch ruleset named `Protect main - CI checks` was configured to protect the `main` branch. It requires a pull request and requires all three CI status checks to pass before merging.

## 5. Model-Quality Regression Demonstration
To test the pipeline's ability to detect a model-quality regression, the threshold in `tests/test_model_quality.py` was temporarily changed to 0.999.
* **Observed results:** The Test Suite job failed because the model accuracy (~96.67%) fell below the 99.9% threshold. The pipeline correctly identified the regression and blocked the pull request.