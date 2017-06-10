# course-ud303

This repository contains downloadable exercise code for the Udacity course
"HTTP and Web Servers".  The exercises are written in portable Python 3 and
HTML.

# connect to localhost 8000
```
ncat 127.0.0.1 8000
```

# request in ncat
```
GET / HTTP/1.1
Host: localhost
```

# listen at port
```
ncat -l 8000
```

# response in ncat
```
HTTP/1.1 200 OK
Content-type: text/plain
Content-length: 50

Hello, browser! I am a real HTTP server, honestly!
```