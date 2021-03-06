AHKhttp
===

[![Semver](https://img.shields.io/badge/Semver-1.0.0-blue.svg?style=flat-square)](https://semver.org/)
[![AHK](https://img.shields.io/badge/AHK-v1.1.21.00-lightgrey.svg?style=flat-square)](https://autohotkey.com/download/)

Basic HTTP Server written in AutoHotkey.

Dependencies
---

Depends on the [AHKsock](https://github.com/jleb/AHKsock) lib by jleb.

Getting started
---

```autohotkey
; Assumes AHKhttp and AHKsock is located in /lib
#include <AHKhttp>

paths := {}
paths["/"] := Func("HelloWorld")

server := new HttpServer()
server.SetPaths(paths)
server.Serve(8000)
return

HelloWorld(ByRef req, ByRef res, ByRef server) {
    res.SetBodyText("Hello world!")
    res.status := 200
}
```

See a more advanced example in [`example.ahk`](examples/example.ahk).

Documentation
---

See the [API documentation](documentation.md).
