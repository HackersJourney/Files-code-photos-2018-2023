sas7bdat.py
===========

This module will read sas7bdat files using pure Python (2.6+, 3+). No SAS software
required! The module started out as a port of the R script of the same name
found here: <https://github.com/BioStatMatt/sas7bdat> but has since been
completely rewritten.

Also included with this library is a simple command line script,
`sas7bdat_to_csv`, which converts sas7bdat files to csv files. It will also
print out header information and meta data using the `--header` option and it
will batch convert files as well. Use the `--help` option for more information.

As is, I've successfully tested the script almost three hundred sample files I
found on the internet. For the most part, it works well. We can now read
compressed files!

I'm sure there are more issues that I haven't come across yet. Please let me
know if you come across a data file that isn't supported and I'll see if I can
add support for the file.

Usage
=====

To install, run:

```
pip install sas7bdat
```

To create a sas7bdat object, simply pass the constructor a file path. The
object is iterable so you can read the contents like this:

```
#!python
from sas7bdat import SAS7BDAT
with SAS7BDAT('foo.sas7bdat', skip_header=True) as reader:
    for row in reader:
        print row
```

Each row will be a list of values of type `string`, `float`, `datetime.date`,
`datetime.datetime`, or `datetime.time`. Without `skip_header`, the first row
returned will be the SAS variable names.

If you'd like to get a pandas DataFrame, use the `to_data_frame` method:

```
#!python
df = reader.to_data_frame()
```

[Variable
attributes](https://support.sas.com/documentation/cdl/en/lrcon/65287/HTML/default/viewer.htm#n08fs0rt7fikeln1uh0t8v5pt25d.htm)
are available from `reader.columns`. The order of these columns will be the same
as the corresponding values in each `row`. Each `Column` has the following
attributes:

* `col_id` (`int`) - the column number
* `name` (`bytes`)
* `label` (`bytes`)
* `format` (`str`)
* `type` (`str`)
* `length` (`int`)
