# Example Workflows for Ephys Data: Bridging the Gap Between Code and Application

**Date:** July 09, 2025  
**Contributors:** abuzarmahmood, Abuzar Mahmood  
**PR:** [https://github.com/katzlabbrandeis/blech_clust/pull/359](https://github.com/katzlabbrandeis/blech_clust/pull/359)

## Introduction

One of the biggest challenges in scientific software is bridging the gap between powerful functionality and practical application. A module might have all the features you need, but if you don't know how to use them together, that power remains untapped.

The `ephys_data` module in Blech Clust provides comprehensive tools for electrophysiology data analysis. On July 9, 2025, this module became significantly more accessible with the addition of example workflows demonstrating real-world analysis pipelines.

## The Challenge

Before this update, researchers faced several hurdles when using the `ephys_data` module:

- **Learning Curve**: Understanding how methods work together required reading source code
- **Best Practices**: Unclear which approaches work best for different scenarios
- **Common Patterns**: Repetitive task implementation varied across users
- **Integration**: Uncertain how to combine multiple analysis steps

## The Solution: Comprehensive Example Workflows

This PR adds **over 1,000 lines of example code** across 6 new workflow files, demonstrating how to accomplish common analysis tasks.

### Workflow Categories

#### 1. Data Loading and Inspection
Examples showing how to:
- Load HDF5 data files
- Inspect dataset structure
- Validate data quality
- Extract metadata

#### 2. Spike Train Analysis
Demonstrations of:
- Loading spike timestamps
- Calculating firing rates
- Analyzing temporal patterns
- Creating PSTHs (peri-stimulus time histograms)

#### 3. LFP (Local Field Potential) Processing
Workflows for:
- LFP extraction and filtering
- Spectral analysis
- Phase-amplitude coupling
- Time-frequency decomposition

#### 4. Multi-Unit Analysis
Examples covering:
- Comparing activity across neurons
- Population-level statistics
- Cross-correlation analysis
- Ensemble dynamics

#### 5. Data Visualization
Visualization workflows including:
- Raster plots
- Firing rate heatmaps
- Spectrograms
- Population vectors

#### 6. Integration with Other Tools
Demonstrations of:
- Exporting data for external analysis
- Importing data from other formats
- Integrating with pandas DataFrames
- Saving results in various formats

## Technical Highlights

### Well-Documented Code

Each example includes:

```python
"""
Workflow: Analyzing Stimulus-Evoked Responses

This example demonstrates how to:
1. Load spike data aligned to stimulus onset
2. Calculate trial-averaged firing rates
3. Identify responsive neurons
4. Visualize population responses

Expected input: HDF5 file with spike times and stimulus markers
Output: Response profiles and visualization
"""
```

### Modular Structure

Workflows are organized as independent scripts that can be:
- Run standalone
- Combined for complex analyses
- Modified for specific needs
- Used as templates for new analyses

### Real Data Examples

Where possible, examples use:
- Realistic parameter values
- Common experimental designs
- Typical data characteristics
- Standard analysis approaches

## Key Workflow Examples

### Example 1: Basic Spike Analysis

```python
# Load ephys data
from ephys_data import ephys_data

# Initialize with data directory
dat = ephys_data('/path/to/data')

# Extract spike times for all neurons
spike_times = dat.get_unit_spike_times()

# Calculate firing rates using convolution
firing_rates = dat.calc_firing_rate_conv(
    spike_times=spike_times,
    bin_size=0.025,  # 25ms bins
    step_size=0.01    # 10ms steps
)

# Visualize
dat.plot_raster(spike_times)
dat.plot_firing_rates(firing_rates)
```

### Example 2: Stimulus-Response Analysis

```python
# Load trial-aligned data
spike_array = dat.get_spikes(
    time_lims=[-1000, 2000],  # -1s to +2s around stimulus
    dig_ins=[0, 1, 2]          # Three stimulus conditions
)

# Calculate PSTHs
psth = dat.calc_psth(
    spike_array,
    bin_size=50,  # 50ms bins
    smoothing='gaussian',
    sigma=25
)

# Statistical comparison
from scipy.stats import ttest_ind

baseline = psth[:, :20]   # Pre-stimulus period
response = psth[:, 20:60]  # Post-stimulus period

stats = ttest_ind(baseline, response, axis=1)
responsive_units = stats.pvalue < 0.05
```

### Example 3: LFP Spectral Analysis

```python
# Extract LFP for specific channels
lfp = dat.get_lfp_data(channels=[0, 1, 2, 3])

# Compute power spectral density
from scipy import signal

freqs, psd = signal.welch(
    lfp,
    fs=dat.sampling_rate,
    nperseg=1024
)

# Identify frequency bands
theta_power = psd[:, (freqs >= 4) & (freqs <= 8)].mean(axis=1)
gamma_power = psd[:, (freqs >= 30) & (freqs <= 80)].mean(axis=1)

# Plot spectrogram
dat.plot_spectrogram(lfp, vmax=10)
```

## Benefits

### For New Users
- **Quick Start**: Copy and modify examples rather than starting from scratch
- **Learning Resource**: Understand the module by seeing it in action
- **Best Practices**: Learn recommended approaches from working code

### For Experienced Users
- **Templates**: Starting points for custom analyses
- **Reference**: Quick lookup of syntax and parameters
- **Validation**: Verify that custom code follows established patterns

### For the Project
- **Documentation**: Executable documentation that's always up-to-date
- **Testing**: Examples serve as integration tests
- **Support**: Reduced support requests with self-service examples

## Files Changed

- `examples/workflow_basic_loading.py` - Data loading and inspection
- `examples/workflow_spike_analysis.py` - Spike train analysis
- `examples/workflow_lfp_processing.py` - LFP analysis workflows
- `examples/workflow_multi_unit.py` - Population analysis
- `examples/workflow_visualization.py` - Plotting and visualization
- `examples/workflow_integration.py` - Tool integration examples

**Total additions: 1,001 lines across 6 files**

## Usage Recommendations

### For Beginners
1. Start with `workflow_basic_loading.py`
2. Move to `workflow_spike_analysis.py`
3. Explore visualization examples
4. Try modifying examples with your data

### For Analysis Development
1. Find the closest example to your goal
2. Copy the example file
3. Modify for your specific needs
4. Add your custom analysis steps

### For Teaching
1. Use examples in tutorials
2. Have students modify parameters
3. Assign example extension exercises
4. Build lesson plans around workflows

## Future Enhancements

The workflow collection can grow to include:

- Advanced analysis techniques (decoding, dimensionality reduction)
- Machine learning integration
- Real-time analysis examples
- Multi-session analysis workflows
- Optogenetics analysis patterns
- Closed-loop experimental designs

Additionally:
- Video tutorials walking through examples
- Interactive Jupyter notebooks
- Workflow generator tools
- Community-contributed examples

## Conclusion

The addition of comprehensive example workflows transforms the `ephys_data` module from a powerful but opaque tool into an accessible resource for the entire neuroscience community. By showing rather than just telling, these examples dramatically reduce the time from "I need to analyze data" to "I have results."

This investment in examples and documentation reflects a growing recognition in scientific software development: the best features are useless if users can't figure out how to apply them. By providing clear, practical examples of real-world analysis workflows, Blech Clust empowers researchers to focus on their science rather than wrestling with code.

As the Python programming pioneer Tim Peters wrote in the Zen of Python: "If the implementation is hard to explain, it's a bad idea. If the implementation is easy to explain, it may be a good idea." These workflows make the implementation easy to explain—and therefore, easy to use.
