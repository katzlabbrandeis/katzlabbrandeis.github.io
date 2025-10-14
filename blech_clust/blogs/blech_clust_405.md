# At-a-Glance Dataset Insights: Introducing the Data Summary Script

**Date:** May 28, 2025  
**Contributors:** abuzarmahmood, Abuzar Mahmood  
**PR:** [https://github.com/katzlabbrandeis/blech_clust/pull/405](https://github.com/katzlabbrandeis/blech_clust/pull/405)

## Introduction

Imagine you're reviewing a neuroscience dataset. You want to quickly understand: How many neurons were recorded? What's the data quality? Are there any obvious issues? Previously, answering these questions required running multiple scripts, checking various outputs, and manually compiling the information.

On May 28, 2025, this changed with the introduction of `blech_data_summary.py`—a comprehensive script that generates a single-file summary of dataset characteristics and quality metrics.

## The Challenge

Before this update, understanding a dataset required:

1. Running QA scripts for quality metrics
2. Checking `blech_units_plot` output for unit characteristics
3. Manually reviewing log files
4. Compiling information from multiple sources
5. Hoping you didn't miss anything important

This fragmented approach was:
- **Time-consuming**: Required running multiple tools
- **Error-prone**: Easy to miss important metrics
- **Inconsistent**: Different users checked different things
- **Unshareable**: No standardized summary format

## The Solution: Comprehensive Data Summary

The new `blech_data_summary.py` script automatically generates a comprehensive dataset summary by:

1. Reading QA test outputs
2. Extracting unit characteristics
3. Analyzing recording quality
4. Computing key statistics
5. Generating a formatted report

### What's Included in the Summary

#### Recording Information
- Dataset name and path
- Recording duration
- Number of channels
- Sampling rate
- Recording date/time (if available)

#### Unit Statistics
- Total units detected
- Number of single units vs. multi-units
- Firing rate distributions
- Waveform characteristics
- ISI (inter-spike interval) statistics

#### Quality Metrics
- SNR (signal-to-noise ratio) per unit
- Isolation quality scores
- Refractory period violations
- Drift detection results
- Bridge detection (electrode correlation issues)

#### Trial Information
- Number of trials per taste/stimulus
- Trial duration statistics
- Missing or problematic trials
- Taste delivery timing

#### Data Completeness
- Which processing steps have been completed
- Missing or failed QA tests
- Warnings and errors encountered
- Recommended next steps

## Technical Implementation

### Command-Line Interface

```bash
# Basic usage
python blech_data_summary.py /path/to/dataset

# Options
python blech_data_summary.py /path/to/dataset \
    --output summary.txt \
    --format markdown \
    --verbose
```

### Key Functions

```python
def generate_summary(data_dir):
    """
    Generate comprehensive dataset summary.
    
    Parameters
    ----------
    data_dir : str
        Path to data directory
        
    Returns
    -------
    summary : dict
        Dictionary containing all summary information
    """
    summary = {}
    
    # Collect recording info
    summary['recording'] = get_recording_info(data_dir)
    
    # Analyze units
    summary['units'] = analyze_units(data_dir)
    
    # QA results
    summary['quality'] = collect_qa_results(data_dir)
    
    # Trial information
    summary['trials'] = analyze_trials(data_dir)
    
    # Processing status
    summary['processing'] = check_processing_status(data_dir)
    
    return summary
```

### Output Formats

The summary can be generated in multiple formats:

#### Plain Text
```
DATASET SUMMARY
===============

Dataset: experiment_20250528
Duration: 3600.0 seconds
Channels: 32
Sampling Rate: 30000 Hz

UNITS
-----
Total Units: 47
Single Units: 35
Multi Units: 12

Average Firing Rate: 2.34 ± 1.87 Hz
Median ISI: 45.2 ms

QUALITY METRICS
--------------
Average SNR: 4.2 ± 1.3
Units with SNR > 3.0: 42/47 (89%)
Refractory Violations: 3/47 (6%)

...
```

#### Markdown
```markdown
# Dataset Summary

## Recording Information
- **Dataset**: experiment_20250528
- **Duration**: 3600.0 seconds  
- **Channels**: 32
- **Sampling Rate**: 30000 Hz

## Units
- **Total**: 47 units
  - Single Units: 35
  - Multi Units: 12
- **Average Firing Rate**: 2.34 ± 1.87 Hz

...
```

#### JSON (for programmatic access)
```json
{
  "recording": {
    "name": "experiment_20250528",
    "duration_s": 3600.0,
    "channels": 32,
    "sampling_rate": 30000
  },
  "units": {
    "total": 47,
    "single_units": 35,
    "firing_rate_mean": 2.34
  }
}
```

## Benefits

### For Individual Researchers
- **Quick Assessment**: Understand dataset quality in seconds
- **Decision Making**: Identify which datasets warrant deeper analysis
- **Troubleshooting**: Quickly spot data issues
- **Documentation**: Automatically document dataset characteristics

### For Lab Groups
- **Standardization**: Consistent dataset evaluation across lab members
- **Communication**: Easy sharing of dataset information
- **Quality Control**: Systematic tracking of recording quality
- **Archiving**: Metadata preservation for long-term storage

### For Publications
- **Reporting**: Automated generation of dataset statistics for methods sections
- **Reproducibility**: Complete documentation of data characteristics
- **Transparency**: Clear reporting of data quality metrics

## Real-World Applications

### Scenario 1: Dataset Triage
A lab has 50 recording sessions. Which should be analyzed first?

```bash
# Generate summaries for all datasets
for dir in /data/recordings/*/; do
    python blech_data_summary.py "$dir" --output "${dir}/summary.txt"
done

# Review summaries to prioritize high-quality datasets
```

### Scenario 2: Troubleshooting
An analysis pipeline failed. What went wrong?

```bash
python blech_data_summary.py /data/failed_session --verbose
```

Output reveals: "Missing QA outputs for drift detection" → Points to exact issue

### Scenario 3: Methods Section Writing
Need dataset statistics for a paper?

```bash
python blech_data_summary.py /data/paper_dataset --format markdown
```

Copy relevant statistics directly into manuscript

## Files Changed

- `blech_data_summary.py` - New summary generation script (435 lines)

**Total: 435 additions across 1 file**

## Usage Best Practices

### During Data Collection
- Run summary after each recording session
- Compare to expected values to catch issues early
- Archive summaries with raw data

### During Analysis
- Generate summary before starting analysis
- Use summary to guide analysis approach
- Reference summary when interpreting results

### For Publication
- Include summary in supplementary materials
- Use statistics in methods section
- Provide summaries for all analyzed datasets

## Future Enhancements

Potential additions to the summary script:

- **Visualization**: Automatic generation of summary plots
- **Comparison**: Compare multiple datasets
- **Alerts**: Highlight potential quality issues
- **Templates**: Customizable summary templates
- **Database Integration**: Store summaries in searchable database
- **Web Interface**: Browser-based summary viewer

## Conclusion

The `blech_data_summary.py` script represents a significant improvement in dataset management and understanding. By consolidating information from multiple sources into a single, comprehensive summary, it saves time, reduces errors, and improves documentation.

In an era where neuroscience datasets are growing larger and more complex, tools that help researchers quickly understand and evaluate their data are invaluable. This summary script does exactly that—transforming a tedious manual process into an automated, standardized procedure.

As the field moves toward greater emphasis on data sharing and reproducibility, having detailed, automatically-generated dataset documentation becomes not just convenient, but essential. This script is a step toward that future.
