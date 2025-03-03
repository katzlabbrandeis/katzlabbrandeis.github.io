# Streamlining Quality Control: A peek into the latest code changes in Blech_clust

![Visual representation of Create a directory for quality control scripts](images/20250303151235_Create_a_technical_illustration_for_a_blog_post_ab.png)


**Date: December 04, 2023**  
**Contributors: abuzarmahmood, Abuzar Mahmood, GitHub**  
**PR: [https://github.com/katzlabbrandeis/blech_clust/pull/125](https://github.com/katzlabbrandeis/blech_clust/pull/125)**

## Introduction

The Blech_clust project has undergone a significant update with the aim of enhancing its quality control processes. This blog post aims to explore these changes in detail, providing insights into what has been altered, why these changes were necessary, and how they will improve the overall quality and functionality of the codebase.

## Key Technical Aspects of the Changes

The main highlight of this pull request (PR) is the creation of a new directory for quality control scripts. This modification aims to keep all scripts related to quality checks in one place, facilitating easier access and improving organization. 

The changes include:

- Reorganizing files into a new 'qa_utils' directory.
- Code adjustments to detect and generate outputs for drift in selected unit activity.
- The addition of a line plot of binned firing.
- Incorporation of similarity violations into a new schema.
- Updating 'channel_corr' to work with the new organization.
- Updating the testing pipeline to include quality assurance.
- Overhauling the 'blech_clust' code to work with the new QA output directory.
- Textual changes in the README file to reflect the new changes.
- Minor fixes and updates in the README.md flowchart.
- Loading all parameters for quality assurance from a params file.

The changes made to the 'blech_clust.py' file are particularly noteworthy. The code has been modified to generate outputs for quality checks in a new directory 'QA_output'. If this directory already exists, the code deletes it and remakes it. This eliminates any chance of old, inaccurate data influencing the new quality checks.

```python
# Old Code
corr_mat = qa.intra_corr(down_dat_stack)
qa.gen_corr_output(corr_mat, dir_name, qa_threshold,)

# New Code
corr_mat = channel_corr.intra_corr(down_dat_stack)
qa_out_path = os.path.join(dir_name, 'QA_output')
if not os.path.exists(qa_out_path):
    os.mkdir(qa_out_path)
else:
    # Delete dir and remake
    shutil.rmtree(qa_out_path)
    os.mkdir(qa_out_path)
channel_corr.gen_corr_output(corr_mat, qa_out_path, qa_threshold,)
```

## Impact and Benefits of these Changes

These changes are poised to bring significant improvements to the Blech_clust project. The creation of a dedicated directory for quality control scripts will make code management easier. This change, coupled with the reorganization of files, will improve the readability and maintainability of the codebase.

Moreover, the modifications made for quality assurance, such as drift detection and similarity violation checks, will improve the reliability and accuracy of the data processed by Blech_clust. The ability of the updated code to remove and recreate the QA output directory ensures that quality checks are always based on the most recent and relevant data.

## Conclusion

The latest updates in the Blech_clust repository are a significant step towards enhancing the robustness and maintainability of the project. By placing a stronger emphasis on quality control and code organization, the contributors have ensured that the project is better equipped to handle future developments and challenges. As the project continues to evolve, further enhancements and robust quality checks will be key to maintaining the integrity of the project and its data.