# Cache-Control

The **Cache-Control** general-header field is used to specify directives for caching mechanisms in both requests and responses. Caching directives are unidirectional, meaning that a given directive in a request is not implying that the same directive is to be given in the response.

### Cache request directives

1. Cache-Control: max-age=<seconds>
2. Cache-Control: max-stale[=<seconds>]
3. Cache-Control: min-fresh=<seconds>
4. Cache-Control: no-cache 
5. Cache-Control: no-store
6. Cache-Control: no-transform
7. Cache-Control: only-if-cached

### Cache response directives

1. Cache-Control: must-revalidate
2. Cache-Control: no-cache
3. Cache-Control: no-store
4. Cache-Control: no-transform
5. Cache-Control: public
6. Cache-Control: private
7. Cache-Control: proxy-revalidate
8. Cache-Control: max-age=<seconds>
9. Cache-Control: s-maxage=<seconds>

### Extension `Cache-Control` directives

Extension `Cache-Control` directives are not part of the core HTTP caching standards document. Be sure to check the [compatibility table](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control#Browser_compatibility) for their support.

## Directives

### Cacheability

- `public`

  Indicates that the response may be cached by any cache.

- `private`

  Indicates that the response is intended for a single user and must not be stored by a shared cache. A private cache may store the response.

- `no-cache`

  Forces caches to submit the request to the origin server for validation before releasing a cached copy.

- `only-if-cached`

  Indicates to not retrieve new data. This being the case, the server wishes the client to obtain a response only once and then cache. From this moment the client should keep releasing a cached copy and avoid contacting the origin-server to see if a newer copy exists.

### Expiration

- `max-age=<seconds>`

  Specifies the maximum amount of time a resource will be considered fresh. Contrary to `Expires`, this directive is relative to the time of the request.

- `s-maxage=<seconds>`

  Overrides `max-age` or the `Expires` header, but it only applies to shared caches (e.g., proxies) and is ignored by a private cache.

- `max-stale[=<seconds>]`

  Indicates that the client is willing to accept a response that has exceeded its expiration time. Optionally, you can assign a value in seconds, indicating the time the response must not be expired by.

- `min-fresh=<seconds>`

  Indicates that the client wants a response that will still be fresh for at least the specified number of seconds.

- `stale-while-revalidate=<seconds>` **

  Indicates that the client is willing to accept a stale response while asynchronously checking in the background for a fresh one. The seconds value indicates for how long the client is willing to accept a stale response.

- `stale-if-error=<seconds>` **

  Indicates that the client is willing to accept a stale response if the check for a fresh one fails. The seconds value indicates for how long the client is willing to accept the stale response after the initial expiration.

### Revalidation and reloading

- `must-revalidate`

  The cache must verify the status of the stale resources before using it and expired ones should not be used.

- `proxy-revalidate`

  Same as `must-revalidate`, but it only applies to shared caches (e.g., proxies) and is ignored by a private cache.

- `immutable`

  Indicates that the response body will not change over time. The resource, if unexpired, is unchanged on the server and therefore the client should not send a conditional revalidation for it (e.g. `If-None-Match` or `If-Modified-Since`) to check for updates, even when the user explicitly refreshes the page. Clients that aren't aware of this extension must ignore them as per the HTTP specification. In Firefox, `immutable`is only honored on `https://` transactions. For more information, see also this [blog post](http://bitsup.blogspot.de/2016/05/cache-control-immutable.html).

### Other

- `no-store`

  The cache should not store anything about the client request or server response.

- `no-transform`

  No transformations or conversions should be made to the resource. The Content-Encoding, Content-Range, Content-Type headers must not be modified by a proxy. A non- transparent proxy might, for example, convert between image formats in order to save cache space or to reduce the amount of traffic on a slow link. The `no-transform`directive disallows this.