# Usage

## Environment files

### Reading

Here is how we can read an environment file named `.env` from the current directory by using the `readFile(at:)` method.

```swift linenums="1"
import DotEnv

let env = DotEnv()
let path = "./.env"

do {
    let file = try env.readFile(at: path)
    print(file)
} catch {
    print(error)
}
```

When reading an environment file, the `encoding` is set to `utf8` by default. We can change it by providing a different value to the second argument like `utf16`.

```swift linenums="6"
do {
    let file = try env.readFile(at: path, encoding: .utf16)
    print(file)
} catch {
    print(error)
}
```

### Parsing

Parsing an environment file and extracting environment variables are done in a similar way by using `parseFile(at:)` method.

```swift linenums="6"
do {
    let variables = try env.parseFile(at: path)
    print(variables)
} catch {
    print(error)
}
```

You can read and parse an environment file separately if you want.

```swift linenums="6"
do {
    let file = try env.readFile(at: path)
    let variables = try env.parseFile(file)
    print(variables)
} catch {
    print(error)
}
```

### Loading

Loading an environment file means reading and parsing its content and setting the extracted environment variables in one go.

```swift linenums="6"
do {
    try env.load(at: path)
} catch {
    print(error)
}
```

Sometimes, you may want to define an environment file programmatically without creating one in the file system and load its environment variables.

```swift linenums="1"
import DotEnv

let env = DotEnv()
let file: File = """
DATABASE_USER=root
DATABASE_PASSWORD=password
"""

do {
    try env.load(file)
} catch {
    print(error)
}
```

### Caching

By default, `DotEnv` tries to cache an environment file using its `path` property as a `cache key`. But, when we create an environment file programmatically without saving it in the file system, it uses its content as a `cache key`. Caching an environment file happens when we read or parse it for the first time. Later calls to the same environment file return the cached file or variables based on which method we call. We can clear the cache by calling the `clearCache()` method.

```swift linenums="1"
import DotEnv

let env = DotEnv()
let path = "./.env"

do {
    print(try env.readFile(at: path)) // Reads the environment file and caches it
    print(try env.readFile(at: path)) // Returns the environment file from the cache
    env.clearCache() // Clears all cached environment files and variables
    print(try env.readFile(at: path)) // Reads the environment file and caches it
} catch {
    print(error)
}
```

```swift linenums="6"
do {
    print(try env.parseFile(at: path)) // Parses the environment file and caches it and its variables
    print(try env.parseFile(at: path)) // Returns the environment variables from the cache
    env.clearCache() // Clears all cached environment files and variables
    print(try env.parseFile(at: path)) // Parses the environment file and caches it and its variables
} catch {
    print(error)
}
```

## Environment variables

### Setting and getting

We can set an environment variable and retrieve its value by using the `set(_:forKey:overwrite:)` and `get(_:encoding:)` methods respectively. By default, the `set(_:forKey:overwrite:)` overwrites the value of an environment variable if it already exists. If it is not something you want, provide `false` to the `overwrite` argument.

```swift linenums="1"
import DotEnv

let env = DotEnv()

env.set("root", forKey: "DATABASE_USER")
print(env.get("DATABASE_USER")) // Prints Optional("root")

env.set("admin", forKey: "DATABASE_USER", overwrite: false)
print(env.get("DATABASE_USER")) // Prints Optional("root")
```

`DotEnv` also supports setting and getting an environment variable with the `subscript` method. It is important to note that this method either sets or overwrites the value of the existing environment variable.

```swift linenums="1"
import DotEnv

let env = DotEnv()
env["DATABASE_USER"] = "admin"
print(env["DATABASE_USER"]) // Prints Optional("admin")
```

It is also possible to set multiple environment variable at once with the `set(_:overwrite:)` method.

```swift linenums="1"
import DotEnv

let env = DotEnv()
env.set([
    "DATABASE_DRIVER": "postgres",
    "DATABASE_USER": "root",
    "DATABASE_PASSWORD": "password"
])
print(env.get("DATABASE_DRIVER")) // Prints Optional("postgres")
print(env.get("DATABASE_USER")) // Prints Optional("root")
print(env.get("DATABASE_PASSWORD")) // Prints Optional("password")
```

### Unsetting and resetting

Sometimes, you may want to delete one or all user-defined environment variables. The `unset(_:)` and `reset()` methods can help with that.

```swift linenums="1"
import DotEnv

let env = DotEnv()
env.set([
    "DATABASE_DRIVER": "postgres",
    "DATABASE_USER": "root",
    "DATABASE_PASSWORD": "password"
])

env.unset("DATABASE_DRIVER")
print(env.get("DATABASE_DRIVER")) // Prints nil
print(env.get("DATABASE_USER")) // Prints Optional("root")
print(env.get("DATABASE_PASSWORD")) // Prints Optional("password")

env.reset()
print(env.get("DATABASE_DRIVER")) // Prints nil
print(env.get("DATABASE_USER")) // Prints nil
print(env.get("DATABASE_PASSWORD")) // Prints nil
```
