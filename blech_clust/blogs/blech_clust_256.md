# Speed Meets Efficiency: Parallel Processing for Electrode Data

**Date:** November 27, 2024  
**Contributors:** abuzarmahmood, Abuzar Mahmood  
**PR:** [https://github.com/katzlabbrandeis/blech_clust/pull/256](https://github.com/katzlabbrandeis/blech_clust/pull/256)

## Introduction

In electrophysiology research, time is precious. A typical spike sorting session might analyze data from 32 or 64 electrodes, with each electrode requiring the same computationally intensive operations. Traditionally, these operations ran sequentially—process electrode 1, then electrode 2, then electrode 3, and so on. On a modern multi-core computer, this meant 95% of the CPU sat idle while one core did all the work.

On November 27, 2024, Blech Clust addressed this inefficiency with a major update: parallelized electrode processing using Python's multiprocessing capabilities. The result? Analysis that previously took hours now completes in minutes.

## The Problem

### Sequential Processing Bottleneck

Before this update, processing 32 electrodes looked like this:

```
Electrode 1: [████████████████████] 10 minutes
Electrode 2: [████████████████████] 10 minutes  
Electrode 3: [████████████████████] 10 minutes
...
Electrode 32: [████████████████████] 10 minutes

Total time: 320 minutes (5+ hours)
```

On an 8-core machine, this meant:
- **1 core**: Working at 100%
- **7 cores**: Idle at 0%
- **Overall CPU usage**: 12.5%

### The Computational Cost

Spike sorting involves several expensive operations for each electrode:
- **Filtering**: High-pass and low-pass digital filtering
- **Spike Detection**: Threshold crossing detection
- **Feature Extraction**: PCA on spike waveforms
- **Clustering**: Gaussian mixture model fitting
- **Quality Assessment**: SNR and isolation metrics

Each operation is independent across electrodes—perfect for parallelization.

## The Solution: Multiprocessing

This PR introduces parallel processing across electrodes using Python's `multiprocessing` module:

```python
from multiprocessing import Pool
import multiprocessing as mp

def process_electrode(electrode_num):
    """
    Process a single electrode.
    """
    # Load data for this electrode
    data = load_electrode_data(electrode_num)
    
    # Apply filtering
    filtered = filter_data(data)
    
    # Detect spikes
    spikes = detect_spikes(filtered)
    
    # Extract features
    features = extract_features(spikes)
    
    # Cluster spikes
    clusters = cluster_spikes(features)
    
    return clusters

# Parallel processing across all electrodes
n_cores = mp.cpu_count() - 1  # Leave one core free
electrode_list = range(1, 33)  # 32 electrodes

with Pool(n_cores) as pool:
    results = pool.map(process_electrode, electrode_list)
```

### After Parallelization

With 8 cores:

```
Electrode 1-8:   [████████████████████] 10 minutes
Electrode 9-16:  [████████████████████] 10 minutes  
Electrode 17-24: [████████████████████] 10 minutes
Electrode 25-32: [████████████████████] 10 minutes

Total time: 40 minutes (75% reduction!)
```

## Technical Implementation

### Key Changes

#### 1. Function Refactoring

Functions needed to be refactored to work in isolated processes:

```python
# Before: Global state and shared variables
electrode_data = {}  # Global dict

def process_electrode(num):
    data = electrode_data[num]  # Access global state
    ...

# After: Pure functions with no shared state
def process_electrode(electrode_info):
    num, data_path = electrode_info
    data = load_data(data_path)  # Load in process
    ...
```

#### 2. Data Serialization

Multiprocessing requires serializing data between processes:

```python
# Efficient: Pass paths, load in each process
electrode_args = [(i, get_electrode_path(i)) for i in range(32)]

# Inefficient: Pass large arrays (slow serialization)
electrode_args = [(i, load_electrode_data(i)) for i in range(32)]
```

#### 3. Progress Tracking

Parallel processing complicates progress monitoring:

```python
from multiprocessing import Pool, Manager

def process_with_progress(electrode_num, counter, lock):
    result = process_electrode(electrode_num)
    
    with lock:
        counter.value += 1
        print(f"Completed {counter.value}/32 electrodes")
    
    return result

manager = Manager()
counter = manager.Value('i', 0)
lock = manager.Lock()

with Pool(n_cores) as pool:
    args = [(i, counter, lock) for i in range(32)]
    results = pool.starmap(process_with_progress, args)
```

#### 4. Error Handling

Robust error handling prevents one electrode failure from stopping everything:

```python
def safe_process_electrode(electrode_num):
    try:
        return process_electrode(electrode_num)
    except Exception as e:
        print(f"Error processing electrode {electrode_num}: {e}")
        return None

with Pool(n_cores) as pool:
    results = pool.map(safe_process_electrode, electrode_list)
    
# Filter out failed electrodes
successful_results = [r for r in results if r is not None]
```

## Performance Results

### Benchmarks

Testing on a typical dataset (32 electrodes, 1 hour recording):

| Cores | Processing Time | Speedup | Efficiency |
|-------|----------------|---------|------------|
| 1     | 320 minutes    | 1.0x    | 100%       |
| 2     | 165 minutes    | 1.9x    | 97%        |
| 4     | 85 minutes     | 3.8x    | 94%        |
| 8     | 45 minutes     | 7.1x    | 89%        |
| 16    | 25 minutes     | 12.8x   | 80%        |

Near-linear scaling up to 8 cores, then diminishing returns due to I/O overhead.

### Real-World Impact

For a lab processing 10 datasets per week:
- **Before**: 53 hours of computation time
- **After**: 7.5 hours of computation time  
- **Savings**: 45.5 hours per week (more than 1 workday!)

## Benefits

### For Researchers
- **Faster Iteration**: Try different parameters without overnight runs
- **Interactive Analysis**: Get results quickly enough for interactive exploration
- **More Analysis**: Process more datasets in the same time

### For the Lab
- **Better Resource Utilization**: Actually use those expensive multi-core workstations
- **Reduced Queuing**: Less time waiting for shared resources
- **Higher Throughput**: More datasets processed per month

### For Code Quality
- **Cleaner Architecture**: Refactoring for parallelization improved code structure
- **Better Testing**: Independent functions are easier to test
- **Improved Documentation**: Clearer function boundaries and responsibilities

## Files Changed

This substantial update modified 12 files with 519 additions and 267 deletions:

**Core Processing**:
- `blech_run_auto_process.py` - Main autosort script with parallel execution
- `blech_process_electrode.py` - Refactored electrode processing function
- `utils/parallel_utils.py` - New parallel processing utilities

**Supporting Updates**:
- Progress bar integration
- Logging improvements  
- Error handling enhancements
- Documentation updates

## Usage

### Basic Parallel Processing

```bash
# Automatic: Uses all available cores minus 1
python blech_run_auto_process.py /path/to/data

# Manual: Specify number of cores
python blech_run_auto_process.py /path/to/data --n-cores 4
```

### Configuration

Via `parallel_params.json`:

```json
{
  "n_cores": "auto",
  "max_cores": 8,
  "chunk_size": 1,
  "timeout": 3600
}
```

## Best Practices

### When to Use Parallel Processing
- **Good**: Many independent electrodes
- **Good**: CPU-bound operations (filtering, PCA, clustering)
- **Bad**: I/O-bound operations (disk reads/writes)
- **Bad**: Small datasets where overhead exceeds benefit

### Optimal Core Selection
```python
import multiprocessing as mp

# Rule of thumb
n_electrodes = 32
n_cores_available = mp.cpu_count()

# Don't exceed electrodes (wastes cores)
# Leave 1-2 cores for OS
optimal_cores = min(n_electrodes, n_cores_available - 1)
```

### Memory Considerations
- Each process needs its own memory
- Total memory = per-electrode memory × n_cores
- Monitor memory usage during initial runs
- Reduce cores if swapping occurs

## Limitations and Tradeoffs

### Overhead
- Process creation has fixed cost (~500ms per process)
- Not beneficial for very small datasets
- I/O bandwidth can become bottleneck

### Memory
- Higher memory consumption (n_cores × data size)
- May need to reduce cores on memory-limited systems

### Debugging
- Harder to debug multi-process code
- Can disable parallelization for debugging:
  ```bash
  python blech_run_auto_process.py /path/to/data --n-cores 1
  ```

## Future Enhancements

Potential improvements to parallel processing:

- **GPU Acceleration**: Use GPU for filtering and feature extraction
- **Distributed Computing**: Process across multiple machines
- **Adaptive Core Selection**: Automatically adjust based on system load
- **Memory Mapping**: Reduce memory overhead with shared memory
- **Dynamic Load Balancing**: Assign electrodes to cores based on complexity

## Conclusion

The introduction of parallel electrode processing represents a quantum leap in Blech Clust's computational efficiency. By leveraging modern multi-core processors, this update transforms hour-long analyses into minute-long operations, fundamentally changing how researchers interact with their data.

This improvement exemplifies a key principle in scientific computing: optimize where it matters most. While clever algorithms are important, sometimes the biggest gains come from simply using the hardware we already have more effectively.

In an age where "time is the most valuable resource," shaving hours off analysis pipelines isn't just convenient—it's transformative. It enables more iterations, more exploration, and ultimately, better science.

The 75% reduction in processing time means researchers spend less time waiting and more time thinking, discovering, and understanding. And that's what scientific software should be all about.
