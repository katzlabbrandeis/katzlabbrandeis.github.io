# Enhancing Quality Control in Blech Clust: A Look into a New Directory for Quality Control Scripts

![Visual representation of Create a directory for quality control scripts](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-wE08XsAk64HjddkFPXa3fRqv.png?st=2025-03-03T16%3A55%3A32Z&se=2025-03-03T18%3A55%3A32Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T02%3A07%3A37Z&ske=2025-03-04T02%3A07%3A37Z&sks=b&skv=2024-08-04&sig=EoMbKs58E4yDBinglE%2Ba3EDTb7CbxGDqR3AwIw%2By6Kg%3D)


**Date:** December 04, 2023

**Contributors:** abuzarmahmood, Abuzar Mahmood, GitHub

## Introduction

A significant enhancement has been made to the [blech_clust](https://github.com/katzlabbrandeis/blech_clust) repository with the creation of a new directory for quality control scripts. This update, made on December 4th, 2023, allows for a more organized and efficient approach to handling quality control processes in the project. These include checking unit similarity, detecting bridging through correlations between raw voltages, and monitoring drift in selected unit activity.

## Key Technical Aspects of The Changes

The changes made in this pull request include reorganizing files into a new directory named `qa_utils`. This new organization makes it easier to navigate and manage the different quality assurance processes in the project. 

The updated code also includes new features for detecting drift and generating outputs. These changes are essential for maintaining the accuracy and integrity of the project's data.

Here's an example of how the code has changed in `blech_clust.py`:

```diff
@@ -199,12 +199,19 @@
 print('Calculating correlation matrix for quality check')
 qa_down_rate = all_params_dict["qa_params"]["downsample_rate"]
 qa_threshold = all_params_dict["qa_params"]["bridged_channel_threshold"]
-down_dat_stack, chan_names = qa.get_all_channels(
+down_dat_stack, chan_names = channel_corr.get_all_channels(
         hdf5_name, 
         downsample_rate = qa_down_rate,)
-corr_mat = qa.intra_corr(down_dat_stack)
-qa.gen_corr_output(corr_mat, 
-                   dir_name, 
+corr_mat = channel_corr.intra_corr(down_dat_stack)
+qa_out_path = os.path.join(dir_name, 'QA_output')
+if not os.path.exists(qa_out_path):
+    os.mkdir(qa_out_path)
+else:
+    # Delete dir and remake
+    shutil.rmtree(qa_out_path)
+    os.mkdir(qa_out_path)
+channel_corr.gen_corr_output(corr_mat, 
+                   qa_out_path, 
                    qa_threshold,)
```
Notably, the testing pipeline was updated to include quality assurance, further solidifying the role of quality control in the project.

## The Impact of These Changes

The creation of a new directory for quality control scripts brings about a more organized structure to the project, making it easier for contributors to locate and update quality control processes. 

Additionally, the updated code to detect drift and generate outputs provides a more effective way to handle potential issues in data quality. By allowing for early detection and remedy of these issues, the integrity of the project's data is preserved, leading to more reliable and accurate outcomes.

In conclusion, this pull request represents a crucial step towards improving the quality control processes in the blech_clust project. It not only enhances the organization of the project but also provides valuable tools for maintaining the quality and integrity of the project's data. This update signifies a commitment to excellence and a dedication to providing the best possible tools for neuroscience research.