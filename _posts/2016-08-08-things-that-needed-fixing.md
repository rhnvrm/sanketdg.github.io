---
layout: post
author: sanketdg
title:  "Things that needed fixing."
date:   2016-08-08 23:00:00 +0530
description: Things that needed fixing.
tag:
- gsoc
- documentation
comments: true
---

This week I am going to talk about the things that I have been working on,
mainly on [PR #2423]. This is the second last week of GSoC and I fixed a
lot of quirks that I was facing for the first two weeks.

The first one deals with opinionated documentation styles. Most projects
follow this style of documentation:

```
:param x:           blablabla
:param muchtoolong: blablabla
```

While this format is used in most projects, it requires a lot of maintainance (and patience). One extra line or a few extra words and you have to literally "re-design" the entire documentation comment. But there's another life-saving
style though.

```
:param x:
    blablabla
:param muchtoolong:
    blablabla
```

When I was writing the parsing algorithm for extracting documentation metadata,
I had completely forgotten about this style, and thus when I took the algorithm for
a test drive, it indeed failed. The bug and the solution were both simple. The
algorithm expects a space after the metadata symbols, which wouldn't ideally
happen in the second style. Thus, [removing] the space clearly solves the problem and
parses everything correctly. This affects parsing of the first style in a small way,
where we now have to account for an extra space.

**In the future**: I am hoping to improve this, the current process of
searching for strings is not the most effecient way, I am thinking of
slowly transitioning to regex for this.

Another tiny bug that I found was in the documentation extraction algorithms that were
already implemented by my mentor [@Makman2].

To talk about this, I need to explain what a documentation marker means in my project.
Its basically a 3-element tuple of strings that define how a documentation comment starts,
continues and ends.

So for a python docstring it would look something like `('"""', '', '"""')`.
For javadoc, it would look like `('/**', ' *', ' */')`

Now the bug was that for documentation comments that were identified with no middle marker
i.e. `marker[1] = ''`, it was completely ignoring lines that only contained a `\n` i.e.
an empty line. This would lead to wrong parsing. The [solution]  (for now) was a simple
if-statement to insert a newline if it found a empty line.

Also, I [fixed] Escaping. Although I am still not sure that the solution is bulletproof and
would work for all cases, it's good enough. Also, turns out that I have been doing the
setting extraction wrong, getting the escaped value (and not the unescaped one).

Also, I removed some code! As a developer, it feels great to remove more and more lines of code
that you don't need. First, I [removed] a relatively useless exception handling in the
parsing algorithm.

Second, I [moved] one of the functions that loaded a file and returned its lines as a list.
It was being used in the testing classes. It was being used in three separate files,
and thus was moved to a new file `TestUtils.py`, from where it was then used.
**REFACTOR EVERYTHING!!!**

Lastly, now ``DocumentationComment`` [requires] a ``DocstyleDefinition`` object, instead of
language and docstyle (which I always thought was redundant). This kind of falls in refactoring,
and thus more removing!

Coming to things that were added, I finalized the design of the assembling functions,
with the help of my mentor. So we decided on having two functions. One constructor-like [function]
that would just arrange the documentation text from the parsed documentation metadata. So it
wouldn't be responsible for the markers and indentation. It returns a ``DocumentationComment``
object that contains all the required things for the final assembling. This function could
sometimes act like a constructor, where it takes parsed metadata and spits out a readymade
DocumentationComment object ready for use.

The final assembling function just [assembles] the documentation taking into account the markers
and indentation. It returns the assembled documentation comment as a string that can be
added/updated in files. While developing this, I actually found out that my algorithm for
doing this was totally buggy and would not work for a lot of corner cases, so I am in the
process of working them out.

Also, on a side note, I [figured] out the metadata settings. This is important, because its
important to implement some variable functionality as settings, because it imparts freedom
to the user to define what they want to parse. Right now, the concept is at its infancy,
for example, settings for a conventional python docstring would like:

```
param_start = :param\ # here's a space
param_end = :
return_sep = :return:
```

That's all for this blog post, I guess. I am almost done with the work
in the core repo. I can finally start developing some cool bears!


[PR #2423]: https://github.com/coala-analyzer/coala/pull/2423
[removing]: https://github.com/coala-analyzer/coala/pull/2423/commits/6eb9ee0354371662bc2fba2fcead7cbec8cf20a6
[solution]: https://github.com/coala-analyzer/coala/pull/2423/commits/edf9f3e443fd4f758ecb80a48318237fd7723fae
[fixed]: https://github.com/coala-analyzer/coala/pull/2423/commits/a3c5187bf4943480938c7169fe575d70d8ec65c2
[removed]: https://github.com/coala-analyzer/coala/pull/2423/commits/edf9f3e443fd4f758ecb80a48318237fd7723fae
[moved]: https://github.com/coala-analyzer/coala/pull/2423/commits/99be48f18a88bb24d79b39d1e056295d725a55af
[requires]: https://github.com/coala-analyzer/coala/pull/2423/commits/19b36f847c6e2f6edaa1d7eec9ede44db31c6178
[assembles]: https://github.com/coala-analyzer/coala/pull/2423/commits/cdc885c0a8ed682a3c47ff743eefba5edcde4123
[function]: https://github.com/coala-analyzer/coala/pull/2423/commits/1f2c93510552948c4ac9301303751fb1255fd498
[figured]: https://github.com/coala-analyzer/coala/pull/2423/commits/37b58ec79cec702e1b2c40ecfda30181a53118d3
[@Makman2]: https://github.com/Makman2
