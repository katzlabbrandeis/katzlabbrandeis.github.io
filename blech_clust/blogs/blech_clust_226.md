# Inferring Firing Rates with BlechRNN: A New Addition to the Blech Clust Toolset

![Visual representation of 225 Add code to infer firing rates using BlechRNN](images/20250303152154_Create_a_technical_illustration_for_a_blog_post_ab.png)


**Date:** September 28, 2024   
**Contributors:** Abuzar Mahmood, abuzarmahmood   
**PR:** [https://github.com/katzlabbrandeis/blech_clust/pull/226](https://github.com/katzlabbrandeis/blech_clust/pull/226)   

## Introduction

In a recent pull request to the Blech Clust repository, a new utility has been added for inferring firing rates from spike trains using a Recurrent Neural Network (RNN). This development represents a significant addition to the Blech Clust toolset, introducing new features and improvements that substantially enhance the tool's functionality and user experience.

## Key Changes and Implications

The PR introduced several key changes, including:

1. An operational version of RNN firing rate inference and plotting.
2. Addition of trial number and stim-time to inputs.
3. An update of README to include infer_rnn_rates usage instructions.
4. Formatting fixes for README.
5. A warning for cases when train loss exceeds test loss.
6. The addition of saving and loading of artifacts along with more command line arguments.
7. Time limits for training data for RNN and a configuration file.
8. Fitting RNN for each taste separately.
9. Writing RNN outputs to h5.
10. An update to README for infer_rnn_rates.
11. Merging master branch of Blech Clust into the new module.

The most significant change is the introduction of the `infer_rnn_rates.py` script, which utilizes the `BlechRNN` library to train an RNN on spike trains and infer the firing rates from the trained model.

```python
usage: infer_rnn_rates.py [-h] [--override_config] [--train_steps TRAIN_STEPS]
                          [--hidden_size HIDDEN_SIZE] [--bin_size BIN_SIZE]
                          [--train_test_split TRAIN_TEST_SPLIT] [--no_pca]
                          [--retrain] [--time_lims TIME_LIMS TIME_LIMS]
                          data_dir

Infer firing rates using RNN

positional arguments:
  data_dir              Path to data directory
```
The script provides a variety of command line arguments that allow users to fine-tune the RNN training and inference process, including parameters such as `--hidden_size`, `--bin_size`, and `--train_steps`.

## Impact and Benefits

The addition of RNN firing rate inference to the Blech Clust toolset provides several benefits. It introduces a modern and sophisticated method for inferring firing rates from spike train data, potentially improving the accuracy and reliability of the inferred rates. The addition of command line arguments allows users to customize the inference process, permitting a degree of flexibility and adaptability that enhances the utility's usefulness across a variety of use cases.

Moreover, the updates to the README file ensure that users are provided with clear instructions and guidelines on how to use the new features, improving accessibility and user experience.

## Conclusion

The recent changes in the Blech Clust repository underscore the project's commitment to improving its toolset and providing users with advanced and sophisticated utilities for processing and analyzing neural data. The addition of RNN firing rate inference represents a significant step forward in the project's evolution, one that will undoubtedly contribute to advancing the field of neural data analysis.