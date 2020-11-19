In HTML, you can serve different image formats by using the `picture` tag.  In AMP, although the `picture` tag isn't supported, you can serve different images by using the `fallback`  attribute.

[tip type="read-on"]
**READ ON –** To learn more about fallbacks, see the [Placeholders & Fallbacks](placeholders.md) guide.
[/tip]

In AMP, there are two way to achieve serving optimized images:

- Developers using image formats that are not widely supported, such as WebP, can configure their server to process browser `Accept` headers and respond with image bytes and the appropriate [`Content-Type` header](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/client-hints/). This avoids the browser from downloading image types it does not support. Read more about [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation).[sourcecode:html]
Accept: image/webp,image/apng,image/*,*/*;q=0.8
[/sourcecode]
- Provide nested image fallbacks, such as the example below.

##### Example: Serve different image formats

In the following example, if the browser supports WebP, serve mountains.webp, otherwise serve mountains.jpg.

[example preview="top-frame" playground="true"]
```html
<amp-img alt="Mountains"
  width="550"
  height="368"
  layout="responsive"
  src="{{server_for_email}}/static/inline-examples/images/mountains.webp">
  <amp-img alt="Mountains"
    fallback
    width="550"
    height="368"
    layout="responsive"
    src="{{server_for_email}}/static/inline-examples/images/mountains.jpg"></amp-img>
</amp-img>
```
[/example]
