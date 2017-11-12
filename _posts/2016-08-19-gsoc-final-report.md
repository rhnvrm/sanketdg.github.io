---
layout: post
author: sanketdg
title:  "That's it, folks!"
date:   2016-08-21 23:00:00 +0530
description: Things that needed fixing.
tag:
- gsoc
- coala
comments: true
---

So this is it. The end of my Google Summer of Code. An amazing 12 weeks of
working on a *real* project with *deadlines* and *milestones*.

### Thanks, awesome mentor!

First and foremost, I would like to thank my mentor [Mischa Krüger] for his
constant guidance and support through the tenure of my project.

Thank you for clarifying my trivial issues that were way too trivial.
Thank you for clearing my doubts on the design of the classes.
Thank you for writing a basic layout for a prototype bear.
Thank you for understanding when I was not able to meet certain deadlines.
Thank you Mischa for being an awesome mentor.

### The Beginning

I was first introduced to coala in [HackerEarth IndiaHacks Open Source Hackathon].
I wanted to participate in it, so I took a look at the list of projects and saw
coala. I jumped on their gitter channel and said *hi*. Lasse hit me back instantly,
introduced me to the project, asked me to choose any newcomer issue, and my first
patch got [accepted] in no time.

As the hackathon came to an end, it was time for organisations to start thinking
about Google Summer of Code. By then, I had been taking part in regular discussions, and code reviews, Lasse asked me if I'd like to do a GSoC:

![](http://i.imgur.com/QbLSmwH.png)

I slowly pivoted to choosing *language independent documentation extraction*
as my GSoC project as I found it having greater depth than my other choices.

I feel privileged to be contributing to coala. The project itself is awesome
in its entirety. I have contributed to my fair share of open source projects
and I have never found any other project that is so organized and newcomer
friendly. How coala is awesome should be itself another post.

### About my project

Now to my project. As stated repeatedly in my past posts, my project was to
build a language independent documentation extraction and parsing library, and use
it to develop bears (static analyzing routines.)

### How it all fits together

Most of the documentation extraction routines were already [written] by my
mentor. Except a couple of tiny bugs, it worked pretty well. The documentation
extraction API was responsible for extracting the documentation given the
language, docstyle and markers and return a ``DocumentationComment`` object.

The [`DocumentationComment`] class defines one documentation comment along with
its language, docstyle, markers, indentation and range.

My first task was to write a language independent parsing routine that
would extract metadata out of a documentation i.e. description,
parameter and return information. This [resides] inside the
`DocumentationComment` class.

The point of this parsing library is to allow bear developers manipulate
metadata without worrying about destroying the format.

I then had to make sure that I had support for the most popular languages.
I used the unofficial `coalang` specification to [define] keywords and symbols
that are used in different documentation comments. They are being [loaded] along
with the docstyle.

Although I do not use the `coalang` stuff yet and still pass keywords and
symbols [manually], it will be used in future.

Lastly, I had to implement a function to assemble a parsed documentation
into a documentation comment.

I separated this functionality into two functions:

* The [first] function would take in a list of parsed documentation comment
metadata and construct a `DocumentationComment` object from that. The
object would contain the assembled documentation comment and its other
characteristics. Note that this just assembles the inside of the
documentation comment, not accounting for the indentation and markers.

* The [second] function takes this `DocumentationComment` object and assembles it
into a documentation comment, as it should be, taking account of the indentation
and the markers.

### Difficulties faced

* The first difficulty I faced was the design of the parsing module itself.
With the help of my mentor, I was able to sort that out. We decided on
using namedtuples for each of the metadata:

```
Parameter = namedtuple('Parameter', 'name, desc')
ReturnValue = namedtuple('ReturnValue', 'desc')
Description = namedtuple('Description', 'desc')
```

* If I wanted to make the library completely language independent, most settings
would have to be configurable to the end user. Initially I hardcoded the keywords
and symbols that I used, but later the coalang specification was used to define
the settings. They are yet to be used in the library.

* While trying to use the above mentioned settings, I realized that the settings
extraction didn't work for trailing spaces. Since I had to have settings with
trailing whitespace, I had to fix the extraction in the `LineParser` class.

### What has been done till now

#### coala

[56e1802] | DocumentationComment: Add language, docstyle param
[72b6c9c] | DocumentationComment: Add indent param
[bc4d7d0] | DocumentationComment: Parse python docstrings
[337b7c1] | DocumentationComment: Parse python doxygen docs
[99fa059] | DocumentationCommentTest: Refactor
[fc2e3bf] | DocumentationComment: Add JavaDoc parsing
[12ede4f] | ConsoleInteraction: Fix empty line tab display
[07135f5] | DocumentationExtraction: Fix newline parsing
[5df5932] | DocumentationComment: Fix python parsing
[f731ee4] | DocumentationComment: Remove redundant code
[e442dce] | TestUtils: Create load_testdata for loading docs
[7de9aed] | LineParser: Fix stripping for escaped whitespace
[31b0410] | DocstyleDefinition: Add metadata param
[edc67aa] | DocumentationExtraction: Conform to pep8
[3a78aa9] | DocumentationComment: Use DocstyleDefinition
[dc35a0a] | DocumentationComment: Add from_metadata()
[78ff315] | DocumentationComment: Add assemble()
[3c239d7] | setup: Package coalang files

## What lies ahead

The API still has a long way to go. A lot of things can be added/improved:

* Maybe the use of namedtuples is not that efficient. I think classes should be used and subclassed from these namedtuples. This will allow the API to be way
more flexible than it currently is, and also retaining the advantages with using
namedtuple.

* A cornercase in assembling [#2645]

* Range is not being calculated correctly. [#2646]

* The API is not using the coalang symbols/keywords. [#2629]

* A lot of things are just assumed from the documentation while parsing.
Related: [#2143]

* Trivial: [#2617], [#2616]

* A lot of documentation related bears can be developed from this API.

It has been an awesome 3 months and an even more awesome 7 months of contributing
to coala. That's it folks!

### Other projects.

Also, I want to talk about the projects of other students:

* [@hypothesist] did an awesome job on [`coala-quickstart`]. The time saved in using
  `coala-quickstart` vs. writing your own `.coafile` is huge and this will lead to
  more projects using coala. He has also worked on caching files to speed up `coala`.

* [@tushar-rishav] built [`coala-html`]! Its a web app for showing your coala results.
  He has also been working on a new [website] for coala.

* [@mr-karan] did some cool [documentation] for the `bears` and implemented syntax
  highlighting in the terminal.

* [@Adrianzatreanu] worked on the `Requirements` API.

* [@Redridge]'s work on `External Bears` will help you [write] bears in your
  favourite programming language.

* [@abhsag24] worked on the `coalang` specification. We can finally integrate
  language independent bears seamlessly!

* Thanks to [@arafsheikh], you can now [use] `coala` in Eclipse.

[Mischa Krüger]: https://github.com/Makman2

[accepted]: https://github.com/coala-analyzer/coala/pull/1195
[HackerEarth IndiaHacks Open Source Hackathon]: https://www.hackerearth.com/sprints/open-source-india-hacks-2016/
[@hypothesist]: https://github.com/hypothesist
[@mr-karan]: https://github.com/mr-karan
[@abhsag24]: https://github.com/abhsag24
[@Adrianzatreanu]: https://github.com/Adrianzatreanu
[@Redridge]: https://github.com/Redridge
[@arafsheikh]: https://github.com/arafsheikh
[@tushar-rishav]: https://github.com/tushar-rishav

[`coala-quickstart`]: https://github.com/coala-analyzer/coala-quickstart
[documentation]: https://gitlab.com/coala/website/
[use]: https://github.com/coala-analyzer/coala-eclipse
[`coala-html`]: https://github.com/coala-analyzer/coala-html
[website]: https://gitlab.com/coala/website/
[write]: http://coala.readthedocs.io/en/latest/Users/Tutorials/External_Bears.html

[3c239d7]: https://github.com/coala-analyzer/coala/commit/3c239d7
[78ff315]: https://github.com/coala-analyzer/coala/commit/78ff315
[dc35a0a]: https://github.com/coala-analyzer/coala/commit/dc35a0a
[3a78aa9]: https://github.com/coala-analyzer/coala/commit/3a78aa9
[edc67aa]: https://github.com/coala-analyzer/coala/commit/edc67aa
[31b0410]: https://github.com/coala-analyzer/coala/commit/31b0410
[7de9aed]: https://github.com/coala-analyzer/coala/commit/7de9aed
[e442dce]: https://github.com/coala-analyzer/coala/commit/e442dce
[f731ee4]: https://github.com/coala-analyzer/coala/commit/f731ee4
[5df5932]: https://github.com/coala-analyzer/coala/commit/5df5932
[07135f5]: https://github.com/coala-analyzer/coala/commit/07135f5
[12ede4f]: https://github.com/coala-analyzer/coala/commit/12ede4f
[fc2e3bf]: https://github.com/coala-analyzer/coala/commit/fc2e3bf
[99fa059]: https://github.com/coala-analyzer/coala/commit/99fa059
[337b7c1]: https://github.com/coala-analyzer/coala/commit/337b7c1
[bc4d7d0]: https://github.com/coala-analyzer/coala/commit/bc4d7d0
[72b6c9c]: https://github.com/coala-analyzer/coala/commit/72b6c9c
[56e1802]: https://github.com/coala-analyzer/coala/commit/56e1802

[#2645]: https://github.com/coala-analyzer/coala/issues/2645
[#2646]: https://github.com/coala-analyzer/coala/issues/2646
[#2629]: https://github.com/coala-analyzer/coala/issues/2629
[#2143]: https://github.com/coala-analyzer/coala/issues/2143
[#2617]: https://github.com/coala-analyzer/coala/issues/2617
[#2616]: https://github.com/coala-analyzer/coala/issues/2616

[resides]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/DocumentationComment.py#L79
[written]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/DocumentationExtraction.py
[manually]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/DocumentationComment.py#L65
[first]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/DocumentationComment.py#L146
[second]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/DocumentationComment.py#L201
[`DocumentationComment`]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/DocumentationComment.py#L9
[define]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/default.coalang#L3-L5
[loaded]: https://github.com/coala-analyzer/coala/blob/master/coalib/bearlib/languages/documentation/DocstyleDefinition.py#L181-L184
