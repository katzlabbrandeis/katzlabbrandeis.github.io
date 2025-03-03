# Blech_Clust: Making Sense of Brain Activity

## What is Blech_Clust?

Blech_Clust is a specialized software toolkit that helps neuroscientists study how the brain processes taste information. When researchers record electrical signals from the brain, they capture a complex mixture of activity from many neurons (brain cells). Blech_Clust helps separate and organize these signals so scientists can understand how individual neurons respond to different tastes.

## Why is this important?

Understanding how the brain processes taste information can help us learn about:
- How we make decisions about what to eat
- Why we prefer certain foods over others
- How taste perception changes in conditions like obesity or eating disorders
- The basic principles of how the brain processes sensory information

## How does it work?

When neuroscientists place tiny electrodes in the brain, they record electrical "spikes" - the language neurons use to communicate. However, each electrode often picks up signals from multiple nearby neurons. Blech_Clust uses sophisticated mathematical techniques to:

1. **Clean the data**: Remove electrical noise and artifacts
2. **Separate signals**: Identify which spikes came from which neurons
3. **Analyze responses**: Determine how each neuron responds to different tastes
4. **Visualize results**: Create graphs and plots to help interpret the findings

## Who uses Blech_Clust?

This software was developed in the Katz Lab at Brandeis University, where researchers study how the brain processes taste information. It's designed for neuroscientists who record brain activity using multi-electrode arrays, particularly those studying taste processing in the gustatory cortex and related brain regions.

## Technical details (for the curious)

Blech_Clust works with data recorded using Intan RHD2132 chips and is optimized for Brandeis University's high-performance computing cluster. It uses a combination of Python and R to process the data, and includes tools for:

- Spike detection and sorting
- Common average referencing
- Quality control of sorted units
- Analysis of neural responses to taste stimuli
- Processing of electromyography (EMG) data to measure mouth movements

The software is open-source and available under the GPL-3.0 license, meaning anyone can use, modify, and share it freely.

## Getting started

If you're a researcher interested in using Blech_Clust, check out the main [README](https://github.com/katzlabbrandeis/blech_clust) for detailed installation instructions and documentation on how to use the software.

## Learn More

Check out our [blog posts](blogs/blogs_main.md) for updates, tutorials, and technical deep dives on Blech_Clust.
