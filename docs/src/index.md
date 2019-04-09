# LiveServer.jl - Documentation

LiveServer is a simple and lightweight development web-server written in Julia, based on [HTTP.jl](https://github.com/JuliaWeb/HTTP.jl).
It has live-reload capability, i.e. when changing files, every browser (tab) currently displaying a corresponding page is automatically refreshed.

LiveServer is inspired from Python's [`http.server`](https://docs.python.org/3/library/http.server.html) and Node's [`browsersync`](https://www.browsersync.io/).

## Installation

The package is currently un-registered.
In Julia ≥ 1.0, you can add it using the Package Manager writing

```
(v1.2) pkg> add https://github.com/asprionj/LiveServer.jl
```

## Usage

The main function `LiveServer` exports is `serve` which starts listening to the current folder and makes its content available to a browser.
The following code creates an example directory and serves it:

```julia
julia> using LiveServer
julia> LiveServer.example() # creates an "example/" folder with some files
julia> cd("example")
julia> serve() # starts the local server & the file watching
✓ LiveServer listening on http://localhost:8000...
  (use CTRL+C to shut down)
```

Open a Browser and go to `http://localhost:8000/` to see the content being rendered; try modifying files (such as `index.html`) to see the changes being rendered immediately in the browser.

## How it works

This is a rough overview of how this package works.
Let's assume you have a directory similar to that generated by `LiveServer.example()`:

```
.
├── css
│   └── master.css
├── files
│   ├── TestImage.gif
│   ├── TestImage.jpg
│   ├── TestImage.png
│   └── zip-file.zip
├── index.html
└── pages
    └── blah.html
```

After launching `serve()` in that directory, you may visit in your browser the page `http://localhost:8000/index.html`.
This initiates a GET request from the browser which is caught by a listener loop in LiveServer.
The requested file (here `index.html`) is then sent to the client after injection of a small script.
This small script will trigger a reload of the page upon receiving a message to do so.

If you now modify the page `index.html`, LiveServer will detect that the file has been modified and send a message to the small script above to trigger the reload.