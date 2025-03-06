# Enhancing CI/CD with Prefect and GitHub Actions: A Deep Dive into Blech Clust's New Workflow

**Date: September 13, 2024**  
**Contributors: abuzarmahmood, Abuzar Mahmood, GitHub**  
**PR: [https://github.com/katzlabbrandeis/blech_clust/pull/224](https://github.com/katzlabbrandeis/blech_clust/pull/224)**

## Introduction

In the fast-paced world of software development, staying ahead means embracing continuous integration and deployment (CI/CD). On September 13, 2024, a game-changing pull request by Abuzar Mahmood was submitted to the Blech Clust project. This PR set out to supercharge their workflow by weaving in GitHub Actions on a self-hosted runner, sharpening the Prefect pipeline’s knack for catching and fixing errors on the fly.

## Key Technical Aspects of the Changes

### Unveiling a New Workflow

A standout addition from this PR is the `.github/workflows/python_workflow_test.yml`. This file is your go-to guide for a robust CI workflow that kicks into gear with every pull request on a self-hosted runner. It rolls out several jobs—think `Preamble`, `Spike-Only`, `EMG-Only`, and `Spike-EMG`—each zeroing in on a different part of the pipeline to ensure nothing slips through the cracks.

### Smart Error Detection

Here's where it gets nifty: error detection through bash scripting right within GitHub Actions. As each test runs, its output is logged, and a trusty `grep` command scans for any lurking "ERROR" messages. Spot one, and the script pulls the brakes by exiting with a non-zero status. This stops potential issues from sneaking further down the line.

```yaml
- name: Prefect SPIKE only test
  shell: bash
  working-directory: /home/exouser/Desktop/blech_clust
  run: python pipeline_testing/prefect_pipeline.py -s 2>&1 |
    tee ~/Desktop/blech_clust/github.log;
    if grep -q "ERROR" ~/Desktop/blech_clust/github.log;
                      then echo "ERROR detected by bash"; exit 1; fi
```

### Upgrades in `prefect_pipeline.py`

The revamped `pipeline_testing/prefect_pipeline.py` script now handles exceptions and hiccups with more finesse. A fresh command-line argument, `--raise-exception`, has been added, allowing the pipeline to flag issues when a subprocess stumbles. This extra layer of vigilance is key to keeping Prefect flows running smoothly.

```python
parser.add_argument('--raise-exception', action='store_true',
                    help='Raise error if subprocess fails')

if break_bool:
    print('====================')
    print('Raising error if subprocess fails')
    print('====================')
```

## Impact and Benefits

Rolling out these CI/CD enhancements brings a treasure trove of benefits:

1. **Boosted Reliability**: Automation is the name of the game, swiftly identifying and tackling issues as they crop up.
2. **Developer Efficiency**: With the CI pipeline handling the heavy lifting of tests, developers can channel their energy into crafting stellar code.
3. **Elevated Code Quality**: Continuous testing is like a safety net, catching bugs before they hit production.

## Challenges and Interesting Decisions

Building a bulletproof CI/CD pipeline comes with its own set of hurdles. Opting for a self-hosted runner was a major call—while it grants greater control over the environment, it also demands more setup and upkeep compared to GitHub-hosted runners. And let's not forget the clever use of bash scripting for error detection—a straightforward yet mighty approach that perfectly aligns with the project's goals.

## Context in the Broader Project

Blech Clust is all about clustering and analyzing neural data, so getting the data processing right is non-negotiable. Keeping the Prefect pipeline running like a well-oiled machine is crucial for the project's success. This PR is a strategic leap towards automating and refining the development process, paving the way for long-term stability and scalability.

## Conclusion and Future Directions

This pull request isn’t just a step forward; it’s a giant leap for the Blech Clust project, laying the groundwork for future advancements in automation and testing. Looking ahead, there’s potential to broaden test coverage, ramp up error handling, and maybe even bring cloud-based runners into the mix to complement the self-hosted setup.

Embracing CI/CD practices is more than just an operational upgrade; it fosters a culture of continuous improvement, essential for thriving in the fast-evolving realm of computational neuroscience.

![CI/CD Pipeline](https://example.com/cicd-pipeline.png)

For more details, check out the [pull request](https://github.com/katzlabbrandeis/blech_clust/pull/224) on GitHub.