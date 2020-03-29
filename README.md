# GWeb: golang + js + wasm

**gweb** -- strictly typed [WebAPI](https://en.wikipedia.org/wiki/Web_API) library on top of [syscall/js](https://golang.org/pkg/syscall/js/). Like [flow](https://github.com/facebook/flow) or [TypeScript](https://www.typescriptlang.org/) but for Go. You need it if you want to interact with browser from [wasm-compiled](https://github.com/golang/go/wiki/WebAssembly) Go program.

See examples on [gweb.orsinium.dev](https://gweb.orsinium.dev/).

## Features

+ **Strictly typed**. It's a wrapper around `syscall/js` that helps you to avoid runtime errors (you'll get them a lot with a raw `syscall/js`).
+ **Backward compatible**. Almost every type is a wrapper around `js.Value`. So, if something missed, you always can fallback to the classic `syscall/js` calls.
+ **Hand crafted**. It's hard to make usable autogeneration of WebAPI since Go is strictly typed language without union types. So we carefully transalted everything with applying Go best practices.
+ **Cleaned up**. The library provides only useful methods and attributes from WebAPI. No obsolete and deprecated methods, no experimental API that supported only by a few engines. Only what we really need right now.
+ **Almost the same API as in JS**. If you have a experience with [vanilla JS](https://stackoverflow.com/a/20435744), you almost learnt everything about the libray.
+ **But better**. WebAPI has a long history with incremental changes and spaces for unimplemented dreams. However, we can see the full picture to provide better experience and more namespaces.
+ **Documented**. Every method is documented to save your time and reduce googling.

## Installation

```bash
GOOS=js GOARCH=wasm go get github.com/life4/gweb
```

If you're using VSCode, it's recommend to create in your project `.vscode/settings.json` file with the following content:

```json
{
    "go.toolsEnvVars": {
        "GOARCH": "wasm",
        "GOOS": "js",
    },
    "go.testEnvVars": {
        "GOARCH": "wasm",
        "GOOS": "js",
    },
}
```

## Error handling

In the beautiful JS world anything at any time can be `null` or `undefined`. Check it when you're not sure:

```go
doc := web.GetWindow().Document()
el := doc.Element("some-element-id")
if el.Type() == js.TypeNull {
    // handle error
}
```

## Missed API

If something is missed, use `syscall/js`-like methods (`Get`, `Set`, `Call` etc):

```go
doc := web.GetWindow().Document()
el := doc.Element("some-element-id")
name = el.Get("name").String()
```

## Packages

GWeb is a collection of a few packages:

+ `web` ([docs](https://pkg.go.dev/github.com/life4/gweb/web?tab=doc)) -- window, manipulations with DOM.
+ `audio` ([docs](https://pkg.go.dev/github.com/life4/gweb/audio?tab=doc)) -- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API). Use `web.GetWindow().AudioContext()` as an entrypoint.
+ `canvas` ([docs](https://pkg.go.dev/github.com/life4/gweb/canvas?tab=doc)) -- canvas-related objects. Use `web.GetWindow().Document().CreateCanvas()` to get started.
+ `css` ([docs](https://pkg.go.dev/github.com/life4/gweb/css?tab=doc)) -- manage styles for HTML elements.

## Contributing

Contributions are welcome! GWeb is a Open-Source project and you can help to make it better. Some ideas what can be improved:

+ Every function and object should have short description based on [MDN Web API docs](https://developer.mozilla.org/en-US/docs/Web/API). Some descriptions are missed.
+ Also, every function that calls a Web API method should have a link in docs for that method.
+ Typos are very possible, don't be shy to fix it if you've spotted one.
+ More objects and methods? Of course! Our goal is to cover everything in WebAPI! Well, excluding deprecated things. See [Features](#features) section to get feeling what should be there.
+ Found a bug? Fix it!

And even if you don't have spare time for making PRs, you still can help by talking to your friends and subscribers about GWeb. Thank you :heart:

## Similar projects

+ [webapi](https://github.com/gowebapi/webapi/) -- bindings that autogenerated from [WebIDL](https://heycam.github.io/webidl/).
+ [godom](https://github.com/siongui/godom) -- bindings on top of [gopherjs](github.com/gopherjs/gopherjs/).
