---
layout: post
title:  "Writing Command Line Scripts with UV"
date:   2025-09-01
categories: Python
---

UV is a Python package and project manager that boasts huge performance improvements over other
packaging tools. UV's benchmarks show a 10 to 100x improvement over pip [^1]. UV can handle large
projects. It also makes it easy to package single-file scripts for the command line.

Let's start with a small Python script. Our script is called excelize.py. It consolidates multiple csv
files into a single excel file. Our script is bare bones and omits error handling and robustness
in the interest of brevity.

{% highlight python %}
import sys
import polars as pl
import xlsxwriter

# Get the command line arguments
target_file_name = sys.argv[1]
source_file_names = sys.argv[2:]

# Write the excel file
with xlsxwriter.Workbook(target_file_name) as workbook:
    for source_file_name in source_file_names:
        df = pl.read_csv(source_file_name)
        df.write_excel(workbook=workbook, worksheet=source_file_name.split("/")[-1].split(".")[0])
{% endhighlight %}

We can run excelize.py from the command line, passing in the target output file as the first argument and the
source csv files as the remaining arguments.

{% highlight shell %}
python excelize.py output.xlsx input1.csv input2.csv
{% endhighlight %}

Our script works! But it presents challenges. You can't just call `excelize` directly. You have to
call `python` first. But which version of Python should you use? Python 3.12? Python 3.8? Python 2.7?
The user needs to know the correct Python version for it to work. They also need to have the correct
version of Python installed on their computer. Once the Python installation is sorted, you still
aren't in the clear. You need the dependencies. Without polars and xlsxwriter, it won't work. 

This is where UV comes in handy. UV takes care of installing and using the correct Python version
and dependencies. All we need to do is update the script [^2].

{% highlight python %}
#!/usr/bin/env -S uv run --script
#
# /// script
# requires-python = ">=3.12"
# dependencies = ["polars", "xlsxwriter"]
# ///

import sys
import polars as pl
import xlsxwriter

# Get the command line arguments
target_file_name = sys.argv[1]
source_file_names = sys.argv[2:]

# Write the excel file
with xlsxwriter.Workbook(target_file_name) as workbook:
    for source_file_name in source_file_names:
        df = pl.read_csv(source_file_name)
        df.write_excel(workbook=workbook, worksheet=source_file_name.split("/")[-1].split(".")[0])
{% endhighlight %}

The first line indicates that we are using UV to run the script.

{% highlight python %}
#!/usr/bin/env -S uv run --script
{% endhighlight %}

The comments below tell UV to use Python 3.12 or greater and to include polars and xlsxwriter.

{% highlight python %}
# /// script
# requires-python = ">=3.12"
# dependencies = ["polars", "xlsxwriter"]
# ///
{% endhighlight %}

Now we can run our script as follows in the shell.

{% highlight shell %}
excelize output.xlsx input1.csv input2.csv
{% endhighlight %}

Notice that our terminal command no longer starts with `python` and we removed the `.py`
extension from the script's file name.

When we run the above command, UV is invoked. UV downloads the appropriate Python version and
dependencies if they are not already available. Then UV runs our script using the correct
Python version and dependency versions. The user only needs to install UV and run the script.

Using UV simplifies writing and using scripts on your own laptop. It also simplifies
sharing scripts with others. For someone else to use your script, they only need to install UV
(if they haven't already) and then they can call the single file script you provide them.

One use-case is to help other developers set up their development environment for the first time.
I have worked on multiple teams that have some form of "set me up" script that helps developers
begin developing. The script might assist in setting up a virtual environment, configuring developer
environment variables, or preparing to deploy builds to a target platform (like a personal cloud
environment or a hardware platform). The challenge is that new developers may not have anything
set up on their computer yet. With UV, we can easily leverage Python to help a user who
doesn't have anything set up on their laptop yet. All they need to do is install UV and they're off!


---

[^1]: Astral, [Benchmarks](https://github.com/astral-sh/uv/blob/main/BENCHMARKS.md), retreived September 1, 2025
[^2]: Astral, [UV Guides: Running a Script with Dependencies](https://docs.astral.sh/uv/guides/scripts/#running-a-script-with-dependencies), retreived September 1, 2025