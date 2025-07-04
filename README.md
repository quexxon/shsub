Shsub
=====

Shsub is a fast template engine for Shell.

The following file `notes.tpl` demonstrates a simple template:

	<ul>
	<%for i in notes/*.md; do-%>
	<%	title="$(grep '^# ' "$i" | head -n1)"-%>
		<li><%="$title"%></li>
	<%done-%>
	</ul>

The following command prints a HTML list
reporting titles of your Markdown notes.

	$ shsub notes.tpl

**Key Features**

- Fast: One-pass compiler written in C;
- Low memory footprint: No dynamic allocation;
- Light-weight: Standalone executable.

**Table of Contents**

- [Template Syntax](#template-syntax)
- [Installation](#installation)
- [Documentation](#documentation)
- [Migrating from 1.x](#migrating-from-1x)
- [Credits](#credits)

Template Syntax
---------------

- Shell commands are surrounded by `<%` and `%>`
and are compiled to the commands themselves;

- Shell expressions are surrounded by `<%=` and `%>`
and each `<%=expr%>` is compiled to `printf %s expr`;

- Ordinary text is compiled to the commands printing that text;

- Template comments are surrounded by `<%!` and `%>`
and are ignored;

- A template can include other templates by `<%+filename%>`;

- If `-%>` is used instead of `%>`,
the following newline character will be ignored;

- `<%%` and `%%>` are compiled to literal `<%` and `%>`.

Installation
------------

It would be better to select a version from the
[release page](https://github.com/dongyx/shsub/releases)
than downloading the working code,
unless you understand the status of the working code.
The latest stable release is always recommended.

	$ make
	$ sudo make install

By default, Shsub is installed to `/usr/local`.
You could call `shsub --version` to check the installation.

Documentation
-------------

Calling Shsub with `--help` prints a brief of the command-line options.
The complete description is documented in the man page shipped with the installation.

The implementation is explained in the [paper](https://www.dyx.name/notes/shsub-impl.html).
If you attempt to contribute to this project, it may help.

Migrating from 1.x
------------------

The breaking changes since Shsub 2.0.0 are:

- Shebang comments are no longer supported;
If an executable is required,
compile the template to a shell script using the `-c` option;

- Expressions are no longer automatically quoted and escaped;

- The `progname` environment variable is no longer set;
If the template needs to use `$0`,
compile it to a shell script using the `-c` option;

To make parallel installations of Shsub 1.x and the later version,
use the `name` installation variable:

	sudo make install name=shsub2

This will install with the name `shsub2`
instead of the default `shsub`.
Both the executable and the man page will use the specified name.

Credits
-------

Shsub was created by [DONG Yuxuan](https://www.dyx.name) in 2022.

Other code contributors:

- [Will Clardy](https://quexxon.net)

[Chris Wellons (*skeeto*)](https://nullprogram.com) reviewed the code
and gave useful advice.

The syntax of Shsub is inspired by
Jakub Jirutka's [ESH](https://github.com/jirutka/esh).
