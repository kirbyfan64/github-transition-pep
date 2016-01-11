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

1. Open an issue for the bug at bugs.python.org [#b.p.o]_
2. Checkout out the CPython source code from hg.python.org [#h.p.o]_
3. Make the fix
4. Upload a patch
5. Have a core developer review the patch using our fork of the
   Rietveld code review tool [#rietveld]_
6. Download the patch to make sure it still applies cleanly
7. Run the test suite
8. Commit the change
9. If the change was for a bugfix release, merge into the
   in-development branch
10. Run the test suite again
11. Commit the merge
12. Push the changes

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

* Maintaining our own infrastructure has been a burden on volunteers
  (e.g., we currently use a custom fork of Rietveld [#rietveld]_
  that is not being maintained)
* Our custom workflow is very time-consuming for core developers
  (not enough automated tooling built to help support it)
* Our custom workflow was a hindrance to external contributors
  (acts as a barrier of entry due to time required to ramp up on
  development process)
* There was no feature differentiating GitLab from GitHub beyond
  GitLab being open source
* Familiarity with GitHub was far higher amongst core developers and
  external contributors than with GitLab
* Our BDFL preferred GitHub (who would be the first person to tell
  you that his opinion shouldn't matter, but the person making the
  decision felt it was important that the BDFL feel comfortable with
  the workflow of his own programming language to encourage his
  participation)


Repositories to Migrate
=======================
While hg.python.org [#h.p.o]_ hosts many repositories, there are only
five key repositories that must  move:

1. ``devinabox`` [#devinabox]_
2. benchmarks [#benchmarks-repo]_
3. peps [#peps-repo]_
4. devguide [#devguide-repo]_
5. cpython [#cpython-repo]_

The ``devinabox`` and benchmarks repositories are code-only. The peps
and devguide repositories involve the generation of webpages. And the
cpython repository has special requirements for integration with
bugs.python.org [#b.p.o]_.

Migration Plan
==============
The migration plan is separated into sections based on what is
required to migrate the repositories listed in the
`Repositories to Migrate`_ section. Completion of requirements
outlined in each section should unblock the migration of the related
repositories. The sections are expected to be completed in order, but
not the requirements within a section.

Requirements for Code-Only Repositories
---------------------------------------
Completion of the requirements in this section will allow the
``devinabox`` and benchmarks repositories to move to GitHub. While
``devinabox`` has a sufficiently descriptive name, the benchmarks
repository does not, hence it will be named "python-benchmark-suite".

Create a 'python-dev' team
''''''''''''''''''''''''''
To manage permissions, a 'python-dev' team will be created as part of
the python organization [#github-python-org]_. Any repository that is
moved will have the 'python-dev' team added to it with write
permissions [#github-org-perms]_. Anyone who previously had writes to
manage SSH keys on hg.python.org will become a team maintainer for the
'python-dev' team.

Define commands to move a Mercurial repository to Git
'''''''''''''''''''''''''''''''''''''''''''''''''''''
Since moving to GitHub also entails moving to Git [#git]_, we must
decide what tools and commands we will run to translate a Mercurial
repository to Git. The exact tools and steps to use are an
open issue: `Tools and commands to move from Mercurial to Git`_.

CLA enforcement
'''''''''''''''
XXX

Adding GitHub username support to bugs.python.org
+++++++++++++++++++++++++++++++++++++++++++++++++
XXX

A bot to enforce CLA signing
++++++++++++++++++++++++++++
XXX add a label for signed or not signed (signed case is to notice if bot is down)

Requirements for Webpage-Related Repositories
---------------------------------------------
XXX

Requirements for the cpython Repository
---------------------------------------
XXX

Document steps to commit a pull request
'''''''''''''''''''''''''''''''''''''''
XXX master, then cherrypick

Handling ``Misc/NEWS``
''''''''''''''''''''''
XXX

Handling ``Misc/ACKS``
''''''''''''''''''''''
XXX

Linking pull requests to issues
'''''''''''''''''''''''''''''''
XXX

Post a link to the pull request in the issue
++++++++++++++++++++++++++++++++++++++++++++
XXX

Notify the issue if the pull request is committed
+++++++++++++++++++++++++++++++++++++++++++++++++
XXX

Update linking service for mapping commit IDs to URLs
'''''''''''''''''''''''''''''''''''''''''''''''''''''
XXX

Notify ``#python-dev`` of commits
'''''''''''''''''''''''''''''''''
XXX

Create https://git.python.org
'''''''''''''''''''''''''''''
XXX

Optional, Planned Features
--------------------------
XXX

Bot to handle pull request merging
''''''''''''''''''''''''''''''''''
XXX must handle multiple branches, news entry, commit message
XXX commit queue
XXX naming

Continuous integration per pull request
'''''''''''''''''''''''''''''''''''''''
XXX

Test coverage report
''''''''''''''''''''
XXX

Notifying issues of pull request comments
'''''''''''''''''''''''''''''''''''''''''
XXX

Allow bugs.python.org to use GitHub as a login provider
'''''''''''''''''''''''''''''''''''''''''''''''''''''''
XXX

Web hooks for re-generating web content
'''''''''''''''''''''''''''''''''''''''
XXX devguide, peps, docs.python.org

Splitting out parts of the documentation into their own repositories
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
XXX

Open Issues
===========
The fate of hg.python.org
-------------------------
XXX

Tools and commands to move from Mercurial to Git
------------------------------------------------
A decision needs to be made on exactly what tooling and what commands
involving those tools will be used to convert a Mercurial repository
to Git. Currently a suggestion has been made to use
https://github.com/frej/fast-export.

Rejected Ideas
==============
Separate Python 2 and Python 3 repositories
-------------------------------------------
XXX

Commit multi-release changes in bugfix branch first
---------------------------------------------------
XXX


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

.. [#benchmarks-repo] Mercurial repository for the Unified Benchmark Suite
   (https://hg.python.org/benchmarks/)

.. [#devinabox] Mercurial repository for ``devinabox`` (https://hg.python.org/devinabox/)

.. [#peps-repo] Mercurial repository of the Python Enhancement Proposals (https://hg.python.org/peps/)

.. [#devguide-repo] Mercurial repository for the Python Developer's Guide (https://hg.python.org/devguide/)

.. [#cpython-repo] Mercurial repository for CPython (https://hg.python.org/cpython/)

.. [#github-python-org] Python organization on GitHub (https://github.com/python)

.. [#github-org-perms] GitHub repository permission levels
   (https://help.github.com/enterprise/2.4/user/articles/repository-permission-levels-for-an-organization/)

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
