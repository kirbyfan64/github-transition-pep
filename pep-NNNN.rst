PEP: NNNN
Title: Migrating from hg.python.org to GitHub
Version: $Revision$
Last-Modified: $Date$
Author: Brett Cannon <brett@python.org>
Status: Active
Type: Process
Content-Type: text/x-rst
Created:
Post-History:

Abstract
========
This PEP outlines the steps required to migrate the development
process for Python from Mercurial as hosted at
hg.python.org [#h.p.o]_ to Git on GitHub [#GitHub]_. Meeting the
minimum goals of this PEP should allow for the development workflow
of Python to be as productive as it currently is, and meeting its
extended goals should improve it.

Rationale
=========
In 2014 it began to become obvious that Python's custom development
process was becoming a hindrance. As an example, for an external
contributor to submit a fix for a bug that eventually was committed,
the basic steps were:

0. Open an issue for the bug at bugs.python.org [#b.p.o]_
0. Checkout out the CPython source code from hg.python.org [#h.p.o]_
0. Make the fix
0. Upload a patch
0. Have a core developer review the patch using our fork of the
   Rietveld code review tool [#rietveld]_
0. Download the patch to make sure it still applies cleanly
0. Run the test suite
0. Commit the change
0. If the change was for a bugfix release, merge into the
   in-development branch
0. Run the test suite again
0. Commit the merge
0. Push the changes

The problem with this is it is a rather heavy process if you simply
want to fix a spelling mistake in the documentation. Even in this
simple case you could only possible skip the code review step as you
would still need to build the documentation. This led to patches
languishing on the issue tracker due to core developers not being
able to work through the backlog fast enough to keep up with
submissions. This led to a side-effect issue of discouraging
outside contribution which is critical for an open source project to
staby vibrant and have a viable future.

Hence the decision was made in late 2014 that a move to a new
development workflow was needed. A request was put out for PEPs
proposing new workflows was made, in the end leading to two:
PEP 481 and PEP 507 proposing GitHub [#github]_ and
GitLab [#gitlab]_, respectively.

The year 2015 was spent off-and-on working on those proposals and
trying to tease out details of what made them different from each
other on the core-workflow mailing list [#core-workflow]_.
PyCon US 2015 also showed that the community was a bit frustrated
with our process thanks to cognitive overheard for new contributors
as well as how long it was taking for core developers to getting
around to looking at a patch (see the end of Guido van Rossum's
keynote at PyCon US 2015 [#guido-keynote]_).

On January 1, 2016 the decision was made by Brett Cannon to move our
development process to GitHub. The key reasons for choosing GitHub
were [#reasons]_:

* There was no feature differentiating GitLab from GitHub beyond
  GitLab being open source
* Familiarity with GitHub was far higher amongst core developers and
  external contributors than with GitLab
* Our BDFL preferred GitHub


References
==========
.. [#h.p.o] https://hg.python.org

.. [#GitHub] GitHub (https://github.com)

.. [#hg] Mercurial (https://www.mercurial-scm.org/)

.. [#git] Git (https://git-scm.com/)

.. [#b.p.o]  https://bugs.python.org

.. [#rietveld] Rietveld (https://github.com/rietveld-codereview/rietveld)

.. [#gitlab] GitLab (https://about.gitlab.com/)

.. [#core-workflow] core-workflow mailing list (https://mail.python.org/mailman/listinfo/core-workflow)

.. [#guido-keynote] Guido van Rossum's keynote at PyCon US (https://www.youtube.com/watch?v=G-uKNd5TSBw)

.. [#reasons] Email to core-workflow outlining reasons why GitHub was selected
   (https://mail.python.org/pipermail/core-workflow/2016-January/000345.html)


Copyright
=========

This document has been placed in the public domain.



..
   Local Variables:
   mode: indented-text
   indent-tabs-mode: nil
   sentence-end-double-space: t
   fill-column: 70
   coding: utf-8
   End:
