GrockTX
=======

[![Build Status](https://travis-ci.org/3flex/grocktx.svg?branch=master)](https://travis-ci.org/3flex/grocktx)

GrockTX is a library for parsing and scraping bank transaction data.  Its
primary function is to obtain and parse the memo strings that appear on bank
and credit card statements such as:

    11-14-09 HAMILTON TRUE VALUE HD DORCHESTER MA auth# 86673

GrockTX provides one main library: ``grocktx.parser``.

* ``grocktx.parser`` parses transaction memo strings.  It picks the
  transaction apart to identify the channel (e.g. point-of-sale, check,
  transfer, etc.), the city, state, phone number and zip (if available), and
  the descriptive portion (e.g. "HAMILTON TRUE VALUE HD").

``grocktx.parser`` has no external dependencies beyond python 2.5.

Installation
============

Install using ``python setup.py install``.  You may use with ``pip`` with the
following requirements entry:

    -e git://github.com/3flex/grocktx.git#egg=grocktx

grocktx.parser
~~~~~~~~~~~~~~

``grocktx.parser`` defines one public method::

    parse(memo, approx_date=None)

``memo`` is a memo string, such as appears on bank and credit card statements.
``approx_date`` is an optional parameter to use to interpret dates on memo
strings that lack year indicators, such as this one:

    WITHDRAW#  - POS 1128 1756 531470 HARVEST COOP CAMBRIDGE MA

If ``approx_date`` is not provided, the year which makes the date closest to
now will be used.  

Example::

    .. code-block:: python

    >>> from grocktx.parser import parse
    >>> parse("11-14-09 HAMILTON TRUE VALUE HD DORCHESTER MA auth# 86673")
    {
        'channel': 'pos',
        'channel_details': {
            'auth': "86673",
            'auth_date': "2009-11-14"
        },
        'vendor': {
            'description': "HAMILTON TRUE VALUE HD",
            'city': "DORCHESTER",
            'state': "MA",
            'zip': "",
            'phone': ""
        }
    }

For more examples of the supported transaction formats and their return
results, see ``grocktx/tests.py``.
