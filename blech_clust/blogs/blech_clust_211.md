# Streamlining EMG Dependencies Installation - The Python Only Approach

![Visual representation of 208 python only setup for emg](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-Ix6ASmEPHWx5pc6xQjIC3vai.png?st=2025-03-03T16%3A59%3A04Z&se=2025-03-03T18%3A59%3A04Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T03%3A11%3A59Z&ske=2025-03-04T03%3A11%3A59Z&sks=b&skv=2024-08-04&sig=iWbBFy8NTgmMhivTAXwDzpbPgtbv7f1cU663k356nHA%3D)


**Date:** August 24, 2024  
**Contributors:** abuzarmahmood, Abuzar Mahmood

## Introduction

In a continuous effort to improve the efficiency and reliability of our codebase, we have made significant changes to the EMG dependencies installation process. This blog post discusses the changes made in the pull request titled "208 python only setup for emg" that was created on August 24, 2024. The primary purpose of these changes is to simplify the process of setting up EMG (Electromyography) dependencies and improve the overall readability and maintainability of the code.

## Key Changes

Firstly, we have updated the way BSA requirements are installed. Instead of the previous method, we now use Conda to install BSA requirements. Conda is an open-source package management system and environment management system that runs on Windows, macOS, and Linux.

```diff
-conda clean --all                                           # Removes unused packages and caches
+conda clean --all -y                                        # Removes unused packages and caches
+conda create --name blech_clust python=3.8.13 -y            # Create "blech_clust" environment with conda requirements
 conda activate blech_clust                                  # Activate blech_clust environment
-bash conda_requirements_base.sh                             # Install main packages using conda/mamba
+conda install -c conda-forge -y --file conda_requirements_base.txt # Install conda requirements
```

Secondly, the `prefect_pipeline` has been updated to run emg-bash in the current environment. This change simplifies the process and reduces the chances of encountering issues related to environment differences.

```diff
-def emg_jetstream_parallel(data_dir, use_emg_env = True):
+def emg_jetstream_parallel(data_dir): 
     script_name = 'bash blech_emg_jetstream_parallel.sh'
-    if use_emg_env:
-        conda_init = 'conda run -p ' + emg_env_path
-        full_str = ' '.join([conda_init, script_name])
-    else:
-        full_str = script_name
+    full_str = script_name
```

Finally, the README file has been updated to include instructions for the new installation process. The instructions are clear and easy to follow, providing users with a smooth onboarding experience.

## Impact

These changes will significantly simplify the process of setting up EMG dependencies. By reducing complexity, we hope to make it easier for both new and existing users to install and use our software. Further, by switching to a Python-only setup, we aim to reduce the potential for errors and improve the maintainability of our code.

## Conclusion

Continual updates and improvements are vital for the growth and efficiency of a codebase. This recent pull request is a testament to our commitment to improving user experience and code efficiency. By simplifying the EMG dependencies installation process, we've taken another step towards enhancing our software's usability and maintainability.