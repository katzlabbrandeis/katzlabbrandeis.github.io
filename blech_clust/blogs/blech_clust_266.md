# Debugging Just Got 10x Easier with Comprehensive Logging

![Visual representation of 20 add logging for all scripts](https://oaidalleapiprodscus.blob.core.windows.net/private/org-hj3a7zwinu5hXuZCuU2WvRFJ/user-o4AWhhARg4pLttg3dlHwlTci/img-VQAqCRe12f20CIGRtx6QxkJN.png?st=2025-03-03T17%3A03%3A44Z&se=2025-03-03T19%3A03%3A44Z&sp=r&sv=2024-08-04&sr=b&rscd=inline&rsct=image/png&skoid=d505667d-d6c1-4a0a-bac7-5c84a87759f8&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-03-03T02%3A22%3A16Z&ske=2025-03-04T02%3A22%3A16Z&sks=b&skv=2024-08-04&sig=asrdfvXvT2NSVAsapnfG5hTHQGI1NHhikpP0vrjMwq8%3D)


**Date: December 05, 2024** 

**Contributors: Abuzar Mahmood, abuzarmahmood, Abuzar Mahmood (aider)** 

## Introduction 

Ever spent hours trying to figure out why your script crashed halfway through processing a 24-hour recording? I have, and it's maddening. That's why I'm so excited about Abuzar's latest PR "20 add logging for all scripts" that just landed in the `blech_clust` repo. This update has already saved me countless debugging hours by showing exactly what's happening under the hood.

## The Technical Changes 

The key changes made in this PR revolve around the addition of the `Tee` class and the enhancement of log formatting. Let's delve into these changes: 

1. **Introduction of the `Tee` Class**: This class is designed to capture stdout/stderr and integrate it with `pipeline_graph_check`. The `Tee` class redirects both stdout and stderr to a log file and the console. This makes it easy to monitor the output of our scripts both in real-time and retrospectively.

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

2. **Enhancement of log formatting**: The script’s logging statements have been modified to improve readability. For example, a message asking if the user wants to reload data is now formatted more neatly.

```diff
- print(f'Data already present: {found_list}')
+ reload_msg = f'Data already present: {found_list}' + '\n' +\
+              'Reload data? (yes/y/n/no) ::: '
```

## The Impact 

These changes provide valuable benefits:

- **Improved Debugging**: By capturing stdout/stderr, developers can now have access to detailed logs which can be instrumental in debugging issues quicker.

- **Enhanced Monitoring**: The improved logging format helps in better understanding of the script execution flow and system health.

- **User-Friendly Interaction**: The enhanced formatting of user prompts leads to a more user-friendly interaction.

## Conclusion 

In conclusion, this pull request serves as an excellent example of how improving logging capability can have a significant impact on the development, maintenance, and user experience of a project. The changes made by Abuzar Mahmood are a testament to how even small enhancements can lead to substantial improvements in software quality. 

Remember, when it comes to software development, never underestimate the power of good logging!
