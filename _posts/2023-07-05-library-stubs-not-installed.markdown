---
layout: post
title:  "Library Stubs Not Installed"
date:   2023-07-05
categories: python
---

I remember the first time I saw the mysterious mypy error. I
was sitting on the couch in my apartment, running mypy over and over. I kept 
getting the same error: `Library stubs not installed for <package>`.

Why didn't it work? My code worked. My unit tests passed. Everything
should be fine. Yet somehow, I needed to install extra stubs to make mypy
happy. I never needed stubs before. What are stubs anyway?

Fixing the problem is simple. If you copy/paste the error into a search engine,
there's probably a Stack Overflow post with an easy solution. If
you use a newer version of mypy, the error may be accompanied by a helpful suggestion
of which library stub to install.

Let's look deeper and discuss what library stubs are, why they exist, and how to fix
this mypy error.

## There's more than one way to hint a cat

Python type hints indicate the types of arguments, variables, and return values. Consider
the following code snippet.

{% highlight python %}
def add(a: int, b: int) -> int:
    return a + b

result: int = add(1, 2)
{% endhighlight %}

`int` appears as a hint. The function `add` takes two arguments of type int and returns
a value of type int. The variable `result` is an int. If you come from a language like
C/C++, type hints look familar. However, the behavior is different in Python because they
are only "hints". There is no runtime type checking [^1]. For example, we could
call the above function with two lists.

{% highlight python %}
>>> add([1,2], [3,4])
[1, 2, 3, 4]
{% endhighlight %}

The function still "works" at runtime because the hints aren't enforced. They are intended
for static type checking, not for runtime type enforcement.

Most of us are used to seeing type hints like the example above. You add hints inline with
the implementation.
When you include type hints in your library, you need to add a marker file `py.typed` that
indicates that the library includes type hints [^2]. If `py.typed` is part of the library, then
mypy will use the hints in the source code. Most code that we work on as
developers follows this convention.

However, code does not have to be hinted this way. You can include a separate package that
contains only type hints and no implementation. The type hints are included in
"stub files" that end in a pyi extension [^3]. The stub files describe
the library's interface, but lack an implementation. If we put the hints for our add function in
a pyi file, then it would look like this.

{% highlight python %}
def add(a: int, b: int) -> int:
    ...
{% endhighlight %}

Notice that the implementation is replaced with an elipse.

There are many reasons why you would deliver stubs separately. For example,
the library supports multiple versions of Python including versions that don't support
type hinting syntax [^4]. That was a concern when type hints were first introduced.

Python also supports runtime creation of classes and functions. Those libraries might
not lend themselves to readable type hints and type stubs
are a better solution. 

Without diving too deeply into why you would separate hints into type stubs, the takeaway
is that there are valid reasons to do so.
When you consume a package that follows that pattern, you need to import the stub library.

## Real examples

Open source projects are a great place to explore how and where different approaches
are used in the wild on complex projects with many users.

### PyVISA: Type hinting in the Python implementation

[PyVISA](https://github.com/pyvisa/pyvisa) is a python package for
controlling measurement devices and test equipment[^5]. For
example, you can use PyVISA to send commands to a power supply or an oscilloscope.
It uses inline type hints. If you look at its GitHub repository, you will see a
py.typed marker file. That tells mypy that the implementation files include the type hints.

If you dig into the source files, you will see those type hints. Here's a function
from the Resource class in [resources.py](https://github.com/pyvisa/pyvisa/blob/1.13.0/pyvisa/resources/resource.py)
(I removed the docstrings).

{% highlight python %}
def set_visa_attribute(
    self, name: constants.ResourceAttribute, state: Any
) -> constants.StatusCode:
    return self.visalib.set_attribute(self.session, name, state)
{% endhighlight %}

Notice that both type hints and implementation are in one place.


### Django: Third party stub library

[Django](https://github.com/django/django) is a web framework [^6].
Django does not include type hints. It does not have a py.typed marker file.

For type hints, you need a separate stub library. The Django Software Foundation
doesn't deliver stubs. There's no official stub library for Django.
But because stubs can be delivered as their own libraries, anyone can
create one. That's exactly what the developers behind
[django-stubs](https://github.com/typeddjango/django-stubs) did. They
built a stub library for Django [^7].

If we look at the GitHub repositories for Django and django-stubs
side-by-side, we see that Django has implementation and django-stubs
has type hints.

Let's compare [django/apps/registry.py](https://github.com/django/django/blob/4.2.3/django/apps/registry.py)
with [django-stubs/apps/registry.pyi](https://github.com/typeddjango/django-stubs/blob/v1.11.0/django-stubs/apps/registry.pyi).
Even before opening the files, we see a similarity. Both files are apps/registry.py(i).
Let's look at at the function `is_installed(..)` in the App class.

Here is the Django implementation (docstring is omitted).

{% highlight python %}
def is_installed(self, app_name):
    self.check_apps_ready()
    return any(ac.name == app_name for ac in self.app_configs.values())
{% endhighlight %}


There are no type hints. There is only implementation.

Now let's look at the same function in django-types.

{% highlight python %}
def is_installed(self, app_name: str) -> bool: ...
{% endhighlight %}

There is no implementation there are only type hints. The argument `app_name` is a str
and the function `is_installed(..)` returns a bool. There's no implementation, just an elipse.


### NumPy: Including type stubs in the source package

[NumPy](https://github.com/numpy/numpy) is a scientific computation 
library [^8]. It has a py.typed marker file. So we
know it contains type hints and we won't need a separate stub library.

Nonetheless, NumPy contains stub files. An interesting example is
[dtypes.py](https://github.com/numpy/numpy/blob/v1.25.0/numpy/dtypes.py) and
[dtypes.pyi](https://github.com/numpy/numpy/blob/v1.25.0/numpy/dtypes.pyi).

The heavy-lifting implementation in dtypes.py is as follows:

{% highlight python %}
__all__ = []


def _add_dtype_helper(DType, alias):
    # Function to add DTypes a bit more conveniently without channeling them
    # through `numpy.core._multiarray_umath` namespace or similar.
    from numpy import dtypes

    setattr(dtypes, DType.__name__, DType)
    __all__.append(DType.__name__)

    if alias:
        alias = alias.removeprefix("numpy.dtypes.")
        setattr(dtypes, alias, DType)
        __all__.append(alias)
{% endhighlight %}


Don't worry if you don't fully understand how the above works. The big thing
to notice is there are no dtypes declared in this file! There's a helper function that allows
code elsewhere in the NumPy library to create the dtypes.

Let's look at a few lines of dtypes.pyi.

{% highlight python %}
Int8DType = np.dtype[np.int8]
UInt8DType = np.dtype[np.uint8]
Int16DType = np.dtype[np.int16]
UInt16DType = np.dtype[np.uint16]
{% endhighlight %}

The pyi file hints the types that will be created in the dtypes module.

This is more complex than the earlier examples, but the underlying concept is the
same. Stub files allow you to separate implementation from type hinting when needed.
Developers can do the separation within a library or between libraries as they choose.


## Solutions

When the library stubs are missing, the solution is simple. Find the corresponding stub
library and add it to your dependencies.

But what if there is no corresponding stub library? A corresponding stub library might
not exist. Even if it exists, it might not be available
for you to use. If you work for a company that has an approval process
for third-party packages, there could be a stub library on the Internet that lacks approval.

One option is to write your own stub library (like django-stubs). Since the runtime library and its
stubs can be distributed separately, this is a viable option. If that's more work than you
are ready to take on, you can disable type checking for that package. You will miss out on 
type checking for that library, but you can still reap the benefits of mypy for the rest
of your code.

To disable mypy type checking, add a `type: ignore` comment. `type: ignore` tells
mypy not to do static analysis on that line of code [^9].


{% highlight python %}
import lib_without_stubs # type: ignore
{% endhighlight %}

The above solution works, but we can do better. Avoid using `type: ignore` on
its own. Indicate what is being ignored and why. 

{% highlight python %}
import lib_without_stubs # type: ignore [import] # lib_without_stubs has no type stubs. <github issue link>
{% endhighlight python %}

The above solution still disables mypy. It also indicates what is being disabled and why.
The error to ignore is in square brackets: `import`. And we include a hash and an
additional comment that explains why we are disabling mypy: `lib_without_stubs has no type stubs`.
Even better, we include a link to an issue--maybe a request on GitHub to add type hints to
the library.

## Wrapping up

The first day "Library stubs not installed" popped up in my terminal was a real head
scratcher. Diving into why it happened deepened my
understanding of Python. Learning about stubs added a new
tool to my arsenal. A year later, I wrote stubs to improve IDE code completion and mypy
coverage for a library that my team was delivering. That solution was on my radar because
I hit a simple mypy error a year earlier.

Next time you see "Library stubs not installed", you'll already know what causes it and
how to fix it. And an introductory understanding of library stubs could come in handy
when you least expect it.

---
[^1]: van Rossum et al, [PEP 484 – Type Hints](https://peps.python.org/pep-0484/), May 22, 2015, retrieved July 4 2023.
[^2]: Smith, [PEP 561 – Distributing and Packaging Type Information](https://peps.python.org/pep-0561/), April 12 2018, retrieved July 4 2023.
[^3]: Ibid.
[^4]: Ibid.
[^5]: GitHub, [PyVISA](https://github.com/pyvisa/pyvisa), retrieved July 4, 2023.
[^6]: GitHub, [Django](https://github.com/django/django), retrieved July 4, 2023.
[^7]: GitHub, [django-stubs](https://github.com/typeddjango/django-stubs), retrieved July 4, 2023.
[^8]: GitHub, [NumPy](https://github.com/numpy/numpy), retrieved July 4, 2023.
[^9]: mypy 1.4.1 Documentation, [Silencing type errors](https://mypy.readthedocs.io/en/stable/type_inference_and_annotations.html?#silencing-type-errors), retrieved July 4, 2023xs