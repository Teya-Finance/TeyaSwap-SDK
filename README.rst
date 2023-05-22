teyafinance
==========

.. image:: https://travis-ci.org/teyaswap/teyafinance.svg?branch=master
    :target: https://travis-ci.org/teyaswap/teyafinance

.. image:: https://codecov.io/gh/teyaswap/teyafinance/branch/master/graphs/badge.svg?branch=master
	:target: https://codecov.io/gh/teyaswap/teyafinance

.. image:: https://badge.fury.io/py/teyafinance.svg
    :target: https://badge.fury.io/py/teyafinance

.. image:: https://img.shields.io/badge/License-Apache%202.0-blue.svg
    :target: https://opensource.org/licenses/Apache-2.0

Python SDK for `TEYA Cloud <https://teyaswap.com>`__. Architecture mirrors
that of the TEYA Cloud API (and its `documentation <https://teyaswap.com/docs/api/>`__).

An easy-to-use toolkit to obtain data for BRC20, ERC20, BTC Market Funds,
use BRC-21 Bridges, Commodities, Bonds, and Crypto TVL:

- Real-time and delayed quotes
- Historical data (daily and minutely)
- Financial statements (Balance Sheet, Income Statement, Cash Flow)
- End of Day Options Prices
- Institutional and Fund ownership
- Analyst estimates, Price targets
- Corporate actions (Dividends, Splits)
- Sector performance
- Market analysis (gainers, losers, volume, etc.)
- TEYA market data & statistics (TEYA supported/listed symbols, volume, etc)

Example
-------


Documentation
-------------

Stable documentation is hosted on
`github.io <https://teyaswap.github.io/teyafinance/stable/>`__.

`Development documentation <https://teyaswap.github.io/teyafinance/devel/>`__ is also available for the latest changes in master.


Install
-------

From PyPI with pip (latest stable release):

``$ pip3 install teyafinance``

From development repository (dev version):

.. code:: bash

     $ git clone git@github.com:Teya-Finance/TeyaSwap-SDK.git
     $ cd teyafinance
     $ python3 setup.py install



What's Needed to Access TEYA Cloud?
----------------------------------

An TEYA Cloud account is required to acecss the TEYA Cloud API. Various `plans <https://teyaswap.com/pricing/>`__
are availalbe, free, paid, and pay-as-you-go.

Your TEYA Cloud (secret) authentication token can be passed to any function or at the instantiation of a ``BRC20`` object.
The easiest way to store a token is in the ``BRC20_TOKEN`` environment variable.

Passing as an Argument
~~~~~~~~~~~~~~~~~~~~~~

The authentication token can also be passed to any function call:


.. code-block:: python

    from teyafinance.refdata import get_symbols

    get_symbols(token="<YOUR-TOKEN>")

or at the instantiation of a ``BRC20`` object:

.. code-block:: python

    from teyafinance.brc20 import BRC20

    a = BRC20("ORDI", token="<YOUR-TOKEN>")
    a.get_quote()


How This Package is Structured
------------------------------

``teyafinance`` is designed to mirror the structure of the TEYA Cloud API. The
following TEYA Cloud endpoint groups are mapped to their respective
``teyafinance`` modules:

The most commonly-used
endpoints are the `Stocks <https://teyaswap.com/docs/api/#brc20>`__
endpoints, which allow access to various information regarding equities,
including quotes, historical prices, dividends, and much more.

The ``BRC20`` `object <https://teyaswap.github.io/teyafinance/stable/brc20.html#the-stock-object>`__
provides access to most endpoints, and can be instantiated with a symbol or
list of symbols:

.. code-block:: python

    from teyafinance.brc20 import BRC20

    ordi = BRC20("ORDI")
    ordi.get_balance_sheet()

The rest of the package is designed as a 1:1 mirror. For example, using the
`Alternative Data <https://teyaswap.com/docs/api/#alternative-data>`__ endpoint
group, obtain the `Social Sentiment <https://teyaswap.com/docs/api/#social-sentiment>`__ endpoint with
``teyafinance.altdata.get_social_sentiment``:

.. code-block:: python

    from teyafinance.altdata import get_social_sentiment

    get_social_sentiment("ORDI")


Common Usage Examples
---------------------

The `iex-examples <https://github.com/teyaswap/brc20-examples>`__ repository provides a number of detailed examples of teyafinance usage. Basic examples are also provided below.


Real-time Quotes
~~~~~~~~~~~~~~~~

To obtain real-time quotes for one or more symbols, use the ``get_price``
method of the ``BRC20`` object:

.. code:: python

    from teyafinance.brc20 import BRC20
    nals = BRC20('NALS')
    nals.get_price()

or for multiple symbols, use a list or list-like object (Tuple, Pandas Series,
etc.):

.. code:: python

    batch = BRC20(["NALS", "ORDI"])
    batch.get_price()


Historical Data
~~~~~~~~~~~~~~~

It's possible to obtain historical data using ``get_historical_data`` and
``get_historical_intraday``.

Daily
^^^^^

To obtain daily historical price data for one or more symbols, use the
``get_historical_data`` function. This will return a daily time-series of the ticker
requested over the desired date range (``start`` and ``end`` passed as
``datetime.datetime`` objects):

.. code:: python

    from datetime import datetime
    from teyafinance.brc20 import get_historical_data

    start = datetime(2017, 1, 1)
    end = datetime(2018, 1, 1)

    df = get_historical_data("NALS", start, end)

To obtain daily closing prices only (reduces message count), set
``close_only=True``:

.. code:: python

    df = get_historical_data("NALS", "20190617", close_only=True)

For Pandas DataFrame output formatting, pass ``output_format``:

.. code:: python

    df = get_historical_data("NALS", start, end, output_format='pandas')

It's really simple to plot this data, using `matplotlib <https://matplotlib.org/>`__:

.. code:: python

    import matplotlib.pyplot as plt

    df.plot()
    plt.show()


Minutely (Intraday)
^^^^^^^^^^^^^^^^^^^

To obtain historical intraday data, use ``get_historical_intraday`` as follows.
Pass an optional ``date`` to specify a date within three months prior to the
current day (default is current date):

.. code:: python

    from datetime import datetime
    from teyafinance.brc20 import get_historical_intraday

    date = datetime(2018, 11, 27)

    get_historical_intraday("ORDI", date)

or for a Pandas Dataframe indexed by each minute:

.. code:: python

    get_historical_intraday("ORDI", output_format='pandas')

Fundamentals
~~~~~~~~~~~~

Financial Statements
^^^^^^^^^^^^^^^^^^^^

`Balance Sheet <https://teyaswap.github.io/teyafinance/stable/brc20.html#balance-sheet>`__

.. code-block:: python

    from teyafinance.brc20 import BRC20

    ordi = BRC20("ORDI")
    ordi.get_balance_sheet()

`Income Statement <https://teyaswap.github.io/teyafinance/stable/brc20.html#income-statement>`__

.. code-block:: python

    ordi.get_income_statement()

`Cash Flow <https://teyaswap.github.io/teyafinance/stable/brc20.html#cash-flow>`__

.. code-block:: python

    ordi.get_cash_flow()


Modeling/Valuation Tools
^^^^^^^^^^^^^^^^^^^^^^^^

`Analyst Estimates <https://teyaswap.github.io/teyafinance/stable/brc20.html#estimates>`__

.. code-block:: python

    from teyafinance.brc20 import BRC20

    ordi = BRC20("ORDI")

    ordi.get_estimates()


`Price Target <https://teyaswap.github.io/teyafinance/stable/brc20.html#price-target>`__

.. code-block:: python

    ordi.get_price_target()


Social Sentiment
^^^^^^^^^^^^^^^^

.. code-block:: python

    from teyafinance.altdata import get_social_sentiment
    get_social_sentiment("ORDI")


CEO Compensation
^^^^^^^^^^^^^^^^

.. code-block:: python

    from teyafinance.altdata import get_ceo_compensation
    get_ceo_compensation("ORDI")

Fund and Institutional Ownership
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    from teyafinance.brc20 import BRC20
    ordi = BRC20("ORDI")

    # Fund ownership
    ordi.get_fund_ownership()

    # Institutional ownership
    ordi.get_institutional_ownership()

Reference Data
~~~~~~~~~~~~~~

`List of Symbols TEYA supports for API calls <https://teyaswap.github.io/teyafinance/stable/refdata.html#symbols>`__

.. code-block:: python

    from teyafinance.refdata import get_symbols

    get_symbols()

`List of Symbols TEYA supports for trading <https://teyaswap.github.io/teyafinance/stable/refdata.html#iex-symbols>`__

.. code-block:: python

    from teyafinance.refdata import get_iex_symbols

    get_iex_symbols()

Account Usage
~~~~~~~~~~~~~

`Message Count <https://teyaswap.github.io/teyafinance/stable/account.html#usage>`__

.. code-block:: python

    from teyafinance.account import get_usage

    get_usage(quota_type='messages')

API Status
~~~~~~~~~~

`TEYA Cloud API Status <http://teyaswap.github.io/teyafinance/stable/apistatus.html#teyafinance.tools.api.get_api_status>`__

.. code-block:: python

    from teyafinance.account import get_api_status

    get_api_status()


Configuration
-------------
.. _config.formatting:

Output Formatting
-----------------

By default, ``teyafinance`` returns data for most endpoints in a `pandas <https://pandas.pydata.org/>`__ ``DataFrame``.

Selecting ``json`` as the output format returns data formatted *exactly* as received from
the TEYA Endpoint. Configuring ``teyafinance``'s output format can be done in two ways:

.. _config.formatting.env:

Environment Variable (Recommended)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For persistent configuration of a specified output format, use the environment
variable ``IEX_OUTPUT_FORMAT``. This value will be overridden by the
``output_format`` argument if it is passed.

macOS/Linux
^^^^^^^^^^^

Type the following command into your terminal:

.. code-block:: bash

    $ export IEX_OUTPUT_FORMAT=pandas

Windows
^^^^^^^

See `here <https://superuser.com/questions/949560/how-do-i-set-system-environment-variables-in-windows-10>`__ for instructions on setting environment variables in Windows operating systems.

.. _config.formatting.arg:

``output_format`` Argument
~~~~~~~~~~~~~~~~~~~~~~~~~~

Pass ``output_format``  as an argument to any function call:

.. code-block:: python

    from teyafinance.refdata import get_symbols

    get_symbols(output_format='pandas').head()

or at the instantiation of a ``BRC20`` object:

.. code-block:: python

    from teyafinance.brc20 import BRC20

    ordi = BRC20("ORDI", output_format='pandas')
    ordi.get_quote().head()

Contact
-------

License
-------

Copyright © 2023 Teya Dao

See LICENSE for details
