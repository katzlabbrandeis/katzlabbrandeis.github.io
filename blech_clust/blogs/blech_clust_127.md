# Automating Clustering: A Leap in Data Processing 

![Visual representation of 26 auto clustering](images/20250303151335_Create_a_technical_illustration_for_a_blog_post_ab.png)


**Date: December 07, 2023**

**Contributors: Abuzar Mahmood, abuzarmahmood**

**PR: [https://github.com/katzlabbrandeis/blech_clust/pull/127](https://github.com/katzlabbrandeis/blech_clust/pull/127)**

## Introduction

The `blech_clust` project just got a major upgrade with changes that dramatically simplify and automate data clustering. This pull request, titled "26 auto clustering" by Abuzar Mahmood, represents months of work to streamline what was previously a labor-intensive process.

## Key Technical Aspects

The PR introduces changes across five files, with the centerpiece being a sophisticated automated sorting script. The improvements include fully functional auto-sorting capabilities, integration of Bayesian Gaussian Mixture (BGM) models as an alternative to standard GMM in `blech_process`, and several critical bug fixes.

Here's a look at the core of the new automated sorting script:

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

The auto-sorting capability is a game-changer for the project. What previously required careful manual intervention can now run automatically, not only saving time but also reducing the potential for human error in the clustering process.

The addition of Bayesian Gaussian Mixture models provides more sophisticated statistical modeling options, particularly valuable for complex datasets where standard GMM approaches might fall short. This enhancement significantly expands the toolkit's versatility across different experimental scenarios.

One particularly useful improvement is the ability to select arbitrary cluster numbers, giving users much more flexibility in how they approach their data analysis.

## Conclusion

This PR represents a significant evolution for `blech_clust`, transforming it into a more powerful, flexible, and user-friendly tool. The automated clustering capabilities, combined with more sophisticated statistical models and numerous bug fixes, make this update particularly valuable for researchers working with complex neural data.

As development continues, these improvements provide a solid foundation for future enhancements that will further streamline neural data analysis workflows.
