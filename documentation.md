Documentation
===

- [HttpServer](#httpserver)
    * [LoadMimes(file)](#loadmimesfile)
    * [GetMimeType(file)](#getmimetypefile)
    * [ServeFile(ByRef response, file)](#servefilebyref-response-file)
    * [SetPaths(paths)](#setpathspaths)
    * [Serve(port)](#serveport)
    * [Handle(ByRef request)](#handlebyref-request)
    * [CheckPartialPathMatch(path, wildcardPaths)](#checkpartialpathmatchpath-wildcardpaths)

- [HttpRequest](#httprequest)
    * [Headers](#headers)
    * [Queries](#queries)
    * [Path](#path)
    * [Method](#method)
    * [Protocol](#protocol)
    * [IsMultipart](#ismultipart)
    * [Done](#done)

- [HttpResponse](#httpresponse)
    * [Headers](#headers-1)
    * [Protocol](#protocol-1)
    * [SetBody(ByRef body, length)](#setbodybyref-body-length)
    * [SetBodyText(text)](#setbodytexttext)
    * [Generate](#generate)


HttpServer
---

#### LoadMimes(file)
Loads mime types from a file.
Returns false if file not found.
```
// Example mime file
text/html                             html htm shtml
text/css                              css
```

#### GetMimeType(file)
Returns the mime type for the given file.
Defaults to ```"text/plain"```

#### ServeFile(ByRef response, file)
Sets the response body to the data of the given file and attempts to set the correct ```Content-Type``` header if it's present in the loaded mime types.

```autohotkey
FileServer(ByRef request, ByRef response, ByRef server) {
    server.ServeFile(response, filePath)
    response.status := 200
}
```

#### SetPaths(paths)
Takes a `paths` object which connects routes with controllers. Routes can be specific or wildcards.

The server will look for an exact path match first, if none can be found, all wildcard paths will be tested and the longest matching path will be used.

```autohotkey
paths := {}
paths["/"] := Func("RootPage")
paths["/article"] := Func("ArticlePage")
paths["/wildcard/*"] := Func("WildcardPage")

RootPage(ByRef request, ByRef response, ByRef server) {
    response.SetBodyText("Hello World")
    response.status := 200
}

ArticlePage(ByRef request, ByRef response, ByRef server) {
    response.SetBodyText("Articles...")
    response.status := 200
}

WildcardPage(ByRef request, ByRef response, ByRef server) {
    response.SetBodyText(request.path)
    response.status := 200
}
```

#### Serve(port)
Starts the http server on the specified port.

#### Handle(ByRef request)
Used internally to handle requests.

#### CheckPartialPathMatch(path, wildcardPaths)
Used internally to check if a path can be matched against the wildcard paths.

HttpRequest
---

#### Headers
Array containing headers from request.
```autohotkey
request.headers["Some-Header"]
```

#### Queries
Array containing queries from request.
```autohotkey
; request -> /dog?name=frank
msgbox % request.queries["name"] ; will display frank
```

#### Path
The path being requested.
```autohotkey
request.path ; /pages/1
```

#### Method
Method of request.
```autohotkey
request.method ; GET
```

#### Protocol
Protocol of request.
```autohotkey
request.protocol
```

#### IsMultipart
Returns true if the request is a multipart request.
```autohotkey
MultiPart(ByRef req, ByRef res) {
    if (req.IsMultipart() && !req.done)
        res.status := 100
        return
    }
    
    msgbox % req.body
    res.status := 200
}
```

#### Done
Whether or not a multipart request is done being sent.


HttpResponse
---

#### Headers
Array containing headers for response.
```autohotkey
response.headers["Date"] := "Today"
```

#### Protocol
Protocol of response.
```autohotkey
response.protocol
```

#### SetBody(ByRef body, length)
Sets the responses body.
```
body - data for body of response
length - length of data
```

#### SetBodyText(text)
Sets the responses body as text.

#### Generate
Returns a buffer that contains the responses data.
