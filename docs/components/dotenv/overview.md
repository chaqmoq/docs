# Overview

Reads, parses, loads, and caches environment files and variables. Here is an example of the `.env` file in the current directory and all the possible types of content it can have and what they result in.

```
# Comment
EMTPY=
QUOTED="quoted"
QUOTED_WITH_WHITESPACE=" quoted with whitespace " # Trailing comment
MULTI_LINE="multi
line"
UNQUOTED=unquoted
UNQUOTED_WITH_WHITESPACE= unquoted with whitespace
DICTIONARY={"key": "value"}
PATH=/path/to
lowercased=lowercased
```

```swift
import DotEnv

let env = DotEnv()
let path = "./.env"

do {
    try env.load(at: path)
} catch {
    print(error)
}

print(env["EMTPY"]) // Prints Optional("")
print(env["QUOTED"]) // Prints Optional("quoted")
print(env["QUOTED_WITH_WHITESPACE"]) // Prints Optional(" quoted with whitespace ")
print(env["MULTI_LINE"]) // Prints Optional("multi\nline")
print(env["UNQUOTED"]) // Prints Optional("unquoted")
print(env["UNQUOTED_WITH_WHITESPACE"]) // Prints Optional("unquoted with whitespace")
print(env["DICTIONARY"]) // Prints Optional("{\"key\": \"value\"}")
print(env["PATH"]) // Prints Optional("/path/to")
print(env["lowercased"]) // Prints Optional("lowercased")
```
