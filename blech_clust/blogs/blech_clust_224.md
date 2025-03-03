# No More Broken Builds: CI/CD Comes to Blech Clust

![Visual representation of 8 add testing and cicd for repo](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-trT2FJsRWwKxR8jSXVNMIjDd.png?st=2025-03-03T17%3A00%3A33Z&se=2025-03-03T19%3A00%3A33Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T02%3A16%3A39Z&ske=2025-03-04T02%3A16%3A39Z&sks=b&skv=2024-08-04&sig=p0NUxxpV9375fM8ezS7DoCYVC6zMginHSemtQG1ZisE%3D)


**Date: September 13, 2024**
**Contributors: GitHub, Abuzar Mahmood, abuzarmahmood**

Remember when we merged that "small fix" last month that broke spike sorting for everyone? Those days are officially over. PR #8 "add testing and cicd for repo" just landed, and it's a game-changer for our development workflow. Abuzar has set up a clever system that runs our Prefect pipeline automatically on every commit using GitHub Actions and a self-hosted runner.

## Introducing Automated Testing and CI/CD Pipeline Integration

The pull request introduces a new workflow that triggers the Prefect pipeline using GitHub Actions on a self-hosted runner. This means that whenever there's a new commit to the repository, the Prefect pipeline will automatically run, testing the new changes. If the pipeline encounters an error, it will log the output to a file, which is then "grep"ed for the term "ERROR." If it finds the term, it triggers a failure, alerting developers to issues with the code.

The PR also introduces automated testing to the 'blech_clust' repository. It includes a new Python script to run tests on a self-hosted runner, making it easier for the development team to catch bugs and issues early in the development cycle.

## Key Technical Aspects

The most significant change in this PR is the introduction of the `.github/workflows/python_workflow_test.yml` file. This file defines the new GitHub Actions workflow that runs the Prefect pipeline on a self-hosted runner. 

The workflow is divided into several jobs, each responsible for a specific task. The 'Preamble' job sets up the repository and prints some initial information. The 'Spike-Only', 'EMG-Only', and 'Spike-EMG' jobs then run the Prefect pipeline with different options and check for errors.

Here's an example of what one of these jobs looks like:

```yml
Spike-Only:
  runs-on: self-hosted
  needs: Preamble
  steps:
    - name: Set up repo
      uses: actions/checkout@v4
    - name: Prefect SPIKE only test
      shell: bash
      working-directory: /home/exouser/Desktop/blech_clust
      run: python pipeline_testing/prefect_pipeline.py -s 2>&1 |
        tee ~/Desktop/blech_clust/github.log;
        if grep -q "ERROR" ~/Desktop/blech_clust/github.log;
                          then echo "ERROR detected by bash"; exit 1; fi
```

In the `pipeline_testing/prefect_pipeline.py` file, a new argument `--raise-exception` is introduced. This argument allows the user to decide whether to raise an error if a subprocess fails, providing more flexibility when running tests.

## Impact of These Changes

With the integration of automated testing and CI/CD pipeline, developers can now catch bugs and issues early in the development cycle, saving time and effort in the long run. The logging of errors also provides a clear path to debug and solve problems, making the development process smoother and more efficient.

## Conclusion

The addition of automated testing and CI/CD pipeline integration in the 'blech_clust' repository is a huge step forward. It not only enhances the efficiency of the development process but also ensures the reliability and robustness of the codebase. This pull request sets a great example of how the integration of GitHub Actions and Prefect can streamline the development workflow and improve the quality of the code.
