# 🦂 Caching

### (HTTP) cache

Implementation that holds requests and responses for reusing in subsequent requests. It can be either a shared cache or a private cache

#### links

- rfc - [https://httpwg.org/specs/rfc9111.html](https://httpwg.org/specs/rfc9111.html)
- youtube - [https://www.youtube.com/watch?v=HiBDZgTNpXY](https://www.youtube.com/watch?v=HiBDZgTNpXY)
- mdn - [https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

### Terms

#### Shared cache

Cache that exists between the origin server and clients (e.g. Proxy, CDN). It stores a single response and reuses it with multiple users — so developers should avoid storing personalized contents to be cached in the shared cache.

#### Private/local/browser cache

Cache that exists in the client. It is also called _local cache_ or _browser cache_. It can store and reuse personalized content for a single user.

#### Store response

Store a response in caches when the response is cacheable. However, the cached response is not always reused as-is. (Usually, "cache" means storing a response.)

#### Reuse response

Reuse cached responses for subsequent requests.

#### Revalidate response

Ask the origin server whether or not the stored response is still [fresh](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#fresh\_and\_stale\_based\_on\_age). Usually, the revalidation is done through a conditional request.

#### Age

The time since a response was generated. It is a criterion for whether a response is [fresh or stale](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#fresh\_and\_stale\_based\_on\_age).

### Response types

#### Fresh response

Indicates that the response is [fresh](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#fresh\_and\_stale\_based\_on\_age). This usually means the response can be reused for subsequent requests, depending on request directives.

#### Stale response

Indicates that the response is a [stale response](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#fresh\_and\_stale\_based\_on\_age). This usually means the response can't be reused as-is. Cache storage isn't required to remove stale responses immediately because revalidation could change the response from being stale to being [fresh](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#fresh\_and\_stale\_based\_on\_age) again.

### Processes

* Validation
* Invalidation

### Headers and directives

Key points about the `Cache-Control` header and its directives:

* The `max-age` directive specifies the maximum amount of time a response can be considered fresh.
* The `must-revalidate` directive requires the response to be revalidated with the origin server before reuse.
* The `public` directive allows the response to be stored in a shared cache.
* The `no-store` directive prevents the response from being stored in any cache.
* The `no-cache` directive allows the response to be stored in a cache, but requires revalidation before use.
* The `max-age=0` directive is often used as a workaround for no-cache, but it can cause stale responses to be reused when caches are disconnected from the origin server [\[1\]](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) [\[2\]](https://www.fastly.com/blog/cache-control-wild) [\[3\]](https://stackoverflow.com/questions/1046966/whats-the-difference-between-cache-control-max-age-0-and-no-cache).
* The `Cache-Control` header is used both in the response and request headers. When sent by the origin server, it controls how caches and user agents should handle the response. When sent by the user agent, it controls how caches and the origin server should handle subsequent requests
* `304 Not Modified` Header used when resource wasn't modified

Standard `Cache-Control` directives:

| Request          | Response                 |
| ---------------- | ------------------------ |
| `max-age`        | `max-age`                |
| `max-stale`      | -                        |
| `min-fresh`      | -                        |
| -                | `s-maxage`               |
| `no-cache`       | `no-cache`               |
| `no-store`       | `no-store`               |
| `no-transform`   | `no-transform`           |
| `only-if-cached` | -                        |
| -                | `must-revalidate`        |
| -                | `proxy-revalidate`       |
| -                | `must-understand`        |
| -                | `private`                |
| -                | `public`                 |
| -                | `immutable`              |
| -                | `stale-while-revalidate` |
| `stale-if-error` | `stale-if-error`         |

### Examples

```http
cache-control: max-age=0, private, must-revalidate
```
