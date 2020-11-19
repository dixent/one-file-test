### AMP Stories

Use `preview="top-frame"` together with `orientation="portrait"` for previewing AMP Stories.

<div class="ap-m-code-snippet"><pre>
  &#91;example preview="top-frame"
         orientation="portrait"
         playground="true"]
    ```html
    &lt;head&gt;
      &lt;script async custom-element=&quot;amp-story&quot;
          src=&quot;https://cdn.ampproject.org/v0/amp-story-1.0.js&quot;&gt;&lt;/script&gt;
      &lt;style amp-custom&gt;
        body {
          font-family: 'Roboto', sans-serif;
        }
        amp-story-page {
          background: white;
        }
      &lt;/style&gt;
    &lt;/head&gt;
    &lt;body&gt;
      &lt;amp-story standalone&gt;
        &lt;amp-story-page id=&quot;cover&quot;&gt;
          &lt;amp-story-grid-layer template=&quot;vertical&quot;&gt;
            &lt;h1&gt;Hello World&lt;/h1&gt;
            &lt;p&gt;This is the cover page of this story.&lt;/p&gt;
          &lt;/amp-story-grid-layer&gt;
        &lt;/amp-story-page&gt;
        &lt;amp-story-page id=&quot;page-1&quot;&gt;
          &lt;amp-story-grid-layer template=&quot;vertical&quot;&gt;
            &lt;h1&gt;First Page&lt;/h1&gt;
            &lt;p&gt;This is the first page of this story.&lt;/p&gt;
          &lt;/amp-story-grid-layer&gt;
        &lt;/amp-story-page&gt;
      &lt;/amp-story&gt;
    &lt;/body&gt;
    ```
  [/example]</pre>
</div>

### Inline Sample

Here is simple inline sample embed. You can define CSS via inline styles:

<div class="ap-m-code-snippet"><pre>
  &#91;example preview="inline" playground="true"]
    ```html
    &lt;div style=&quot;background: red; width: 200px; height: 200px;&quot;&gt;Hello World&lt;/div&gt;
    ```
  &#91;/example]
  [/example]</pre>
</div>

<div class="ap-m-code-snippet">
  <pre>
  &lsqb;example preview="default: none|inline|top-frame"
          playground="default: true|false"
          imports="&lt;custom-element-1&gt;,&lt;custom-element-2&gt;,..."
          template="&lt;custom-template&gt;"]
  ```html
    // code sample
  ```
  &lsqb;/example]
  </pre>
</div>

This is what it looks like:

[example preview="inline" playground="true"]
```html
<div style="background: red; width: 200px; height: 200px;">Hello World</div>
```
[/example]
