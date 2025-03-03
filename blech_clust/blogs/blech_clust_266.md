# Boosting the Clarity of Blech Clust Scripts with Logging

![Visual representation of 20 add logging for all scripts](images/20250303152357_Create_a_technical_illustration_for_a_blog_post_ab.png)


**Date:** *December 05, 2024*

**Contributors:** *Abuzar Mahmood (aider), abuzarmahmood, Abuzar Mahmood*

**PR:** *[https://github.com/katzlabbrandeis/blech_clust/pull/266](https://github.com/katzlabbrandeis/blech_clust/pull/266)*

## Introduction

In the realm of software development, logging is a critical tool for monitoring and debugging applications. It provides valuable insights about the application's behavior and health in different environments. This blog post examines an important update to the Katz Lab's `blech_clust` project that integrates logging into all scripts.

## Changes Overview

This update involved changes in two files: `blech_clust.py` and `utils/blech_utils.py`. The primary additions include the introduction of the `Tee` class to capture stdout/stderr and integrate it with `pipeline_graph_check`. Also, the formatting of log statements was improved to enhance readability.

## Technical Highlights

The `Tee` class, newly introduced in `utils/blech_utils.py`, is used to redirect the output to both stdout/stderr and a log file simultaneously. This capability is invaluable in scripts running in the background or processes launched via cron jobs, where the console output is not readily available.

```python
class Tee:
    """Tee output to both stdout/stderr and a log file"""
    def __init__(self, data_dir, name='output.log'):
        self.log_path = os.path.join(data_dir, name)
        self.file = open(self.log_path, 'a')
        self.stdout = sys.stdout
        self.stderr = sys.stderr
        sys.stdout = self
        sys.stderr = self
    ...
```
In `blech_clust.py`, changes were made to replace print statements with log statements, enhancing the script's readability and clarity, especially when diagnosing issues.

```diff
-            print(f'Data already present: {found_list}')
+            reload_msg = f'Data already present: {found_list}' + '\n' +\
+                        'Reload data? (yes/y/n/no) ::: '
             reload_data_str, continue_bool = entry_checker(
-                    msg='Reload data? (yes/y/n/no) ::: ',
+                    msg= reload_msg,
                     check_func=lambda x: x in ['y', 'yes', 'n', 'no'],
                     fail_response='Please enter (yes/y/n/no)')
```

## Impact and Benefits

Enhanced logging will significantly improve the debugging process and monitoring of the `blech_clust` scripts. It will provide developers with real-time visibility into the behavior of the scripts, making it easier to identify and rectify issues.

## Conclusion

Incorporating logging into all scripts of the `blech_clust` project is a laudable improvement. It not only makes it easier to monitor the scripts but also provides valuable insights for debugging. This enhancement is a testament to the continuous efforts of the contributors to maintain high-quality, robust code.