# RNNs to the Rescue: Better Firing Rate Inference Has Arrived

![Visual representation of 225 Add code to infer firing rates using BlechRNN](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-v04SZM6P8a3VCr151OmvseTF.png?st=2025-03-03T17%3A02%3A13Z&se=2025-03-03T19%3A02%3A13Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T02%3A20%3A59Z&ske=2025-03-04T02%3A20%3A59Z&sks=b&skv=2024-08-04&sig=pH8uz2b6FwHGWEtZOUwir%2BFCGD8kP/ioRZm6MBHrtb0%3D)


**Date: September 28, 2024**

**Contributors: abuzarmahmood, Abuzar Mahmood**

I've been struggling with noisy firing rate estimates for months, trying every kernel method in the book. Nothing worked quite right until yesterday, when Abuzar's PR integrating the `BlechRNN` module landed. This is a game-changer for anyone working with sparse spike trains - the recurrent neural network approach captures temporal dynamics that traditional methods miss completely.

## The Crucial Changes

The primary commits in this pull request involve:

1. A working version of RNN firing rate inference and plotting
2. Enhancements to data input, including the addition of trial number and stim-time
3. Updates to the README to include usage instructions for infer_rnn_rates
4. Warning for train loss greater than test loss
5. Saving and loading of artifacts + more command line arguments
6. Adding time limits for training data for RNN and a new config file
7. Fitting the RNN for each taste separately
8. Writing RNN outputs to h5
9. Merging the master branch of abuzarmahmood/blech_clust into the newly created 225-integrate-blechrnn-as-a-module branch.

One of the most significant changes includes integrating the `BlechRNN` library in the `infer_rnn_rates.py` script. Here's a snippet from the `README.md` detailing the usage of this script:

```markdown
### Utilities
#### utils/infer_rnn_rates.py

This script is used to infer firing rates from spike trains using a Recurrent Neural Network (RNN). The RNN is trained on the spike trains and the firing rates are inferred from the trained model. The script uses the `BlechRNN` library for training the RNN.

usage: infer_rnn_rates.py [-h] [--override_config] [--train_steps TRAIN_STEPS]
                          [--hidden_size HIDDEN_SIZE] [--bin_size BIN_SIZE]
                          [--train_test_split TRAIN_TEST_SPLIT] [--no_pca]
                          [--retrain] [--time_lims TIME_LIMS TIME_LIMS]
                          data_dir
```

## Impact and Benefits

This new feature fundamentally changes how researchers can analyze spikes in neuronal activity. With these changes, users can now utilize the power of RNNs to infer firing rates from spike trains. This addition allows for a more sophisticated analysis of neuron behavior and will be a useful tool for neuroscientists.

Moreover, the warnings and data input enhancements will improve the user experience and ensure more accurate results. The added ability to save and load artifacts, along with more command-line arguments, increases flexibility and control for the users.

## Conclusion

The integration of `BlechRNN` into the `Blech_clust` package represents a significant step forward in the utilization of machine learning in the field of neuroscience. This commit not only improves the utility of the package but also paves the way for more sophisticated analyses in the future. We look forward to seeing the innovative research this update will undoubtedly inspire.
