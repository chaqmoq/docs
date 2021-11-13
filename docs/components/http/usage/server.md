# Usage

## Start and stop

```swift linenums="1"
import HTTP

let server = Server()
defer { server.stop() }
try server.start()
```

## Listen to events

```swift linenums="1"
import HTTP

let server = Server()
server.onStart = { _ in
    print("Server has started")
}
server.onStop = {
    print("Server has stopped")
}
server.onError = { error, _ in
    print("Error: \(error)")
}
defer { server.stop() }
try server.start()
```

## Response

### Return `String`

```swift linenums="1"
import HTTP

let server = Server()
server.onReceive = { _, _ in
    "Hello World"
}
defer { server.stop() }
try server.start()
```

### Return `Response`

```swift linenums="1"
import HTTP

let server = Server()
server.onReceive = { request, _ in
    Response("Hello World")
}
defer { server.stop() }
try server.start()
```

### Return `EventLoopFuture<String>`

```swift linenums="1"
import HTTP

let server = Server()
server.onReceive = { request, eventLoop in
    // Some async operation that returns EventLoopFuture<String>
    let promise = eventLoop.makePromise(of: String.self)
    eventLoop.execute {
        promise.succeed("Hello World")
    }

    return promise.futureResult
}
defer { server.stop() }
try server.start()
```

### Return `EventLoopFuture<Response>`

```swift linenums="1"
import HTTP

let server = Server()
server.onReceive = { request, eventLoop in
    // Some async operation that returns EventLoopFuture<Response>
    let promise = eventLoop.makePromise(of: Response.self)
    eventLoop.execute {
        promise.succeed(Response("Hello World"))
    }

    return promise.futureResult
}
defer { server.stop() }
try server.start()
```
