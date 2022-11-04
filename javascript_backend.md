---
layout: default
title: JavaScript backend
---

# JavaScript backend

The JavaScript backend doesn’t yet have user documentation, but there is a paper
on the JS FFI available
[here](https://www.iro.umontreal.ca/~feeley/papers/BelangerFeeleyELS21.pdf). To
get you started here is a minimal example:

```shell
$ cat hello.scm
(declare (standard-bindings) (extended-bindings))
(define (alert msg) (##inline-host-statement "alert(@scm2host@(@1@))" msg))
(alert "hello!")
$ gsc -target js -exe -o hello.html hello.scm
$ open hello.html
```

Here’s a slightly longer example showing off the SIX FFI:

```shell
$ cat sixffi.scm
(include "~~lib/_six/js#.scm") ;; get six.infix macro
(define Date \Date)
(define now (Date))
(define el \document.createElement("div"))
\(`el).innerHTML=`now
\document.body.appendChild(`el)
```

And here is how you can get a minimal REPL which supports the SIX FFI:

```shell
$ cat jsrepl.scm
(include "~~lib/_six/six-expand.scm")
(##namespace (""))
(eval '(##define-syntax six.infix _six/six-expand#six.infix-js-expand))
(##repl-debug-main)
$ gsc/gsc -:= -target js -exe -o jsrepl.html jsrepl.scm
$ open jsrepl.html
```
