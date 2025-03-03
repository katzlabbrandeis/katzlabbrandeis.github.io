# Title: Drift Detection Enhancement with Spike-Time Histograms: A Deep Dive into PR #302

![Visual representation of 302 drift detection using spike counts](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-6acQfYXkrqV3xUdjvirN7i0e.png?st=2025-03-03T17%3A05%3A17Z&se=2025-03-03T19%3A05%3A17Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T02%3A04%3A12Z&ske=2025-03-04T02%3A04%3A12Z&sks=b&skv=2024-08-04&sig=teh/oLKkLKb9D4E5HP5pNUDz5CmadpRA4mZURrn0gow%3D)


**Date: January 21, 2025**
**Contributors: abuzarmahmood, Abuzar Mahmood**

## Introduction

Pull Request #302 titled "drift detection using spike counts" presents a significant shift in the way drift detection is performed in the 'blech_clust' project. Instead of using Principal Component Analysis (PCA) of firing rates, the code now performs drift detection on spike-time histograms. This new method, along with other changes to the code, aims to improve the accuracy and efficiency of drift detection.

## Key Changes and Technical Aspects

The most significant change in this PR is the way drift detection is performed. Instead of using PCA of firing rates, the code now performs drift detection on spike-time histograms. This change is reflected in the refactoring of the `elbo_drift.py` file. 

```diff
- this_dat.get_spikes()
- spike_trains = this_dat.spikes
+ # Get spike-times from the hdf5 file
+ hdf5_path = this_dat.hdf5_path
+ with tables.open_file(hdf5_path, 'r') as hf5:
+    units = hf5.get_node('/sorted_units')
+    unit_names = [unit._v_name for unit in units]
+    spike_times = [unit.times.read() for unit in units]

+ # Convert to histograms
+ max_time = int(np.ceil(max([x[-1] for x in spike_times])))
+ bins = np.linspace(0, max_time, 150)
+ spiketime_hists = np.stack([np.histogram(x, bins=bins)[0]
+                           for x in spike_times])
+ zscored_hists = zscore(spiketime_hists, axis=1)

+ # Perform PCA and keep 5 components
+ pca = PCA(n_components=5, whiten=True)
+ pca.fit(zscored_hists.T)
```

The PR also introduces new features to enhance the analysis of drifts. It includes the display of total variance explained in the PCA plot title and enhances drift analysis with flexible changepoints and CSV export. 

## Impact and Benefits

These changes aim to enhance the accuracy and efficiency of drift detection. By performing drift detection on spike-time histograms, the code can potentially provide more accurate and meaningful results. The addition of flexible changepoints provides a more comprehensive analysis, and the ability to export to CSV allows for easier data manipulation and sharing. 

## Conclusion

The changes in PR #302 showcase a more robust and improved version of the 'blech_clust' project. The shift from PCA of firing rates to spike-time histograms for drift detection shows the project's commitment to improving accuracy and efficiency. With the additional features like flexible changepoints and CSV export, the project continues to evolve and improve, providing more robust and comprehensive tools for neuroscientists.

For more details, you can visit the PR on GitHub [here](https://github.com/katzlabbrandeis/blech_clust/pull/315).