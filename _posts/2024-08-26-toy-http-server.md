---
layout: post
title: "Toy HTTP Server"
---

<div style="text-align: center;">
  <img src="{{ site.url }}/images/ToyHTTPServerExample.png" width="50%"/>
</div>
<br>

Check out the project on GitHub: [https://github.com/daneelsan/ToyHTTPServer](https://github.com/daneelsan/ToyHTTPServer)

Recently, I decided to dive into the world of networking by building a simple HTTP server from scratch. The goal was to understand the basics of how web servers work and to implement one in multiple programming languages. The result is the **Toy HTTP Server**, a minimalistic server that handles GET requests and serves static files. It’s implemented in **Lua**, **Python**, **Wolfram Language**, and **Zig**.

### Why Build an HTTP Server?

HTTP servers are the backbone of the web, and while frameworks like Flask, Express, or Nginx make it easy to build and deploy web applications, I wanted to strip away the abstractions and see how things work at a lower level. By building a server from scratch, I gained a deeper appreciation for the simplicity and elegance of the HTTP protocol.

### Features

The Toy HTTP Server is a basic implementation that:
- Listens for incoming TCP connections on `localhost:8888`.
- Handles GET requests for static files (e.g., HTML, images).
- Responds with appropriate HTTP status codes (200 for success, 404 for not found).
- Supports multiple programming languages, each with its own implementation.

### Implementation Details

The server is built using simple socket functionality, which is available in most programming languages. Here’s a quick overview of how it works in each language:

#### Lua
Lua’s lightweight nature makes it a great choice for scripting. I used the `luasocket` library to handle networking and `mimetypes` to determine the correct content type for responses.

Here’s a snippet of the Lua implementation:

```lua
local function http_handle_GET(request)
    local filename = request.uri:sub(2) -- Remove the leading '/'
    local response_line, response_headers, response_body

    local file = io.open(filename, "rb")
    if file ~= nil then
        response_line = http_response_line(200)
        local content_type = mimetypes.guess(filename) or "text/html"
        response_headers = http_response_headers({ ["Content-Type"] = content_type })
        response_body = file:read("*all")
        file:close()
    else
        response_line = http_response_line(404)
        response_headers = http_response_headers()
        response_body = "<h1>404 Not Found</h1>"
    end

    local blank_line = "\r\n"
    return table.concat({ response_line, response_headers, blank_line, response_body })
end
```

#### Python
Python’s `socket` module provides low-level networking capabilities. The implementation is straightforward, with the server listening for connections and parsing incoming HTTP requests.

Here’s a snippet of the Python implementation:
```python
def handle_GET(self, request):
    filename = request.uri.strip("/")  # remove the slash from the request URI
    if os.path.exists(filename):
        response_line = self.response_line(status_code=200)
        content_type = mimetypes.guess_type(filename)[0] or "text/html"
        response_headers = self.response_headers({"Content-Type": content_type})
        with open(filename, "rb") as f:
            response_body = f.read()
    else:
        response_line = self.response_line(status_code=404)
        response_headers = self.response_headers()
        response_body = b"<h1>404 Not Found</h1>"
    blank_line = b"\r\n"
    response = b"".join([response_line, response_headers, blank_line, response_body])
    return response
```

#### Wolfram Language
The Wolfram Language is not typically associated with networking, but it has powerful built-in functions like `SocketListen` and `SocketOpen`.

Here’s a snippet of the Wolfram Language implementation:
```
handleMethod["GET"][request_] :=
    Module[{fileName, statusCode, body, headers = <||>},
        fileName = FileNameJoin[{".", request["PathString"]}];
        If[fileName =!= "." && FileExistsQ[fileName],
            statusCode = 200;
            headers["Content-Type"] = First[Import[fileName, "MIMEType"], "TEXT/HTML"];
            body = ReadByteArray[fileName];
            ,
            statusCode = 404;
            body = "<h1>404 Not Found</h1>";
        ];
        HTTPResponse[
            body,
            <|
                "StatusCode" -> statusCode,
                $defaultResponseHeaders,
                headers
            |>
        ]
    ];
```

#### Zig
Zig is a modern systems programming language that emphasizes simplicity and performance. The Zig implementation uses its standard library for networking, showcasing Zig’s potential for building low-level systems software. Fun fact: although it's the only compiled language in this list, the number of lines of zig code wasn't the largest.

Here’s a snippet of the Zig implementation:
```zig
fn handle_GET(http_request: HTTPRequest, conn: std.net.Server.Connection) !void {
    var local_path: []const u8 = undefined;
    if (std.mem.eql(u8, http_request.uri, "/")) {
        local_path = "index.html";
    } else {
        local_path = http_request.uri[1..];
    }
    const file = std.fs.cwd().openFile(local_path, .{}) catch |err| switch (err) {
        error.FileNotFound => {
            const response = "HTTP/1.1 404 NOT FOUND \r\n" ++
                "Server: ToyServer (zig)\r\n" ++
                "Content-Type: text/html; charset=utf8\r\n" ++
                "Content-Length: 9\r\n" ++
                "\r\n" ++
                "NOT FOUND";
            _ = try conn.stream.writer().print(response, .{});
            return;
        },
        else => return err,
    };
    defer file.close();
    const mime = guess_mime_type(local_path);
    const memory = std.heap.page_allocator;
    const maxSize = std.math.maxInt(usize);
    const file_contents = try file.readToEndAlloc(memory, maxSize);
    const response = "HTTP/1.1 200 OK \r\n" ++
        "Server: ToyServer (zig)\r\n" ++
        "Content-Type: {s}\r\n" ++
        "Content-Length: {}\r\n" ++
        "\r\n";
    _ = try conn.stream.writer().print(response, .{ mime, file_contents.len });
    _ = try conn.stream.writer().write(file_contents);
}
```

### Testing the Server
You can test the server using `curl` or a web browser. Here’s an example of a successful request:
```shell
$ curl -i 127.0.0.1:8888/index.html
HTTP/1.1 200 OK
content-type: text/html;charset=UTF-8

<html>
    <head>
        <title>Index page</title>
    </head>
    <body>
        <h1>Index page</h1>
        <p>This is the index page.</p>
        <img src="./images/random.jpeg">
    </body>
</html>
```
And here’s what happens when a file is not found:
```shell
$ curl -i --http0.9 127.0.0.1:8888/notafile.txt
HTTP/1.1 404 Not Found
Server: ToyServer (python)
Content-Type: text/html

<h1>404 Not Found</h1>
```

### Challenges and Learnings
Building this server was a good learning experience. I did encounter some challenges:

* Handling different MIME types for files.
* Parsing HTTP requests correctly.
* Ensuring cross-language consistency in functionality.

One interesting issue I encountered was with Safari, which didn’t handle the server’s responses as expected. It's important to test across different clients!

### Future Improvements
While the server is functional, there’s always room for improvement:

* Add support for more HTTP methods (e.g., POST, PUT).
* Implement multithreading to handle multiple requests simultaneously.
* Improve error handling and logging.
* Add support for dynamic content (e.g., CGI scripts).

### Conclusion

Building this HTTP server was a great way to learn how networking and web protocols work. It also let me try out different programming languages and see their strengths. If you want to understand web servers better, I recommend building one yourself - it’s a solid learning experience.

Links you might be interested in:

* [GitHub Repository](https://github.com/daneelsan/ToyHTTPServer)
* [LuaSocket Documentation](https://lunarmodules.github.io/luasocket/)
* [Python Socket Documentation](https://docs.python.org/3/library/socket.html)
* [Wolfram Language Networking Functions](http://reference.wolfram.com/language/guide/Networking.html)
* [Zig Standard Library Documentation](https://ziglang.org/documentation/master/std/)