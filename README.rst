teyafinance
==========

.. image:: https://travis-ci.org/addisonlynch/teyafinance.svg?branch=master
    :target: https://travis-ci.org/addisonlynch/teyafinance

.. image:: https://codecov.io/gh/addisonlynch/teyafinance/branch/master/graphs/badge.svg?branch=master
	:target: https://codecov.io/gh/addisonlynch/teyafinance

.. image:: https://badge.fury.io/py/teyafinance.svg
    :target: https://badge.fury.io/py/teyafinance

.. image:: https://img.shields.io/badge/License-Apache%202.0-blue.svg
    :target: https://opensource.org/licenses/Apache-2.0

Python SDK for `TEYA Cloud <https://teyaswap.com>`__. Architecture mirrors
that of the TEYA Cloud API (and its `documentation <https://teyaswap.com/docs/api/>`__).

An easy-to-use toolkit to obtain data for Stocks, ETFs, Mutual Funds,
Forex/Currencies, Options, Commodities, Bonds, and Cryptocurrencies:

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
- Social Sentiment and CEO Compensation

Example
-------

.. image:: https://raw.githubusercontent.com/addisonlynch/teyafinance/master/docs/source/images/iexexample.gif


Documentation
-------------

Stable documentation is hosted on
`github.io <https://addisonlynch.github.io/teyafinance/stable/>`__.

`Development documentation <https://addisonlynch.github.io/teyafinance/devel/>`__ is also available for the latest changes in master.


Install
-------

From PyPI with pip (latest stable release):

``$ pip3 install teyafinance``

From development repository (dev version):

.. code:: bash

     $ git clone https://github.com/addisonlynch/teyafinance.git
     $ cd teyafinance
     $ python3 setup.py install



What's Needed to Access TEYA Cloud?
----------------------------------

An TEYA Cloud account is required to acecss the TEYA Cloud API. Various `plans <https://teyaswap.com/pricing/>`__
are availalbe, free, paid, and pay-as-you-go.

Your TEYA Cloud (secret) authentication token can be passed to any function or at the instantiation of a ``Stock`` object.
The easiest way to store a token is in the ``IEX_TOKEN`` environment variable.

Passing as an Argument
~~~~~~~~~~~~~~~~~~~~~~

The authentication token can also be passed to any function call:


.. code-block:: python

    from teyafinance.refdata import get_symbols

    get_symbols(token="<YOUR-TOKEN>")

or at the instantiation of a ``Stock`` object:

.. code-block:: python

    from teyafinance.stocks import Stock

    a = Stock("AAPL", token="<YOUR-TOKEN>")
    a.get_quote()


How This Package is Structured
------------------------------

``teyafinance`` is designed to mirror the structure of the TEYA Cloud API. The
following TEYA Cloud endpoint groups are mapped to their respective
``teyafinance`` modules:

The most commonly-used
endpoints are the `Stocks <https://teyaswap.com/docs/api/#stocks>`__
endpoints, which allow access to various information regarding equities,
including quotes, historical prices, dividends, and much more.

The ``Stock`` `object <https://addisonlynch.github.io/teyafinance/stable/stocks.html#the-stock-object>`__
provides access to most endpoints, and can be instantiated with a symbol or
list of symbols:

.. code-block:: python

    from teyafinance.stocks import Stock

    aapl = Stock("AAPL")
    aapl.get_balance_sheet()

The rest of the package is designed as a 1:1 mirror. For example, using the
`Alternative Data <https://teyaswap.com/docs/api/#alternative-data>`__ endpoint
group, obtain the `Social Sentiment <https://teyaswap.com/docs/api/#social-sentiment>`__ endpoint with
``teyafinance.altdata.get_social_sentiment``:

.. code-block:: python

    from teyafinance.altdata import get_social_sentiment

    get_social_sentiment("AAPL")


Common Usage Examples
---------------------

The `iex-examples <https://github.com/addisonlynch/iex-examples>`__ repository provides a number of detailed examples of teyafinance usage. Basic examples are also provided below.


Real-time Quotes
~~~~~~~~~~~~~~~~

To obtain real-time quotes for one or more symbols, use the ``get_price``
method of the ``Stock`` object:

.. code:: python

    from teyafinance.stocks import Stock
    tsla = Stock('TSLA')
    tsla.get_price()

or for multiple symbols, use a list or list-like object (Tuple, Pandas Series,
etc.):

.. code:: python

    batch = Stock(["TSLA", "AAPL"])
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
    from teyafinance.stocks import get_historical_data

    start = datetime(2017, 1, 1)
    end = datetime(2018, 1, 1)

    df = get_historical_data("TSLA", start, end)

To obtain daily closing prices only (reduces message count), set
``close_only=True``:

.. code:: python

    df = get_historical_data("TSLA", "20190617", close_only=True)

For Pandas DataFrame output formatting, pass ``output_format``:

.. code:: python

    df = get_historical_data("TSLA", start, end, output_format='pandas')

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
    from teyafinance.stocks import get_historical_intraday

    date = datetime(2018, 11, 27)

    get_historical_intraday("AAPL", date)

or for a Pandas Dataframe indexed by each minute:

.. code:: python

    get_historical_intraday("AAPL", output_format='pandas')

Fundamentals
~~~~~~~~~~~~

Financial Statements
^^^^^^^^^^^^^^^^^^^^

`Balance Sheet <https://addisonlynch.github.io/teyafinance/stable/stocks.html#balance-sheet>`__

.. code-block:: python

    from teyafinance.stocks import Stock

    aapl = Stock("AAPL")
    aapl.get_balance_sheet()

`Income Statement <https://addisonlynch.github.io/teyafinance/stable/stocks.html#income-statement>`__

.. code-block:: python

    aapl.get_income_statement()

`Cash Flow <https://addisonlynch.github.io/teyafinance/stable/stocks.html#cash-flow>`__

.. code-block:: python

    aapl.get_cash_flow()


Modeling/Valuation Tools
^^^^^^^^^^^^^^^^^^^^^^^^

`Analyst Estimates <https://addisonlynch.github.io/teyafinance/stable/stocks.html#estimates>`__

.. code-block:: python

    from teyafinance.stocks import Stock

    aapl = Stock("AAPL")

    aapl.get_estimates()


`Price Target <https://addisonlynch.github.io/teyafinance/stable/stocks.html#price-target>`__

.. code-block:: python

    aapl.get_price_target()


Social Sentiment
^^^^^^^^^^^^^^^^

.. code-block:: python

    from teyafinance.altdata import get_social_sentiment
    get_social_sentiment("AAPL")


CEO Compensation
^^^^^^^^^^^^^^^^

.. code-block:: python

    from teyafinance.altdata import get_ceo_compensation
    get_ceo_compensation("AAPL")

Fund and Institutional Ownership
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    from teyafinance.stocks import Stock
    aapl = Stock("AAPL")

    # Fund ownership
    aapl.get_fund_ownership()

    # Institutional ownership
    aapl.get_institutional_ownership()

Reference Data
~~~~~~~~~~~~~~

`List of Symbols TEYA supports for API calls <https://addisonlynch.github.io/teyafinance/stable/refdata.html#symbols>`__

.. code-block:: python

    from teyafinance.refdata import get_symbols

    get_symbols()

`List of Symbols TEYA supports for trading <https://addisonlynch.github.io/teyafinance/stable/refdata.html#iex-symbols>`__

.. code-block:: python

    from teyafinance.refdata import get_iex_symbols

    get_iex_symbols()

Account Usage
~~~~~~~~~~~~~

`Message Count <https://addisonlynch.github.io/teyafinance/stable/account.html#usage>`__

.. code-block:: python

    from teyafinance.account import get_usage

    get_usage(quota_type='messages')

API Status
~~~~~~~~~~

`TEYA Cloud API Status <http://addisonlynch.github.io/teyafinance/stable/apistatus.html#teyafinance.tools.api.get_api_status>`__

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

or at the instantiation of a ``Stock`` object:

.. code-block:: python

    from teyafinance.stocks import Stock

    aapl = Stock("AAPL", output_format='pandas')
    aapl.get_quote().head()

Contact
-------

License
-------

Copyright © 2023 Teya Dao

See LICENSE for details