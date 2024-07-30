This Project is in development and does not work!

# Goal

- Create a simple importable Python module which will produce parsed
   WHOIS data for a given domain.
- Able to extract data for all the popular TLDs (com, org, net, ...)
- Query a WHOIS server directly instead of going through an
   intermediate web service like many others do.

# Example

```python {"id":"01J4265RHTMKMNPQ29DKWXJ89S"}
>>> import whois
>>> w = whois.whois('example.com')
>>> w.expiration_date  # dates converted to datetime object
datetime.datetime(2022, 8, 13, 4, 0)
>>> w.text  # the content downloaded from whois server
u'\nDomain Name: EXAMPLE.COM
Registry Domain ID: 2336799_DOMAIN_COM-VRSN
...'

>>> print(w)  # print values of all found attributes    
{
  "creation_date": "1995-08-14 04:00:00",
  "expiration_date": "2022-08-13 04:00:00",
  "updated_date": "2021-08-14 07:01:44",
  "domain_name": "EXAMPLE.COM",
  "name_servers": [
      "A.IANA-SERVERS.NET",
      "B.IANA-SERVERS.NET"
  ],
  ...
```

# Install

Install from pypi:

```bash {"id":"01J4265RHTMKMNPQ29DKY6HJK2"}
pip install python-whois
```

Or checkout latest version from repository:

```bash {"id":"01J4265RHTMKMNPQ29DMZHCQ7N"}
git clone git@github.com:richardpenman/whois.git
pip install -r requirements.txt
```

Run test cases:

```bash {"id":"01J4265RHTMKMNPQ29DPY59XT9"}
python -m pytest
```

# Problems?

Pull requests are welcome!

Thanks to the many who have sent patches for additional TLDs. If you want to add or fix a TLD it's quite straightforward.
See example domains in [whois/parser.py](https://github.com/richardpenman/whois/blob/master/whois/parser.py)

Basically each TLD has a similar format to the following:

```python {"id":"01J4265RHTMKMNPQ29DS5AZHKB"}
class WhoisOrg(WhoisEntry):
  """Whois parser for .org domains
  """
  regex = {
    'domain_name':      'Domain Name: *(.+)',
    'registrar':        'Registrar: *(.+)',
    'whois_server':     'Whois Server: *(.+)',
    ...
  }

  def __init__(self, domain, text):
    if text.strip() == 'NOT FOUND':
      raise PywhoisError(text)
    else:
      WhoisEntry.__init__(self, domain, text)
```
