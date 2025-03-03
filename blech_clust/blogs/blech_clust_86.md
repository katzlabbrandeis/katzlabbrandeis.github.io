# Finding Bridged Channels Just Got Easier

![Visual representation of 80 test for bridges channels](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-4VydGryOkUU13raN8XaYIAj0.png?st=2025-03-03T16%3A54%3A41Z&se=2025-03-03T18%3A54%3A41Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T02%3A11%3A01Z&ske=2025-03-04T02%3A11%3A01Z&sks=b&skv=2024-08-04&sig=%2BOu6soV62Ebd3p8e6u23B0G4oxsv7bsth6G4VqZ6za8%3D)


**Date: August 09, 2023**
**Contributors: Abuzar Mahmood, abuzarmahmood**

## Introduction
If you've ever spent hours manually checking for bridged channels in your neural recordings, I've got good news. Abuzar's latest PR "80 test for bridges channels" just landed in Blech Clust, and it's a game-changer for our data quality checks. After struggling with this exact problem last month on a 128-channel recording, I wish I'd had this tool then!

## Technical Overview
The pull request introduces a simple yet powerful addition to the `blech_clust.py` script. The changes allow the calculation of correlations between raw data, providing a robust method to check for bridged channels.

The core of the changes centers around four key aspects:
1. Scripts to calculate thresholds of correlation on raw data
2. Working code to test for bridging
3. Sorting of data by channel number
4. Simplification of thresholding and plotting of correlation matrix

The changes spanned 5 files, with 286 lines of code added and only one line deleted. The code is primarily written in Python (py), with JSON used for certain data structures.

Let's dive into some of the changes:

```python
# Necessary blech_clust modules
from utils import read_file
from utils.blech_utils import entry_checker, imp_metadata
from utils.blech_process_utils import path_handler
```
Here, the necessary modules are imported, including `read_file`, `entry_checker`, and `imp_metadata` from the `utils` package. The modules facilitate the reading and processing of raw data files.

```python
# Calculate correlations
down_rate = 100
corr_list = []
for this_dir in tqdm(dir_list):
    metadata_handler = imp_metadata([[], this_dir])
    hdf5_name = metadata_handler.hdf5_name
    with tables.open_file(hdf5_name, 'r+') as hf5: 
        raw_elecs = hf5.list_nodes('/raw')
        down_dat = [x[:][::down_rate] for x in raw_elecs]
    corr_mat = intra_corr(np.vstack(down_dat))
    corr_list.append(corr_mat)
```
In this segment of the new code, the correlations between raw data are calculated. This is done by iterating through raw data directories, reading the data, and computing the intra-correlation matrix.

## Impact and Benefits
The changes brought about by this pull request have profound implications for the way we analyze neural data. By facilitating the calculation of correlations between raw data, a more in-depth analysis is made possible. The enhancements in the scripts for calculating thresholds and the simplified thresholding and plotting of the correlation matrix streamline the data analysis process, making it faster and more efficient.

The benefits extend not just to individual researchers, but also to the broader neuroscience community that relies on Blech Clust for data analysis. By ensuring that the data is sorted by channel number, the changes also improve the usability and understandability of the output, which is essential for collaborative projects.

## Conclusion
In conclusion, the pull request "80 test for bridges channels" brings significant enhancements to the Blech Clust project, improving the way correlations between raw data are calculated and analyzed. These changes represent a step forward in the ongoing mission to provide effective tools for the neuroscience community. They are a testament to the contributors' dedication and expertise in the field. The neuroscience community eagerly awaits more such improvements to further our understanding of the brain.
