# Boosting the Clarity of Blech Clust Scripts with Logging

![Visual representation of 20 add logging for all scripts](images/20250303152357_Create_a_technical_illustration_for_a_blog_post_ab.png)


**Date:** December 05, 2024

**Contributors:** Abuzar Mahmood, abuzarmahmood

**PR:** [https://github.com/katzlabbrandeis/blech_clust/pull/266](https://github.com/katzlabbrandeis/blech_clust/pull/266)

## Introduction

Anyone who's spent hours debugging a script knows the frustration of trying to figure out what went wrong with limited information. For the Katz Lab's `blech_clust` project, this pain point has been addressed with a comprehensive logging system that brings much-needed visibility to script execution.

## Changes Overview

This update focused on two key files: `blech_clust.py` and `utils/blech_utils.py`. The changes introduce a robust logging framework that captures output from all scripts, making troubleshooting and monitoring significantly easier.

The centerpiece of this update is the new `Tee` class that elegantly captures both stdout and stderr, redirecting them to log files while still displaying them in the console.

## Technical Highlights

The `Tee` class in `utils/blech_utils.py` solves a common problem in scientific computing - how to capture output from long-running processes that might be executed in the background or via scheduled jobs:

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

In `blech_clust.py`, the team replaced basic print statements with more informative log messages that provide context and improve readability:

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

This logging system brings several practical benefits to researchers using the toolkit:

1. Easier troubleshooting of failed runs, with complete logs showing exactly where things went wrong
2. Better visibility into long-running processes
3. Improved ability to share diagnostic information when seeking help
4. Historical record of script execution for reproducibility

## Conclusion

The addition of comprehensive logging represents a significant quality-of-life improvement for `blech_clust` users. While not as flashy as new analysis features, this infrastructure enhancement addresses a fundamental need in scientific computing - knowing what your code is actually doing. It's the kind of thoughtful improvement that makes a toolkit truly usable in real-world research settings.
