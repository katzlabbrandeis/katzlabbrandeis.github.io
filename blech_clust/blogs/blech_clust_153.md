# Streamlining the EMG Pipeline in the Blech Clust Project

**Date**: February 21, 2024  
**Contributors**: GitHub, Abuzar Mahmood, abuzarmahmood  
**PR**: [https://github.com/katzlabbrandeis/blech_clust/pull/153](https://github.com/katzlabbrandeis/blech_clust/pull/153)

## Introduction

A significant update has been made to the Electromyography (EMG) pipeline in the Blech Clust Project. This update simplifies the handling of trial metadata using dataframes and allows EMG data to be processed without chunking into tastes. The key contributor to this update is Abuzar Mahmood, who has made substantial changes to improve the functionality and accessibility of the project.

![Image](https://github.com/abuzarmahmood/blech_clust/assets/12436309/3a44e1a7-af29-4f48-8aa1-427b3e983a81)

## Key Technical Changes

The changes introduced in this pull request are extensive, spanning 23 files with 1328 additions and 1748 deletions. The key files updated are: README.md, blech_clust.py, blech_make_arrays.py, blech_run_process.sh, and emg/emg_BSA_segmentation.py.

One of the significant changes is the separation of EMG output by dig-in. This feature allows uneven taste deliveries, providing more flexibility in handling data.

The code has also been simplified by processing EMG data without breaking it down by taste or laser. This simplification not only eases the handling of data but also enables better management of uneven trials.

```diff
-        plt.axvline(dig_in_markers[1][marker], c='yellow', lw=2, alpha = 0.5)
-plt.yticks(np.arange(len(dig_in_int)), dig_in_str)
+        plt.axvline(dig_in_markers[1][marker], c='yellow', lw=2, alpha = 0.5,
+                    zorder = -1)
+plt.yticks(np.arange(len(dig_in_str)), dig_in_str)
 plt.title('Digital Inputs')
 plt.xlabel('Time (s)')
 plt.ylabel('Digital Input Channel')
```

The README.md file has been updated to provide better instruction and understanding for users. The workflow diagram has been updated, and a new dependency graph section has been added to aid users in understanding the flow of operations.

## Impact and Benefits

The changes brought by this update significantly improve the project's efficiency and user-friendliness. By simplifying the handling of trial metadata and processing EMG data without chunking into tastes, the pipeline becomes more streamlined and easier to use. Furthermore, the human-readable output of a `trial_info_frame` containing all metadata associated with a trial greatly enhances the user experience.

## Conclusion

The changes introduced by Abuzar Mahmood have undeniably made the Blech Clust Project more robust and user-friendly. By streamlining the EMG pipeline and improving documentation, this update ensures a smoother and more efficient user experience. This is a significant step forward for the project and sets a high standard for future updates.