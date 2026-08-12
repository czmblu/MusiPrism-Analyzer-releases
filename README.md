<div align="center">

# MusiPrism Analyzer — downloads

**A prism splits light into its colours. This splits a song into its instruments.**

</div>

This repository carries the **releases** of MusiPrism Analyzer and nothing else:
the source code lives in a private repository.

It exists so that the application can update itself. On start the app asks this
repository for the latest release and, if there is a newer one, offers to
install it — replacing the application files only, about a hundred kilobytes,
while Python, the libraries and the models already on the disk are left alone.
Reaching a private repository would have meant shipping a token inside the
application, and a token inside a program that anyone can download is a token
anyone can read.

**[⬇ Go to the latest release](../../releases/latest)**

Every release carries `musiprism-app.zip`, the update the app downloads by
itself. The full ready-to-run packages, for a machine that has nothing
installed, are attached to the releases that publish them.

MIT licensed.
