===============================
pycrunchbase
===============================

| |docs| |travis| |coveralls|
| |version| |downloads| |wheel| |supported-versions| |supported-implementations|

.. |docs| image:: https://readthedocs.org/projects/pycrunchbase/badge/?style=flat
    :target: https://readthedocs.org/projects/pycrunchbase
    :alt: Documentation Status

.. |travis| image:: http://img.shields.io/travis/ngzhian/pycrunchbase/master.png?style=flat
    :alt: Travis-CI Build Status
    :target: https://travis-ci.org/ngzhian/pycrunchbase

.. |coveralls| image:: https://coveralls.io/repos/ngzhian/pycrunchbase/badge.svg
    :target: https://coveralls.io/r/ngzhian/pycrunchbase

.. |version| image:: http://img.shields.io/pypi/v/pycrunchbase.png?style=flat
    :alt: PyPI Package latest release
    :target: https://pypi.python.org/pypi/pycrunchbase

.. |downloads| image:: http://img.shields.io/pypi/dm/pycrunchbase.png?style=flat
    :alt: PyPI Package monthly downloads
    :target: https://pypi.python.org/pypi/pycrunchbase

.. |wheel| image:: https://pypip.in/wheel/pycrunchbase/badge.png?style=flat
    :alt: PyPI Wheel
    :target: https://pypi.python.org/pypi/pycrunchbase

.. |supported-versions| image:: https://pypip.in/py_versions/pycrunchbase/badge.png?style=flat
    :alt: Supported versions
    :target: https://pypi.python.org/pypi/pycrunchbase

.. |supported-implementations| image:: https://pypip.in/implementation/pycrunchbase/badge.png?style=flat
    :alt: Supported imlementations
    :target: https://pypi.python.org/pypi/pycrunchbase

Python bindings to CrunchBase

pycrunchbase v0.3.0 has support for CrunchBase API version 3, but things are still flaky, so any kind of
bug reports is greatly appreciated, for detail see notes below.

Examples
========

Initialize the API using your API Key, will throw ValueError if missing

::

    cb = CrunchBase(API_KEY)

Look up an organization by name

::

    github = cb.organization('github')

The response contains snippets of data regarding relationships
that the organization has, an example is the funding_rounds

::

    funding_rounds_summary = github.funding_rounds

All relationships are paged, and only 8 is returned initially
to get more data do this, it handles paging for you
and returns a False-y value if there are no more pages

::

    more_funding_rounds = cb.more(funding_rounds_summary)

Data in relations are just summaries, and you probably want more details
For example funding_rounds returns 5 values: type, name, path
created_at, updated_at.

If you actually want to know who invested, you have to get to make
more API calls.

First get the uuid of the round

::

    round_uuid = funding_rounds_summary[0].uuid

Then use the CrunchBase API to make that call

::

    round = cb.funding_round(round_uuid)

Again, investments is a relationship on a FundingRound,
so we can get the first item in that relationship

::

    an_investor = round.investments[0]  # a InvestorInvestmentPageItem

And printing that gives us the name of the investor, and the amount
invested in USD

::

    print(str(an_investor))  # prints: Investor Name $100000


Installation
============

::

    pip install pycrunchbase

Documentation
=============

https://pycrunchbase.readthedocs.org/

Development
===========

To run the all tests run::

    tox

Contributions are always welcome!

Use `GitHub issues <https://github.com/ngzhian/pycrunchbase/issues>`_
to report a bug or send feedback.

The best way to send feedback is to file an issue at https://github.com/ngzhian/pycrunchbase/issues.

Contributors
============

Thanks to these contributors:

* `dustinfarris <https://github.com/dustinfarris>`_

Goals
=====

1. Support all (or almost all) of CrunchBase's API functionalities
2. Speedy updates when CrunchBase's API changes
3. 'Pythonic' bindings, user doesn't feel like we're requesting URLs

Notes on CrunchBase version 3 changes
=====================================

In version 3, CrunchBase changed the names of some endpoints, e.g person -> people, and they
have gone with the plural form of all entities. pycrunchbase does not adhere strictly to that.
For example, there is still a `person` method, but a `people` method is also provided
so that it remains backwards compatible and also supports methods that matches the name
of the entity.


License
=======

MIT
