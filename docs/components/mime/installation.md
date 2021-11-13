# Installation

## Swift

Download and install [Swift](https://swift.org/download) 5.3 or higher.

## Swift Package Manager

### New Package

This creates a new executable package named `MyPackage`.

```shell
mkdir MyPackage
cd MyPackage
swift package init --type executable
```

### Package Manifest

Add the `MIME` package as a dependency by specifying its `name`, `url`, and `version`.

```swift
dependencies: [
    .package(name: "chaqmoq-mime", url: "https://github.com/chaqmoq/mime.git", from: "1.0.0")
]
```

Add the `MIME` target to your desired target as a dependency by specifying its `name` and `package` where you want to import and use it. In this case, we are adding it to the default target named `MyPackage` generated using the `swift package init` command we executed earlier.

```swift
targets: [
    .target(name: "MyPackage", dependencies: [
        .product(name: "MIME", package: "chaqmoq-mime")
    ])
]
```

### Build

This builds the `MyPackage` and installs the `MIME` package with the `Debug` configuration needed for development.

```shell
swift build
```

Once you are ready to deploy, you can run the command below that builds the `MyPackage` and installs the `MIME` package with the `Release` configuration optimized for production.

```shell
swift build -c release
```

## Xcode Project

1. Open your `Xcode` project.
2. Go to `File` -> `Swift Packages` -> `Add Package Dependency`.
3. Enter the `MIME` package URL: `https://github.com/chaqmoq/mime.git`.
4. Choose the version you want to install. We suggest installing the latest stable version by using the [Up to Next Major](https://developer.apple.com/documentation/swift_packages/package/dependency/requirement/2878218-uptonextmajor) strategy.
5. Add the `MIME` package to your desired target of the `MyPackage` where you want to import and use it.
