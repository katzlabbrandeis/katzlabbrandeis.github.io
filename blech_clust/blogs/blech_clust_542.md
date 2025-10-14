# Enhanced User Experience: Graceful Handling of Missing Sampling Rate Files

**Date:** August 01, 2025  
**Contributors:** abuzarmahmood, Abuzar Mahmood, aider (gpt-4o)  
**PR:** [https://github.com/katzlabbrandeis/blech_clust/pull/542](https://github.com/katzlabbrandeis/blech_clust/pull/542)

## Introduction

In electrophysiology research, the sampling rate is a critical parameter—it determines how frequently voltage measurements are recorded from neural electrodes. Typically, this information is stored in an `info.rhd` file created during recording. But what happens when this file is missing or corrupted?

Previously, Blech Clust would simply crash with an unhelpful error message. On August 1, 2025, this changed with the introduction of an intelligent fallback mechanism that prompts users for the sampling rate when the file isn't found.

## The Problem

### The Old Behavior

When `info.rhd` was missing:

```
Traceback (most recent call last):
  File "blech_clust.py", line 42, in <module>
    sampling_rate = get_sampling_rate()
  File "utils/file_io.py", line 15, in get_sampling_rate
    raise FileNotFoundError("info.rhd file not found")
FileNotFoundError: info.rhd file not found
```

Result: Pipeline stops. User has to manually edit code or recreate the file.

### Why This Happened

The `info.rhd` file can be missing for several legitimate reasons:
- Recording system malfunction
- File corruption during transfer
- Manual data organization that separated metadata
- Old datasets where the file format wasn't used
- Testing with synthetic or reformatted data

## The Solution

Instead of failing hard, Blech Clust now gracefully handles missing sampling rate files by:

1. Attempting to read the `info.rhd` file
2. If not found, prompting the user for the sampling rate
3. Validating the user input
4. Continuing with the analysis using the provided value

### Implementation

```python
def get_sampling_rate(data_dir):
    """
    Get sampling rate from info.rhd file or prompt user if missing.
    """
    info_file = os.path.join(data_dir, 'info.rhd')
    
    if os.path.exists(info_file):
        # Normal path: read from file
        sampling_rate = read_rhd_sampling_rate(info_file)
    else:
        # Fallback: prompt user
        print("info.rhd file not found.")
        sampling_rate = get_user_sampling_rate()
    
    return sampling_rate

def get_user_sampling_rate():
    """
    Prompt user for sampling rate with validation.
    """
    while True:
        try:
            rate_str = input("Please enter sampling rate (Hz): ")
            rate = float(rate_str)
            
            # Validate reasonable range (100 Hz to 100 kHz)
            if 100 <= rate <= 100000:
                return rate
            else:
                print("Sampling rate must be between 100 and 100,000 Hz")
        except ValueError:
            print("Please enter a valid number")
```

## Key Features

### 1. User-Friendly Prompts

Clear messaging helps users understand what's happening:

```
info.rhd file not found.
Please enter sampling rate (Hz): _
```

### 2. Input Validation

Prevents common mistakes:
- Rejects non-numeric input
- Ensures rate is within reasonable range (100 Hz - 100 kHz)
- Provides helpful error messages

### 3. Backward Compatibility

Existing workflows with `info.rhd` files continue to work exactly as before—the prompt only appears when needed.

### 4. Documentation Updates

Updated documentation shows users:
- How to respond to the prompt
- Common sampling rates for different systems
- How to recreate the `info.rhd` file if needed

## Benefits

### For Users
- **No More Crashes**: Missing files don't stop the entire analysis
- **Flexibility**: Can work with datasets that lack metadata files
- **Informed Decisions**: Prompt explains what's needed and why

### For Workflows
- **Robustness**: Pipelines don't fail on minor file issues
- **Testability**: Easier to test with synthetic data
- **Portability**: Datasets can be moved and reorganized more freely

### For Debugging
- **Better Error Messages**: Clear indication of what's wrong
- **Recovery Path**: Users can continue without editing code
- **Logging**: User-provided values are logged for troubleshooting

## Technical Details

### Files Changed
- `blech_clust.py` - Updated sampling rate retrieval logic
- `utils/file_io.py` - Added user prompt functionality
- `utils/blech_utils.py` - Enhanced validation helpers

### Changes Breakdown
- **21 additions, 7 deletions**
- **3 files modified**

## Real-World Usage Scenarios

### Scenario 1: Corrupted Metadata
A researcher's recording system experiences a power failure mid-session. The data files are intact, but `info.rhd` is corrupted.

**Before:** Had to manually edit code or recreate the file  
**After:** Simply enters the known sampling rate when prompted

### Scenario 2: Legacy Data Analysis
A lab wants to reanalyze old datasets that predate the current file format.

**Before:** Had to create dummy `info.rhd` files for each dataset  
**After:** Can process data directly, providing sampling rate as needed

### Scenario 3: Synthetic Data Testing
Developers creating test datasets need to run the pipeline.

**Before:** Had to create properly formatted metadata files  
**After:** Can provide parameters programmatically or interactively

## Best Practices

When using this feature:

1. **Document the Sampling Rate**: Note the value used in your analysis notes
2. **Consider Creating the File**: If repeatedly analyzing the same dataset, create an `info.rhd` file to avoid repeated prompts
3. **Verify the Value**: Double-check that the sampling rate matches your recording system
4. **Update Scripts**: For automated pipelines, consider passing sampling rate as a command-line argument

## Future Enhancements

This fallback mechanism could be extended to handle other metadata:

- Recording duration
- Number of channels
- Channel layout information
- Experiment parameters
- Session metadata

Additionally, the prompt could offer:
- Common preset values for known recording systems
- Ability to specify units (kHz vs Hz)
- Option to save the value to a new `info.rhd` file

## Conclusion

This seemingly small change—prompting for a missing value instead of crashing—represents an important shift in software design philosophy. Rather than assuming perfect input conditions, the code gracefully degrades and guides users toward a solution.

In scientific software, where data comes from diverse sources and goes through many processing steps, this kind of robustness is invaluable. It prevents minor hiccups from derailing entire analyses and empowers users to solve problems themselves rather than waiting for developer intervention.

The collaboration with aider (gpt-4o) in implementing this feature demonstrates how AI-assisted development can help identify and solve user experience issues efficiently. The result is software that's not just more functional, but more humane.
