# Streamlining EMG Dependencies Installation: A Dive into Python-Only Setup

![Visual representation of 208 python only setup for emg](images/20250303151743_Create_a_technical_illustration_for_a_blog_post_ab.png)


**Date: August 24, 2024**

**Contributors: abuzarmahmood, Abuzar Mahmood**

**PR: [https://github.com/katzlabbrandeis/blech_clust/pull/211](https://github.com/katzlabbrandeis/blech_clust/pull/211)**

## Introduction

In this blog post, we will be dissecting the changes made in pull request 208, titled 'Python only setup for emg.' The purpose of these changes is to enhance the installation of emg dependencies, a critical step in setting up the 'blech_clust' environment.

## Key Technical Aspects

The changes introduced in this PR are primarily focused on refining the setup process for EMG dependencies. The key modifications include:

1. The installation of BSA requirements using conda.
2. Refactoring of conda installs.
3. An update to the `prefect_pipeline` to execute `emg-bash` in the current environment.
4. A revamp of the README for EMG installation.

A significant part of the code diff shows the changes made to the README file. The installation process is streamlined with the addition of `-y` flags to conda commands, which allows for automated confirmation. This change reduces the manual intervention required during the setup process.

The installation of EMG (BSA) requirements has been added as an optional step, enhancing the flexibility of setup. This section also includes explicit instructions for setting up the channel priority to 'strict,' which is crucial for the successful installation of these requirements.

## Impact and Benefits

The changes brought about in this pull request significantly simplify the setup process. By introducing automation through the `-y` flags and tightening the conda installs, the chances of setup errors are substantially reduced. 

Moreover, the inclusion of an optional step to install EMG (BSA) requirements provides flexibility to users. Those who need these requirements can easily install them, while those who don't can skip this step without any negative impact on the setup process.

Furthermore, the update to `prefect_pipeline` to run `emg-bash` in the current environment eliminates the need for switching environments, making the process smoother and more efficient.

## Code Examples

Here is an example of the changes in the README.md file:

```diff
-conda clean --all                                           # Removes unused packages and caches
-conda create --name blech_clust python=3.8.13               # Create "blech_clust" environment with conda requirements
+conda clean --all -y                                        # Removes unused packages and caches
+conda create --name blech_clust python=3.8.13 -y            # Create "blech_clust" environment with conda requirements
```

And here are the new optional steps added for the installation of EMG (BSA) requirements:

```diff
+### Install EMG (BSA) requirements (OPTIONAL)
+# Tested with installation after neuRecommend requirements
+cd <path_to_blech_clust>/requirements                       # Move into blech_clust folder with requirements files
+conda config --set channel_priority strict                  # Set channel priority to strict, THIS IS IMPORTANT, flexible channel priority may not work
+bash emg_install.sh                                         # Install EMG requirements
```

## Conclusion

In conclusion, this pull request brings valuable improvements to the setup process for the 'blech_clust' environment. By simplifying the installation of the EMG dependencies, it reduces the potential for errors and streamlines the setup process, offering a smoother experience for users.