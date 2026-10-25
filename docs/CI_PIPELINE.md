# CI Pipeline Documentation

## 1. Overview

This project uses GitHub Actions to implement a Continuous Integration (CI) pipeline for the MLOps Iris project. The workflow is defined in `.github/workflows/ci.yml`.

The pipeline runs when code is pushed to the `main` or `develop` branches and when a pull request targets either branch.

## 2. CI Jobs

### 2.1 Build and Lint

**Job:** `build-and-lint`

- Sets up Python 3.11.
- Installs project dependencies and Flake8.
- Compiles Python files.
- Checks coding style.

**Success condition:** Compilation and linting complete without errors.

### 2.2 Test Suite

**Job:** `test-suite`

**Dependency:** `needs: build-and-lint`

- Collects the Iris dataset.
- Preprocesses the data.
- Engineers features.
- Runs automated pytest tests.
- Generates `test-results.xml` and `coverage.xml`.
- Uploads the reports as a GitHub Actions artifact.

**Success condition:** All tests pass.

### 2.3 Model Quality Gate

**Job:** `model-quality-gate`

**Dependency:** `needs: test-suite`

- Prepares the Iris dataset.
- Runs `tests/test_model_quality.py`.
- Checks the minimum acceptable model accuracy.

**Normal minimum accuracy threshold:** 0.90 (90%).

## 3. Job Dependency Order

The pipeline runs in the following order:

1. `build-and-lint`
2. `test-suite`
3. `model-quality-gate`

The `needs` keyword makes each dependent job wait for the preceding job to succeed.

## 4. Branch Protection

A ruleset named `Protect main - CI checks` targets the `main` branch.

The ruleset requires pull requests and passing status checks before merging. The required checks are `build-and-lint`, `test-suite`, and `model-quality-gate`.

## 5. Pull Request Verification

A feature branch named `feature/ci-verification-test` was used to verify the pull-request workflow. A harmless README change was committed and pushed, and a pull request was created targeting `main`.

Record the actual results of your GitHub Actions run here.

## 6. Model-Quality Regression Demonstration

A temporary branch named `feature/simulate-regression` was used to test model-quality failure detection.

The minimum accuracy threshold was temporarily changed from 0.90 to 0.999.

The documented test result was approximately 0.9667 accuracy (96.67%), below the temporary threshold.

The full test suite reported 15 passed tests and 1 failed test. The `test-suite` job failed, and the dependent `model-quality-gate` job was skipped.

The demonstration pull request was not merged. The normal threshold of 0.90 was verified on `main`.

## 7. Conclusion

The GitHub Actions CI pipeline automates code compilation, linting, testing, report generation, and model-quality validation. Pull-request checks and branch protection help prevent changes from being merged into `main` when required checks fail.