# Reading EDF
There are 2 ways that I go about reading EDF data.
## Option 1 - EdfLoader
I have a tool called EdfLoader (https://github.com/HKH515/edf-loader) which is supposed to ease loading data from EDF files and split the, into train/test sets for data analysis / machine learning.

Use this tool with precaution as I do not guarantee the correctness.
## Option 2 - pyedflib
pyedflib is a python port of the C library edflib.
The documentation is not very good, thus I am writing this document.


There are two modules in pyedflib, EdfReader and EdfWriter, they do exactly what you think they do, read edf files and write edf files, respectively.
### EdfReader

Typical usage for loading a edf file and reading data is as follows.
```python
from pyedflib import EdfReader

reader = EdfReader("path/to/file.edf")
```
### Reading the data
To get the data from a particular channel (eeg lead, oxygen, or something else), you would access the index of that channel with `reader.readSignal(i)`, note that `i` needs to be integer.

It may seem confusing to know what index each channel is stored in, to see what indices correspond to what channel you can get a list of the channel names by calling `reader.getSignalLabels()`, the output will look something like `labels = ['eog-left', 'eog-right', 'eeg-C3', 'eeg-C4', 'eeg-M2', 'eeg-M1']`.

Say you wanted to read the `eog-left` channel, you would do `reader.readSignal(0)`, as `eog-left` is the first (zero indexed) entry in `labels`.

To simplify my life, I frequently just create a dictionary mapping names of channels to indices, `label_to_index = {labels[i] : i for i in range(len(channel_names))}`, now I could simply load the data by doing `read.readSignal(label_to_index['eog-left'])`.


Here is the official documentation for EdfReader
https://pyedflib.readthedocs.io/en/latest/ref/edfreader.html#edf-bdf-file-reader-edfreader