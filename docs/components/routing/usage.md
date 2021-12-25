## Usage

```swift
import Routing

// RouteCollection
let routes = RouteCollection([
    Route(method: .GET, path: "/posts", name: "post_list") { _ in Response() }!,
    Route(method: .POST, path: "/posts", name: "post_create") { _ in Response() }!
])
let posts = routes.builder.grouped("/posts", name: "post_")!
posts.group("/{id<\\d+>}") { post in
    post.delete(name: "delete") { _ in Response() }
    post.get(name: "get") { _ in Response() }
    post.put(name: "update") { _ in Response() }
}
print(routes.count) // 4
print(routes[.DELETE].count) // 1
print(routes[.GET].count) // 2
print(routes[.POST].count) // 1
print(routes[.PUT].count) // 1

// Router
let router = Router(routes: routes)

// Resolving a Route
var route = router.resolveRouteBy(method: .GET, uri: "/posts")!
print(route.name) // "post_list"

route = router.resolveRoute(named: "post_get", parameters: ["id": "1"])!
print(route.name) // "post_get"

route = router.resolveRoute(named: "post_create")!
print(route.name) // "post_create"

// Generating a URL
var url = router.generateURLForRoute(named: "post_list", query: ["filter": "latest"])!
print(url.absoluteString) // "/posts?filter=latest"

url = router.generateURLForRoute(named: "post_get", parameters: ["id": "1"], query: ["shows_tags": "true"])!
print(url.absoluteString) // "/posts/1?shows_tags=true"

url = router.generateURLForRoute(named: "post_delete", parameters: ["id": "1"])!
print(url.absoluteString) // "/posts/1"
```
