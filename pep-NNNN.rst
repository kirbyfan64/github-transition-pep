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
stay vibrant and have a viable future.

Hence the decision was made in late 2014 that a move to a new
development workflow was needed. A request for PEPs
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
six key repositories that must  move:

1. devinabox [#devinabox]_
2. benchmarks [#benchmarks-repo]_
3. tracker [#tracker-repo]_
4. peps [#peps-repo]_
5. devguide [#devguide-repo]_
6. cpython [#cpython-repo]_

The devinabox and benchmarks repositories are code-only. The peps
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
devinabox, benchmarks, and tracker repositories to move to
GitHub. While devinabox has a sufficiently descriptive name, the
benchmarks repository does not, hence it will be named
"python-benchmark-suite". The tracker repo also has a misleading name
and will be renamed "bugs.python.org".

Create a 'python-dev' team
''''''''''''''''''''''''''
To manage permissions, a 'python-dev' team will be created as part of
the python organization [#github-python-org]_. Any repository that is
moved will have the 'python-dev' team added to it with write
permissions [#github-org-perms]_. Anyone who previously had rights to
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
A key part of any open source project is making sure that its source
code can be properly licensed. This requires making sure all people
making contributions have signed a contributor license agreement
(CLA) [#cla]_. Up until now, enforcement of CLA signing of
contributed code has been enforced by core developers checking
whether someone had an ``*`` by their username on
bugs.python.org [#b.p.o]_. With this migration the plan is to start
off with automated checking and enforcement of contributors signing
the CLA.

Adding GitHub username support to bugs.python.org
+++++++++++++++++++++++++++++++++++++++++++++++++
To keep tracking of CLA signing under the direct control of the PSF,
tracking who has signed the PSF CLA will be continued by marking that
fact as part of someone's bugs.python.org user profile. What this
means is that an association will be needed between a person's
bugs.python.org [#b.p.o]_ account and their GitHub account, which
will be done through a new field in a user's profile.

A bot to enforce CLA signing
++++++++++++++++++++++++++++
With an association between someone's GitHub account and their
bugs.python.org [#b.p.o]_ account which has the data as to whether
someone has signed the CLA, a bot can monitor pull requests on
GitHub and denote whether the contributor has signed the CLA.

If the user has signed the CLA, the bot will add a positive label to
the issue to denote the pull request has no CLA issues (e.g., a green
label stating, "CLA: ✓"). If the contributor has not signed a CLA,
a negative label will be added to the pull request will be blocked
using GitHub's status API (e.g., a red label stating, "CLA: ✗").
Using a label for both positive and negative cases provides a
fallback notification if the bot happens to fail, preventing
potential false-positives or false-negatives. It also allows for an
easy way to trigger the bot again by simply removing a CLA-related
label.

Requirements for Web-Related Repositories
----------------------------------------
Due to their use for generating webpages, the devguide [#devguide]_
and peps [#peps]_ repositories need their respective processes
updated to pull from their new Git repositories.

The devguide repository might also need to be named
``python-devguide`` to make sure the repository is not ambiguous
when viewed in isolation from the
python organization [#github-python-org]_.

Requirements for the cpython Repository
---------------------------------------
Obviously the most active and important repository currently hosted
at hg.python.org [#h.p.o]_ is the cpython
repository [#cpython-repo]_. Because of its importance and high-
frequency use, it requires more tooling before being moved to GitHub
compared to the other repositories mentioned in this PEP.

Document steps to commit a pull request
'''''''''''''''''''''''''''''''''''''''
During the process of choosing a new development workflow, it was
decided that a linear history is desired. What this means is that the
convenient "Merge" button in GitHub pull requests is undesireable as
it creates a merge commit along with all of the contributor's
individual commits (this does not affect the other repositories where
the desire for a linear history isn't nearly as strong).

Luckily Git [#git]_ does not require GitHub's workflow and so one can
be chosen which gives us a linear history by using Git's CLI. The
expectation is that all pull requests will be fast-forwarded and
rebased before being pushed to the master repository. This should
keep proper attribution to the pull request author in the Git
history.

A second set of recommended commands will also be written for
committing a contribution from a patch file uploaded to
bugs.python.org [#b.p.o]_. This will obviously help keep the linear
history, but it will need to be made to have attribution to the patch
creator.

The exact sequence of commands that will be given as guidelines to
core developers is an open issue:
`Git CLI commands for committing a pull request to cpython`_.

Handling ``Misc/NEWS``
''''''''''''''''''''''
XXX https://pypi.python.org/pypi/towncrier http://bugs.python.org/issue18967

Handling ``Misc/ACKS``
''''''''''''''''''''''
XXX

Linking pull requests to issues
'''''''''''''''''''''''''''''''
XXX https://hg.python.org/lookup/

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

Backup of pull request data
'''''''''''''''''''''''''''
XXX

Optional, Planned Features
--------------------------
XXX https://wiki.python.org/moin/TrackerDevelopmentPlanning

Bot to handle pull request merging
''''''''''''''''''''''''''''''''''
XXX must handle multiple branches, news entry, commit message
XXX commit queue
XXX naming

Continuous integration per pull request
'''''''''''''''''''''''''''''''''''''''
XXX https://travis-ci.org/ https://codeship.com/ https://circleci.com/

Test coverage report
''''''''''''''''''''
XXX https://coveralls.io/ https://coveralls.io/

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

Status
======
XXX list all sections (w/ backlinks) stating the status

Open Issues
===========
For this PEP, open issues are ones where a decision needs to be made
to how to approach or solve a problem. Open issues do not entail
coordination issues such as who is going to write a certain bit of
code.

The fate of hg.python.org
-------------------------
XXX

Tools and commands to move from Mercurial to Git
------------------------------------------------
A decision needs to be made on exactly what tooling and what commands
involving those tools will be used to convert a Mercurial repository
to Git. Currently a suggestion has been made to use
https://github.com/frej/fast-export.

Git CLI commands for committing a pull request to cpython
---------------------------------------------------------
Because Git [#git]_ may be a new version control system for core
developers, the commands people are expected to run will need to be
written down. These commands also need to keep a linear history while
giving proper attribution to the pull request author.

Another set of commands will also be necessary for when working with
a patch file uploaded to bugs.python.org [#b.p.o]_. Here the linear
history will be kept implicitly, but it will need to make sure to
keep/add attribution.

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

.. [#devinabox] Mercurial repository for devinabox (https://hg.python.org/devinabox/)

.. [#peps-repo] Mercurial repository of the Python Enhancement Proposals (https://hg.python.org/peps/)

.. [#tracker-repo] bugs.python.org code repository (https://hg.python.org/tracker/python-dev/)

.. [#devguide-repo] Mercurial repository for the Python Developer's Guide (https://hg.python.org/devguide/)

.. [#cpython-repo] Mercurial repository for CPython (https://hg.python.org/cpython/)

.. [#github-python-org] Python organization on GitHub (https://github.com/python)

.. [#github-org-perms] GitHub repository permission levels
   (https://help.github.com/enterprise/2.4/user/articles/repository-permission-levels-for-an-organization/)

.. [#cla] Python Software Foundation Contributor Agreement (https://www.python.org/psf/contrib/contrib-form/)

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
