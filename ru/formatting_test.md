### AMP-истории

Используйте `preview="top-frame"` вместе с `orientation="portrait"` для предварительного просмотра историй AMP.

[example preview="top-frame"
         orientation="portrait"
         playground="true"]
    ```html
    <head>
      <script async custom-element="amp-story"
          src="https://cdn.ampproject.org/v0/amp-story-1.0.js"></script>
      <style amp-custom>
        body {
          font-family: 'Roboto', sans-serif;
        }
        amp-story-page {
          background: white;
        }
      </style>
    </head>
    <body>
      <amp-story standalone>
        <amp-story-page id="cover">
          <amp-story-grid-layer template="vertical">
            <h1>Hello World</h1>
            <p>This is the cover page of this story.</p>
          </amp-story-grid-layer>
        </amp-story-page>
        <amp-story-page id="page-1">
          <amp-story-grid-layer template="vertical">
            <h1>First Page</h1>
            <p>This is the first page of this story.</p>
          </amp-story-grid-layer>
        </amp-story-page>
      </amp-story>
    </body>
    ```
  [/example]### Встроенный образец

Вот простой встроенный образец. Вы можете определить CSS с помощью встроенных стилей:

[example preview="inline" playground="true"]
    ```html
    <div style="background: red; width: 200px; height: 200px;">Hello World</div>
    ```
  [/example]Вот как это выглядит:

[example preview="inline" playground="true"]
```html
<div style="background: red; width: 200px; height: 200px;">Hello World</div>
```
[/example]
