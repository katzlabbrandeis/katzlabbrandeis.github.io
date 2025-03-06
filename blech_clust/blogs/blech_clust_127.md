# Automating Clustering: A Leap in Data Processing 

![Visual representation of 26 auto clustering](images/20250303151335_Create_a_technical_illustration_for_a_blog_post_ab.png)


**Date: December 07, 2023**

**Contributors: Abuzar Mahmood, abuzarmahmood**

**PR: [https://github.com/katzlabbrandeis/blech_clust/pull/127](https://github.com/katzlabbrandeis/blech_clust/pull/127)**

## Introduction

This blog post highlights the significant changes made to the `blech_clust` project, which aims to simplify and automate the process of data clustering. The changes were made in a pull request (PR) titled "26 auto clustering", authored by Abuzar Mahmood and abuzarmahmood. The PR was created on December 7, 2023. 

## Key Technical Aspects

The PR introduces various changes across five different files, with the most notable modification being the introduction of an automated sorting script. The changes include working versions of auto-sorting and auto-sort steps, integration of Bayesian Gaussian Mixture (BGM) as an alternative to regular Gaussian Mixture Model (GMM) in `blech_process`, and bug fixes.

Here is a glimpse of how the new automated sorting script looks like:

```python
# Get directory where the hdf5 file sits, and change to that directory
# Get name of directory with the data files
# Create argument parser
parser = argparse.ArgumentParser(
        description = 'Spike extraction and sorting script')
parser.add_argument('--dir-name',  '-d', 
                    help = 'Directory containing data files')
parser.add_argument('--show-plot', '-p', 
        help = 'Show waveforms while iterating (True/False)', default = 'True')
parser.add_argument('--sort-file', '-f', help = 'CSV with sorted units',
                    default = None)
args = parser.parse_args()

##############################
# Instantiate sort_file_handler
this_sort_file_handler = post_utils.sort_file_handler(args.sort_file)

if args.dir_name is not None: 
    metadata_handler = imp_metadata([[],args.dir_name])
else:
    metadata_handler = imp_metadata([])
```

## Impact and Benefits

The introduction of auto-sorting and auto-sort steps is an important development in the project. This feature not only simplifies the clustering process but also reduces chances of erroneous results due to human error. 

Furthermore, by integrating Bayesian Gaussian Mixture as an alternative to regular GMM in `blech_process`, the project now offers more robust and flexible modeling capabilities. This integration broadens the range of use-cases `blech_clust` can effectively handle.

Moreover, the bug fixes and modifications in this PR improve the overall stability and efficiency of the project. For instance, the PR allows selection of arbitrary cluster numbers, which provides users with greater flexibility when using `blech_clust`.

## Conclusion

This PR marks a significant step forward for `blech_clust`, enhancing its efficiency, flexibility, and reliability. The integration of auto-sorting and Bayesian Gaussian Mixture models, alongside other bug fixes and improvements, provides users with a more robust and versatile tool for their data clustering needs. 

As the `blech_clust` project continues to evolve and improve, we look forward to seeing further enhancements and innovations that will continue to simplify and streamline the data clustering process.