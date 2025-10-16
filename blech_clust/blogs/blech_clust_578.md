# Multi-Platform Installation Testing: Ensuring Compatibility Across Linux Distributions

**Date:** September 26, 2025  
**Contributors:** abuzarmahmood, Abuzar Mahmood  
**PR:** [https://github.com/katzlabbrandeis/blech_clust/pull/578](https://github.com/katzlabbrandeis/blech_clust/pull/578)

## Introduction

One of the biggest challenges in scientific software development is ensuring that your code works reliably across different computing environments. Researchers use various Linux distributions, Python versions, and system configurations. What works perfectly on Ubuntu 24.04 with Python 3.11 might fail mysteriously on Ubuntu 20.04 with Python 3.8.

On September 26, 2025, the Blech Clust project addressed this challenge head-on by introducing a dedicated GitHub Actions workflow for testing installation across multiple platforms and Python versions. This update ensures that researchers can confidently install and use Blech Clust regardless of their specific setup.

## The Problem

Previously, the Blech Clust project's installation testing was tightly coupled with functional testing in a single workflow. This created several issues:

- Installation problems only surfaced when running full functional tests
- Testing multiple OS/Python combinations was resource-intensive on self-hosted runners
- Difficult to isolate installation issues from functionality issues
- No systematic coverage of different Ubuntu versions and Python releases

## The Solution: Dedicated Installation Workflow

This PR introduces a new, separate GitHub Actions workflow specifically designed for installation testing:

### Multi-Platform Matrix Testing

The new workflow tests across a comprehensive matrix of environments:

**Operating Systems:**
- Ubuntu 20.04 (LTS)
- Ubuntu 22.04 (LTS)
- Ubuntu 24.04 (Latest)

**Python Versions:**
- Python 3.8
- Python 3.9
- Python 3.10
- Python 3.11

This matrix creates **12 different test configurations**, ensuring broad compatibility.

### Key Features

#### 1. Label-Triggered Execution
The workflow only runs when a PR is labeled with 'install', preventing unnecessary resource consumption:

```yaml
on:
  pull_request:
    types: [labeled]
  
jobs:
  installation-test:
    if: contains(github.event.pull_request.labels.*.name, 'install')
```

#### 2. GitHub-Hosted Runners
Uses GitHub's infrastructure instead of self-hosted runners, improving reliability and reducing maintenance burden.

#### 3. Comprehensive Make Target Testing
Tests all installation targets defined in the Makefile:
- `make base` - Core dependencies
- `make emg` - EMG analysis components
- `make neurec` - Neural recommendation classifier
- `make blechrnn` - RNN-based firing rate inference
- `make prefect` - Workflow orchestration
- `make patch` - Dependency patches

#### 4. Verification Steps
Includes checks to ensure dependencies are properly installed and accessible.

## Technical Implementation

### Workflow Structure

```yaml
name: Installation Test

on:
  pull_request:
    types: [labeled]

jobs:
  test-installation:
    runs-on: ${{ matrix.os }}
    if: contains(github.event.pull_request.labels.*.name, 'install')
    
    strategy:
      matrix:
        os: [ubuntu-20.04, ubuntu-22.04, ubuntu-24.04]
        python-version: ['3.8', '3.9', '3.10', '3.11']
```

### Separation of Concerns

The update also streamlined the main Prefect workflow by:
- Removing installation-related jobs
- Eliminating conditional logic based on 'install' label
- Focusing purely on functional testing

This separation makes both workflows simpler, faster, and easier to maintain.

## Benefits

### 1. Better Resource Utilization
Installation tests use GitHub-hosted runners, reducing load on self-hosted infrastructure while functional tests can still use specialized hardware when needed.

### 2. Improved Coverage
Testing 12 different OS/Python combinations provides much better coverage than the previous approach, catching platform-specific installation issues early.

### 3. Faster Feedback
Parallel execution across the matrix means developers get feedback on installation issues quickly, often in under 10 minutes.

### 4. Conditional Execution
Only runs when needed (when 'install' label is present), saving CI/CD resources for normal development work.

### 5. Clearer Failure Diagnosis
When installation fails, it's immediately clear which OS/Python combination has the problem, making debugging much easier.

## Impact on Development Workflow

### For Contributors
- Add the 'install' label to PRs that modify installation scripts or dependencies
- Get comprehensive feedback on installation compatibility
- Confidence that installation works across supported platforms

### For Users
- Assurance that installation will work on their specific platform
- Better documentation of supported platforms and Python versions
- Faster resolution of platform-specific installation issues

## Files Changed

- `.github/workflows/installation_test.yml` - New dedicated installation test workflow
- `.github/workflows/python_workflow_test.yml` - Streamlined to remove installation testing

## Future Enhancements

This new testing infrastructure opens the door for additional improvements:

- Testing on additional platforms (macOS, Windows via WSL)
- Testing with different conda versions
- Automated detection of dependency version conflicts
- Performance benchmarks for installation time
- Cache optimization to speed up repeated installs

## Conclusion

The introduction of dedicated multi-platform installation testing represents a significant maturity step for the Blech Clust project. By systematically testing across different Ubuntu versions and Python releases, the project ensures that researchers can successfully install and use the toolkit regardless of their specific computing environment.

This separation of concerns—installation testing vs. functional testing—makes both workflows more focused, efficient, and maintainable. It's a prime example of how thoughtful CI/CD design can improve both developer productivity and user experience.

In scientific computing, where reproducibility and accessibility are paramount, this kind of infrastructure work may not be glamorous, but it's absolutely essential.
