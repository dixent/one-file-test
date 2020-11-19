In AMP, there are two way to achieve serving optimized images:

- Developers using image formats that are not widely supported, such as WebP, can configure their server to process browser `Accept` headers and respond with image bytes and the appropriate [`Content-Type` header](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/client-hints/). This avoids the browser from downloading image types it does not support. Read more about [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation).[sourcecode:html]
Accept: image/webp,image/apng,image/*,*/*;q=0.8
[/sourcecode]
- Provide nested image fallbacks, such as the example below.
