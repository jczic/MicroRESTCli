## MicroRESTCli is a micro HTTP JSON REST Web client based on [MicroWebCli](http://microwebcli.hc2.fr) for MicroPython (used on ESP32 and [Pycom](http://www.pycom.io) modules)

![HC²](hc2.png "HC²")

Very easy to integrate and very light with 2 files only :
- `"microRESTCli.py"` - The REST client
- `"microWebCli.py"` - The base Web client ([available here](http://microwebcli.hc2.fr))

Simple but effective :
- Use it to requesting fastly HTTP JSON REST endpoints (Web APIs)
- Supports secured connections *SSL/TLS over HTTP* (https)
- Supports authentications *HTTP Basic* and *Bearer Token*
- Automatically bounces on HTTP(S) redirections (ressources moved)
- Adds helpers to convert datetime formats between timestamp and json
- Coulds save to a file the response content of any REST request

### Using *microRESTCli* main class :

| Name  | Function |
| - | - |
| Constructor | `rCli = MicroRESTCli(baseUrl, user=None, password=None, token=None)` |
| Makes a GET request | `rCli.GET(resUrl, fileToSave=None, progressCallback=None)` |
| Makes a POST request | `rCli.POST(resUrl, o, fileToSave=None, progressCallback=None)` |
| Makes a PUT request | `rCli.PUT(resUrl, o, fileToSave=None, progressCallback=None)` |
| Makes a PATCH request | `rCli.PATCH(resUrl, o, fileToSave=None, progressCallback=None)` |
| Makes a DELETE request | `rCli.DELETE(resUrl, fileToSave=None, progressCallback=None)` |
| Gets status code of last call | `rCli.GetLastStatusCode()` |
| Gets status message of last call | `rCli.GetLastStatusMessage()` |
| Gets json response object of last call | `rCli.GetLastJSONResponse()` |

| Name  | Property |
| - | - |
| Gets or sets the connection timeout in secondes | `rCli.ConnTimeoutSec` |
| Gets or sets the dict headers of http requests | `rCli.Headers` |

### Simple example :
```python
from microRESTCli import MicroRESTCli

rCli = MicroRESTCli('https://my-web-api.io/rest', user='toto', password='blah123')

invoice = rCli.GET('invoice/%d' % 42)

try :
  u = { 'name':'geek', 'age':18 }
  resp = rCli.POST('user', u)
  print('User added')
except :
  err = rCli.GetLastJSONResponse()
  if err :
    print("%s API request failed (%s)" % (err.name, err.reason))
```

### Example of saving response to a file :
```python
from microRESTCli import MicroRESTCli

def progressCallback(microWebCli, progressSize, totalSize) :
  if totalSize :
    print('Progress: %d bytes of %d downloaded...' % (progressSize, totalSize))
  else :
    print('Progress: %d bytes downloaded...' % progressSize)

rCli        = MicroRESTCli('https://my-web-api.io/rest', token='jIud87jzIsUzj3=')
filename    = '/flash/invoice.pdf'
contentType = rCli.GET('invoice/%d/pdf' % 42, filename, progressCallback)
print('File of type "%s" saved to "%s"' % (contentType, filename))
```

### Using *microRESTCli* helpers :

| Name  | Function |
| - | - |
| Converts timestamp to json datetime | `MicroRESTCli.Timestamp2JsonDT(timestamp=None)` |
| Converts json datetime to timestamp | `MicroRESTCli.JsonDT2Timestamp(jsonDT)` |


### By JC`zic for [HC²](https://www.hc2.fr) ;')

*Keep it simple, stupid* :+1:
