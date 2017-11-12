---
layout: post
author: sanketdg
title:  "Escaping!"
date:   2016-07-24 23:00:00 +0530
description: This post is about me and escaping.
tag:
- gsoc
comments: true
---

So, escaping.

So, for configuration parsing, escaping is a big thing.

So, for `coala`'s config parsing, it looks like having
whitespace at the end of a value is forbidden. The whole
codebase makes sure of that!

So, for this week, I am going to try to solve that.

So, at first, I have to make sure that the whitespace
doesn't get stripped while parsing. The whitespace will be escaped
at first. The second stripping happens after the unescaping,
for which I have to make sure that it only happens if the
last character is not escaped.
