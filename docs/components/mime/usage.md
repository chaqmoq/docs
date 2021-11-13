# Usage

## Initialization

Creating a new instance of `MIME` type without any arguments results in `application/octet-stream` without a file extension.

```swift linenums="1"
import MIME

let mime = MIME()
print(mime) // "application/octet-stream"
print(mime.ext) // nil
```

If you provide valid values to the `type` and `subtype` arguments whose combination exists in the [supported MIME types](/components/mime/overview/#supported-mime-types), it creates the corresponding `MIME` type with the appropriate default file extension. Otherwise, it falls back to `application/octet-stream` without a file extension.

```swift linenums="1"
import MIME

let mime = MIME(type: "text", subtype: "html")
print(mime) // "text/html"
print(mime.ext) // "html"
```

You can also combine the `type` and `subtype` arguments together.

```swift linenums="1"
import MIME

let mime = MIME("application/java-archive")
print(mime) // "application/java-archive"
print(mime.ext) // "jar"
```

There are quite a lot of `MIME` types that support multiple file extensions. The default one is chosen if you don't provide a valid value to the `ext` argument as a hint. Providing an invalid file extension leads to falling back to the default file extension.

```swift linenums="1"
import MIME

let mime = MIME("application/java-archive", ext: "war") // Defaults to `jar`
print(mime) // "application/java-archive"
print(mime.ext) // "war"
```

A new instance of `MIME` can also be instantiated with the `ext` argument only that can automatically set the `type` and `subtype` properties. If the value is not a valid file extension, it falls back to `application/octet-stream` without a file extension.

```swift linenums="1"
import MIME

let mime = MIME(ext: "css")
print(mime) // "text/css"
print(mime.ext) // "css"
```

Last but not least, you can initialize a new instance of `MIME` type with the `path` or `url` arguments that work the same way as the `ext` argument.

```swift linenums="1"
import MIME

let mime = MIME(path: "/public/js/main.js")
print(mime) // "text/javascript"
print(mime.ext) // "js"
```

```swift linenums="1"
import MIME

let mime = MIME(url: URL(string: "https://chaqmoq.dev/public/img/logo.png")!)
print(mime) // "image/png"
print(mime.ext) // "png"
```

## Guessing

`MIME` types can be guessed from `bytes` or `Data`. See the full list of all [guessable MIME types](/components/mime/overview/#guessable-mime-types) for more info.

### Bytes

```swift linenums="1"
import MIME

let bytes: [UInt8] = [0x47, 0x49, 0x46, ...]
let mime = MIME.guess(from: bytes)
print(mime) // "image/gif"
print(mime.ext) // "gif"
```

### Data

```swift linenums="1"
import MIME

let data = Data([0xFF, 0xD8, 0xFF, ...])
let mime = MIME.guess(from: data)
print(mime) // "image/jpeg"
print(mime.ext) // "jpg"
```
