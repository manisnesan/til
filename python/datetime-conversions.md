# Datetime Conversions using python datetime module and fastcore

## datetime

- datetime module provides utility functions such as strptime, strftime. 
- These functions can help to parse the date provided as string and also helps to emit it in the desired date format.

## fastcore

- xtras module provides the handy utc to local time conversions making life easier.

### Examples

```python
from fastcore.all import *
from datetime import datetime

def utc2est(utc: str, format_str: str='%Y-%m-%dT%H:%M:%SZ')-> str:
    """ Takes the UTC time in string format and convert into EST """
    default_format_str='%Y-%m-%dT%H:%M:%SZ'
    utc_timestamp = datetime.strptime(utc, format_str)
    est_timestamp = utc2local(utc_timestamp)
    return est_timestamp.strftime(default_format_str)

def est2utc(est: str, format_str: str='%Y-%m-%dT%H:%M:%SZ') -> str:
    """ Takes the EST time in string format and convert into UTC """
    default_format_str='%Y-%m-%dT%H:%M:%SZ'
    est_timestamp = datetime.strptime(est, format_str)
    utc_timestamp = local2utc(est_timestamp)
    return utc_timestamp.strftime(default_format_str)
```
