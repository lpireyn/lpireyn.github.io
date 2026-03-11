+++
title = "A quick look at docopt"
date = 2026-03-11
description = "A quick look at docopt, a command-line interface description language."

[taxonomies]
tags = ["cli"]
+++
[docopt](http://docopt.org/) is a command-line interface description language that takes an original approach.

<!-- more -->

Most command-line interface libraries require you to define the interface using some sort of builder or DSL.
The serious ones can then generate the usage text based on that definition.
{% margin_note(id="usage-text") %}
The usage text is typically displayed when you use the `--help` option to a command.
{% end %}
This is the case of [picocli](https://picocli.info/) (a popular Java library)
and [clap](https://github.com/clap-rs/clap) (a popular Rust crate).

docopt takes a reverse approach:
rather than generating the usage text from an interface you define manually,
it generates the interface from the usage text you write.

{% blockquote(author="docopt website", url="http://docopt.org/") %}
**docopt** is based on conventions that have been used for decades in help messages and man pages for describing a program's interface.
An interface description in **docopt** *is* such a help message.
{% end %}

At the time of this writing, there is a [Java implementation](https://github.com/docopt/docopt.java) and a [Rust crate](https://github.com/docopt/docopt.rs) &ndash; although the latter has been archived in 2023 and is not maintained anymore in favor of clap.

Although the idea is interesting, I suspect there are a few limitations to the approach,
which probably explains docopt's seemingly limited popularity.
