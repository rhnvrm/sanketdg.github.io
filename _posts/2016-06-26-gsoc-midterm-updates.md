---
layout: post
author: sanketdg
title:  "Summer of Code Midterm Updates"
date:   2016-06-26 03:00:00 +0530
description: GSoC Midterm updates
tag:
- gsoc
- documentation
comments: true
---

Talking about short updates, I have successfully
completed parsing routines for Python and Java.
My next task would be to use the `coalang` functionality to implement
parsing routines which are completely implicit and to provide support
for C and C++ documentation styles.

So instead of passing the parameter and return symbols as strings through a
function, they would be extracted from the coalang files. A strong API
to access coalang files would help here.

Support for multiple params also need to be kept in mind. A documentation
style can support many type of formats.

After this is done, I will start working on the big thing i.e.
`DocumentationBear`! The first feature of capitalizing sentences has
been already implemented (needs a little bit of improvement.)

The second thing to do is to implement checking the docs against a
specified style. This can be done in two ways:

- The first one involves the user supplying a regex to check against
the documentation.

- Another way would be to define some predefined styles that are generally
followed as conventions in most projects, and then check them against the
documentation. For example for python docstrings, two conventions seem to
rule:

```
:param x:          blablabla
:param muchtolong: blablabla
```

```
:param x:
    blablabla
:param muchtolong:
    blablabla
```

Supporting these two conventions as predefined styles would avoid most
projects writing a complex regex.

Then I would go forward with more functionality like indentation checking
and wrapping of characters in a long line to subsequent lines. I will also
check for grammar within documentation!

If there is time available after all this, I would go forward with refactoring
all the classes related with documentation extraction and improve the parsing
routines to make them more dynamic. I would also like to tackle the problem where
languages and docstyles have different fields for extracting, not only the current
three(description, parameters and return values).

On a final note, I have a issue tracker at [GitLab]. Also, to help me organize
my work, I have opened a public Trello [board]. The board is empty right now,
but I will start filling it up from tomorrow.

[GitLab]: https://gitlab.com/coala/GSoC2016/issues?scope=all&sort=id_desc&state=opened&utf8=%E2%9C%93&milestone_title=Documentation+Extraction
[board]: https://trello.com/b/corsZ0pn/gsoc-2016-documentation-extraction
