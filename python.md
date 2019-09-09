# Python cheat sheet

## Pad a number with zeros on the left

From [StackOverflow](http://stackoverflow.com/questions/339007/nicest-way-to-pad-zeroes-to-string)

Strings:

```python
>>> n = '4'
>>> print n.zfill(3)
>>> '004'
```

And for numbers:

```python
>>> n = 4
>>> print '%03d' % n
>>> 004
>>> print format(4, '03') # python >= 2.6
>>> 004
>>> print '{0:03d}'.format(4)  # python >= 2.6
>>> 004
>>> print('{0:03d}'.format(4))  # python 3
>>> 004
```
## Turn off warnings

Turn them off for a specific function:

```python
import warnings

def fxn():
    warnings.warn("deprecated", DeprecationWarning)

with warnings.catch_warnings():
    warnings.simplefilter("ignore")
    fxn()
```

Turn them off for everything (not condonded):

```python
import warnings
warnings.filterwarnings("ignore")
```

## Pretty-print JSON using IPython's pretty-printing

Here is the Python code:

```python
import json
from IPython.lib.pretty import pretty

with open(f"{folder}/A1-B000610-3_56_F-1-1_S28.sig") as f:
    signature_data = json.load(f)

print(pretty(signature_data, max_seq_length=10))
```

And the json output. Note: it is *not* valid JSON but it's enough for humans to get the idea:

```json
[{'class': 'sourmash_signature',
  'email': '',
  'filename': '/arg/2/0',
  'hash_function': '0.murmur64',
  'license': 'CC0',
  'name': 'A1-B000610-3_56_F-1-1_S28',
  'signatures': [{'abundances': [5, 1, 14, 32, 8, 2, 1, 1, 46, 1, ...],
    'ksize': 21,
    'max_hash': 0,
    'md5sum': '280b9babf31a3b7f820f9695f948808c',
    'mins': [321797371588609,
     224101973024771,
     113000222478351,
     1333158846758931,
     1102392644880415,
     587674942934704,
     586113718319139,
     278315472003108,
     875509323118629,
     399207939020840,
     ...],
    'molecule': 'DNA',
    'num': 500,
    'seed': 42},
   {'abundances': [7, 11, 1, 2, 1, 1, 2, 8, 5, 1, ...],
    'ksize': 31,
    'max_hash': 0,
    'md5sum': '9d82f6c550e83cdd97a9190e2fa7d1f0',
    'mins': [1098995281889281,
     494274597793794,
     805332503111685,
     944591797540874,
     433029384222735,
     777578192238617,
     815900320305157,
     729433322275847,
     593759110850609,
     823805458110523,
     ...],
    'molecule': 'DNA',
    'num': 500,
    'seed': 42},
   {'abundances': [1, 1, 1, 1, 8, 1, 11, 1, 1, 1, ...],
    'ksize': 51,
    'max_hash': 0,
    'md5sum': 'dd5c95634d6bd0eab9de215fd2bc6401',
    'mins': [947940305352714,
     98079127326732,
     970020576107181,
     108820536326160,
     652103673985041,
     562565376081938,
     714932558929943,
     703817730994207,
     65688630462498,
     679891240570919,
     ...],
    'molecule': 'DNA',
    'num': 500,
    'seed': 42}],
  'version': 0.4}]
```
