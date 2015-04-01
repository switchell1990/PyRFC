PyRFC - The Python RFC Connector
================================

Description
-----------

The _pyrfc_ Python package provides Python bindings for _SAP NetWeaver RFC Library_, 
for a comfortable way of calling ABAP modules from Python and Python modules from ABAP, 
via SAP Remote Function Call (RFC) protocol.


Platforms & Prerequisites
-------------------------

The _pyrfc_ has been initially built with Python 2.6 and later enhanced, mostly used and tested with Python 2.7 and Python 3,
on Linux and Windows 64 and 32 bit platforms.

OS X and ARM platforms are currently not supported either, as _SAP NW RFC Library_ is not available for those platforms.

To start using _pyrfc_ you need to obtain _SAP NW RFC Library_ from _SAP Service Marketplace_,
following [these instructions](http://sap.github.io/PyRFC/install.html#install-c-connector).

A prerequisite to download is having a **customer or partner account** on _SAP Service Marketplace_ and if you
are SAP employee please check SAP OSS note [1037575 - Software download authorizations for SAP employees](http://service.sap.com/sap/support/notes/1037575).

_SAP NW RFC Library_ is fully backwards compatible, supporting all NetWeaver systems, from today, down to release R/3 4.0. 
You can therefore always use the newest version released on Service Marketplace and connect to older systems as well.


Usage examples
--------------

In order to call remote enabled ABAP function module (ABAP RFM), first a connection must be opened.

```python
>>> from pyrfc import Connection
>>> conn = Connection(ashost='10.0.0.1', sysnr='00', client='100', user='me', passwd='secret')
```

Using an open connection, we can invoke remote function calls (RFC).

```python
>>> result = conn.call('STFC_CONNECTION', REQUTEXT=u'Hello SAP!')
>>> print result
{u'ECHOTEXT': u'Hello SAP!',
 u'RESPTEXT': u'SAP R/3 Rel. 702   Sysid: ABC   Date: 20121001   Time: 134524   Logon_Data: 100/ME/E'}
```

Finally, the connection is closed automatically when the instance is deleted by the garbage collector. As this may take some time, we may either call the close() method explicitly or use the connection as a context manager:

```python
>>> with Connection(user='me', ...) as conn:
        conn.call(...)
    # connection automatically closed here
```

Alternatively, connection parameters can be provided as a dictionary, 
like in a next example, showing the connection via saprouter.

```python
>>> def get_connection(connmeta):
...     """Get connection"""
...     print 'Connecting ...', connmeta['ashost']
...     return Connection(**connmeta)

>>> from pyrfc import Connection

>>> SAPROUTER = '/H/111.22.33.44/S/3299/W/e5ngxs/H/555.66.777.888/H/'

>>> TEST = {
...    'user'      : 'me',
...    'passwd'    : 'secret',
...    'ashost'    : '10.0.0.1',
...    'saprouter' : SAPROUTER,
...    'sysnr'     : '00',
...    'client'    : '100',
...    'trace' : '3', #optional, in case you want to trace
...    'lang'      : 'EN'
... }

>>> conn = get_connection(TEST)
Connecting ... 10.0.0.1

>>>c.alive
True
```

Installation & Documentation
----------------------------

For further details on connection parameters, _pyrfc_ installation and usage, 
please refer to [_pyrfc_ documentation](http://sap.github.io/PyRFC), 
complementing _SAP NW RFC Library_ [programming guide and documentation](http://service.sap.com/rfc-library) 
provided on SAP Service Marketplace.
