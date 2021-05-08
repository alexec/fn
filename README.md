# fn

A programming languge trying to design-out cost.

* Safe - is hard to write buggy code
* Fast - to build 
* Fast - to launch 
* Fast - to write - terse, avoids using keys at the edge of the keyboard, or needing shift: e.g. `{`, `}`, `:', `<`, `>`, `#` etc
* Compiles to standalone binary 
* GC free - predicatble executor and no need to tune GC 
* 1st-class mutable and immutable value objects
* 1st-class streams and functions
* 1st-class support for structs, specifically declaring structs 
* Un-lintable - i.e. the language syntax designs-out lint (which means whitespace is important).
* No comments?
* Parameters can be optional?
* Infered types?
* Public/private by default?

```
func main
  print "hello world"
```

```
import http
func get url:string
  r=[http.get "/api/"+url]
  if r.statusCode!=200
    throw "not OK"+r.statusCode
  return r.body
```

```
type httpRequest
  url string
  body []byte
  headers
    - name string
      value string
```

