.. image:: https://pypip.in/version/trading_ig/badge.svg
    :target: https://pypi.python.org/pypi/trading_ig/
    :alt: Latest Version

.. image:: https://pypip.in/py_versions/trading_ig/badge.svg
    :target: https://pypi.python.org/pypi/trading_ig/
    :alt: Supported Python versions

.. image:: https://pypip.in/format/trading_ig/badge.svg
    :target: https://pypi.python.org/pypi/trading_ig/
    :alt: Download format

.. image:: https://pypip.in/license/trading_ig/badge.svg
    :target: https://pypi.python.org/pypi/trading_ig/
    :alt: License

.. image:: https://pypip.in/status/trading_ig/badge.svg
    :target: https://pypi.python.org/pypi/trading_ig/
    :alt: Development Status

.. image:: https://sourcegraph.com/api/repos/github.com/femtotrader/ig-markets-rest-api-python-library/.badges/status.png
   :target: https://sourcegraph.com/github.com/femtotrader/ig-markets-rest-api-python-library

.. image:: https://badges.gitter.im/Join%20Chat.svg
   :target: https://gitter.im/femtotrader/ig-markets-rest-api-python-library?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge

.. image:: https://travis-ci.org/femtotrader/ig-markets-rest-api-python-library.svg?branch=master
    :target: https://travis-ci.org/femtotrader/ig-markets-rest-api-python-library

IG Markets REST API - Python Library
====================================

A lightweight Python library that can be used to connect to the IG
Markets REST API with a LIVE or DEMO account.

You can use the IG Markets HTTP / REST API to submit trade orders, open
positions, close positions and view market sentiment. IG Markets provide
Retail Spread Betting and CFD accounts for trading Equities, Forex,
Commodities, Indices and much more.

Full details about the API along with information about how to open an
account with IG can be found at the link below:

http://labs.ig.com/

How To Use The Library
----------------------

Using this library to connect to the IG Markets API is extremely easy.
All you need to do is import the IGService class, create an instance,
and call the methods you wish to use. There is a method for each
endpoint exposed by their API. The code sample below shows you how to
connect to the API, switch to a secondary account and retrieve all open
positions for the active account.

**Note:** The secure session with IG is established when you create an
instance of the IGService class.

.. code:: python

    from trading_ig import IGService
    from trading_ig_config import config

    ig_service = IGService(config.username, config.password, config.api_key, config.acc_type)
    ig_service.create_session()

    account_info = ig_service.switch_account(config.acc_number, False) # not necessary
    print(account_info)

    open_positions = ig_service.fetch_open_positions()
    print("open_positions:\n%s" % open_positions)

    print("")

    epic = 'CS.D.EURUSD.MINI.IP'
    resolution = 'D'
    num_points = 10
    response = ig_service.fetch_historical_prices_by_epic_and_num_points(epic, resolution, num_points)
    df_ask = response['prices']['ask']
    print("ask prices:\n%s" % df_ask)

with ``trading_ig_config.py``

.. code:: python

    class config(object):
        username = "YOUR_USERNAME"
        password = "YOUR_PASSWORD"
        api_key = "YOUR_API_KEY"
        acc_type = "DEMO" # LIVE / DEMO
        acc_number = "ABC123"

Config can also be set as environment variable

.. code:: bash

    export IG_SERVICE_USERNAME="..."
    export IG_SERVICE_PASSWORD="..."
    export IG_SERVICE_API_KEY="..."
    export IG_SERVICE_ACC_TYPE="DEMO" # LIVE / DEMO
    export IG_SERVICE_ACC_NUMBER="..."

and we can get it using

.. code:: python

    from trading_ig import ConfigEnvVar
    config = ConfigEnvVar("IG_SERVICE")

it should display:

::

    open_positions:
    Empty DataFrame
    Columns: []
    Index: []

    ask prices:
                            Open     High      Low    Close
    DateTime
    2014:11:18-00:00:00  1.24510  1.25465  1.24442  1.25330
    2014:11:19-00:00:00  1.25332  1.26013  1.25127  1.25461
    2014:11:20-00:00:00  1.25463  1.25760  1.25048  1.25427
    2014:11:21-00:00:00  1.25428  1.25689  1.23755  1.23924
    2014:11:23-00:00:00  1.23640  1.23770  1.23607  1.23725
    2014:11:24-00:00:00  1.23864  1.24453  1.23830  1.24390
    2014:11:25-00:00:00  1.24389  1.24877  1.24026  1.24743
    2014:11:26-00:00:00  1.24744  1.25322  1.24443  1.25077
    2014:11:27-00:00:00  1.25078  1.25244  1.24569  1.24599
    2014:11:28-00:00:00  1.24598  1.24909  1.24269  1.24505

Many IGService methods return `Python
Pandas <http://pandas.pydata.org/>`__ DataFrame, Series or Panel.

Cache queries requests-cache
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set CachedSession using:

.. code:: python

    from datetime import datetime, timedelta
    import requests_cache
    session = requests_cache.CachedSession(cache_name='cache', backend='sqlite', expire_after=timedelta(hours=1))
    # set expire_after=None if you don't want cache expiration
    # set expire_after=0 if you don't want to cache queries

CachedSession can be applied globally on IGService

.. code:: python

    ig_service = IGService(config.username, config.password, config.api_key, config.acc_type, session)
    ig_service.create_session()

or just for a given method (like fetching prices)

.. code:: python

    epic = 'CS.D.EURUSD.MINI.IP'
    resolution = 'D'
    start_date = '2014-12-15'
    end_date = '2014-12-20'
    response = ig_service.fetch_historical_prices_by_epic_and_date_range(epic, resolution, start_date, end_date, session)
    

Install
-------

From Python package index
~~~~~~~~~~~~~~~~~~~~~~~~~

::

    $ pip install trading_ig

From source
~~~~~~~~~~~

Get latest version using Git

::

    $ git clone https://github.com/femtotrader/ig-markets-rest-api-python-library.git
    $ cd ig-markets-rest-api-python-library
    $ python setup.py install
