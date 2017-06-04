# Queries and quoting
## Unpacking query parameters
When you take a look at a URI for a major web service, you'll often see several query parameters, which are a sort of variable assignment that occurs after a ? in the URI. For instance, here's a Google Image Search URI:
```
https://www.google.com/search?q=gray+squirrel&tbm=isch
```
This will be sent to the web server as this HTTP request:
```
GET /search?q=gray+squirrel&tbm=isch HTTP/1.1
Host: www.google.com
```
The query part of the URI is the part after the ```?``` mark. Conventionally, query parameters are written as ```key=value``` and separated by ```&``` signs. So the above URI has two query parameters, ```q``` and ```tbm```, with the values ```gray+squirrel``` and ```isch```.

(isch stands for Image Search. I'm not sure what tbm means.)

There is a Python library called ```urllib.parse``` that knows how to unpack query parameters and other parts of an HTTP URL. (The library doesn't work on all URIs, only on some URLs.) Take a look at the [urllib.parse documentation here](https://docs.python.org/3/library/urllib.parse.html). Check out the ```urlparse``` and ```parse_qs``` functions specifically. Then try out this demonstration in your Python interpreter:

```python
>>> from urllib.parse import urlparse, parse_qs
>>> address = 'https://www.google.com/search?q=gray+squirrel&tbm=isch'
>>> parts = urlparse(address)
>>> print(parts)
ParseResult(scheme='https', netloc='www.google.com', path='/search', params='', query='q=gray+squirrel&tbm=isch', fragment='')
>>> print(parts.query)
q=gray+squirrel&tbm=isch
>>> query = parse_qs(parts.query)
>>> query
{'q': ['gray squirrel'], 'tbm': ['isch']}
```

## URL quoting
Did you notice that ```'gray+squirrel'``` in the query string became ```'gray squirrel'``` in the output of parse_qs? HTTP URLs aren't allowed to contain spaces or certain other characters. So if you want to send these characters in an HTTP request, they have to be translated into a "URL-safe" or "URL-quoted" format.

"**Quoting**" in this sense doesn't have to do with quotation marks, the kind you find around Python strings. It means translating a string into a form that doesn't have any special characters in it, but in a way that can be reversed (unquoted) later.

(And if that isn't confusing enough, it's sometimes also referred to as **URL-encoding** or **URL-escaping**).

One of the features of the URL-quoted format is that spaces are sometimes translated into plus signs. Other special characters are translated into hexadecimal codes that begin with the percent sign.

Take a look at the [documentation for urllib.parse.quote and related functions](https://docs.python.org/3/library/urllib.parse.html#url-quoting). Later in the course when you want to construct a URI in your code, you'll need to use appropriate quoting. More generally, whenever you're working on a web application and you find spaces or percent-signs in places you don't expect them to be, it means that something needs to be quoted or unquoted.