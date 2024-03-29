# neadva
Python 3 implementation of Hungarian Tax Office (NAV) version 3 reporting api
-----------
0.52 Alpha version update of the module. Can be used to supply data and return transaction numbers safely (working).
Suggest to check the invoices before submitting in the test interface of NAV API first

More work required for full interface support of NAV API.

Clone the repo to install:

`git clone https://github.com/ServerAstra/neadva`

or install using pip:

`pip install neadva`

Overall library architecture at the moment allows one-liners as well as full object-oriented approach.

Every interface class allows overrides for configuration and necessary options, they also provide result items as a dictionary and as properties simultaneously allowing automated processing and ease of use via interpreter console.

In the future we will also implement additional interface for familiar ORM like methods.

Unfortunately the limitations of NAV API and its documentation prevent this from becoming a priority so this has been postponed for the time being.

Usage:
-----------
```python
    from neadva import Invoice, Transaction, SubmitInvoices, MapTaxNumber, Config
    Config.user = {"login": "login",
            "password": "password",
            "taxNumber": "12345678",
            "signKey": "12-345678-910111213141516",
            "exchangeKey": "12345678910"}
    Config.software = {"softwareId": "HU12345678RANDOM",
                "softwareName": "None",
                "softwareOperation": "LOCAL_SOFTWARE",
                "softwareMainVersion": "0",
                "softwareDevName": "NAV Kft.",
                "softwareDevContact": "nun@nil.com"}
    taxrequestdata = MapTaxNumber('12345678')()
    print(taxrequestdata)
    print(taxrequestdata.items())
    import glob
    invoices = SubmitInvoices()
    for invoice in glob.glob('Invoices/*.xml'):
        print(invoice)
        with open(invoice, 'r') as f:
            invoices.append(Invoice(f.read()))
    sentinvoices = invoices()
    print(sentinvoices.transaction)
    print(sentinvoices.items())
```

or since v0.5

```python
    for invoice in glob.glob('Invoices/*.xml'):
        invoices.append(Invoice.fromfile(invoice))
    sentinvoices = invoices()
    print(sentinvoices.transaction)
    print(sentinvoices.items())
```
Optionally you can choose to not compress data:
```python
    Config.compression = False
```
Or go live with:
```python
    Config.live = True
```

Config class is global for all instances in the current interpreter run.
However you can supply an instance explicitly via `config = Config(**configvalues)` where configvalues has user dict and software dict and includes other non-default modifications for each separate class.

Warning
-----------
Tested only with test interface of NAV not production interface. Might still have some bugs due to differences in test and prod environment which happens sometimes

Modules required:
-----------
`pip install pycryptodome lxml pybase62 requests`

Todo:

* Support for transaction search
* Support for invoice search
* Single interface where you just use `neadva.taxinfo('12345678')` `neadva.upload(*files)` `neadva.download(from="2021-04", till="2021-10")` `neadva.status(*listofinvoices)`
* Support for annulments