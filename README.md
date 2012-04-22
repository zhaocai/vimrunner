Using Vim's client/server functionality, this library exposes a way to spawn a
Vim instance and control it programatically. Apart from being a fun party
trick, this could be used to do integration testing on vimscript.

This is still fairly experimental, so use with caution. Any issue reports or
contributions are very welcome on the
[github issue tracker](https://github.com/AndrewRadev/Vimrunner/issues)

## Usage

There are two objects that can be used to control Vim, a "server" and a
"client". The server takes care of spawning a server vim instance that will be
controlled, and the client is its interface.

A server can be started in two ways:

  - `Vimrunner::Server.start`: Starts a terminal instance. Since it's
    "invisible", it's nice for use in automated tests. If it's impossible to
    start a terminal instance due to missing requirements for the `vim` binary
    (see "Requirements" below), a GUI instance will be started.
  - `Vimrunner::Server.start(:gui => true)`: Starts a GUI instance of Vim. On
    Linux, it'll be a `gvim`, on Mac it defaults to `mvim`.

Both methods return a `Server` instance. For a more comprehensive list of
options you can start the server with, check out
[the docs](http://rubydoc.info/gems/vimrunner/Vimrunner/Server).

To be able to send commands to the server, you need a client that knows some
details about it:

``` ruby
server = Vimrunner::Server.start

client = Vimrunner::Client.new(server)
# or,
client = server.new_client
```

There are also two convenience methods that start a server and return a
connected client -- `Vimrunner.start_vim` and `Vimrunner.start_gui_vim`.

For a full list of methods you can invoke on the remote vim instance, check out
the methods on the `Client` class in
[the docs](http://rubydoc.info/gems/vimrunner/Vimrunner/Client).

## Requirements

Vim needs to be compiled with `+clientserver`. This should be available with
the `normal`, `big` and `huge` featuresets. The client/server functionality
(regrettably) needs a running X server to function, even for a terminal vim.
This means that if you're using it for automated tests on a remote server,
you'll probably need to start it with xvfb.

## Experimenting

The `vimrunner` executable opens up an irb session with `$vim` set to a running
`gvim` (or `mvim`) client. You can use this for interactive experimentation. A
few things you can try:

``` ruby
$vim.edit 'some_file_name'  # edit a file
$vim.insert 'Hello, World!' # enter insert mode and write some text
$vim.normal 'T,'            # go back to the nearest comma
$vim.type 'a<cr>'           # append a newline after the comma
$vim.write                  # write file to disk
```
