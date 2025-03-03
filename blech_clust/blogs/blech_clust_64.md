# Speeding Up Collision Calculations with a New Algorithm

![Visual representation of Add faster algorithm for calculating collisions](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-q59RmCdCCkKyxddSi08WZRpC.png?st=2025-03-03T16%3A53%3A49Z&se=2025-03-03T18%3A53%3A49Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T02%3A19%3A49Z&ske=2025-03-04T02%3A19%3A49Z&sks=b&skv=2024-08-04&sig=K9PfNtlKKiYCoREE3Cdf3Q/CRa%2BZyKH7GX5x/iVVvUA%3D)


**Date:** April 07, 2023  
**Contributors:** abuzarmahmood

## Introduction
In the world of software development, efficiency is key. No one loves a system that lags or consumes unnecessary resources. That's why we are constantly looking for ways to make our code run faster and more efficiently. In this blog post, I will be discussing recent changes made to the `blech_clust` library, specifically the addition of a faster algorithm for calculating collisions.

## The Changes
Our previous algorithm for calculating collisions was functional, but not as efficient as we wanted. This inefficiency prompted a code overhaul, spearheaded by our valued contributor, abuzarmahmood. 

The key change was the introduction of a new algorithm that computes unit similarity in a more efficient manner. This algorithm, `unit_similarity_abu`, leverages the `numpy` and `collections` libraries to optimize collision calculations. Here's a snippet of the new code:

```python
def unit_similarity_abu(all_spk_times):
    ...
    # Count duplicates generated dur to binning
    mat_len = max([max(x) for x in int_spike_times])

    spike_times_array = np.zeros((len(all_spk_times), mat_len+1))
    # Populate the matrix
    for i, x in enumerate(int_spike_times):
        for y in x:
            spike_times_array[i, y] = 1
    ...
    unit_distances = np.zeros((len(units), len(units)))
    for unit_num, this_dict in enumerate(overlapping_counts):
        keys, vals = list(zip(*this_dict.items()))
        unit_distances[unit_num][np.array(keys)] = np.array(vals)
    ...
```

The new code essentially computes the unit similarity by creating a matrix of spike times and counting the overlaps. It's a more efficient approach compared to the previous algorithm, which involved a nested loop through the unit times, leading to a time complexity of O(n^2).

## Impact and Benefits
The impact of this change can't be overstated. The new algorithm is significantly faster, reducing the time complexity from O(n^2) to O(n). This means that for large datasets, the time savings will be substantial.

Moreover, the reduction in computational time can lead to energy savings, which is particularly crucial for large-scale computations. Lastly, the new algorithm simplifies the codebase, making it more maintainable and easier to understand.

## Conclusion
In conclusion, this pull request has introduced an important update to the `blech_clust` library. The new algorithm for calculating collisions not only speeds up the computation but also simplifies the codebase. This change is a testament to our ongoing commitment to improve the efficiency and usability of our software. 

As always, we are open to your contributions and feedback. If you're interested in learning more or contributing, please visit the [pull request](https://github.com/katzlabbrandeis/blech_clust/pull/64) on GitHub.