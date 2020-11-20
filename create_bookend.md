<table>
<thead>
<tr>
  <th width="20%">Type</th>
  <th>Description</th>
</tr>

</thead>
<tbody>
  <tr>
    <td>heading</td>
    <td>Allows you to specify a heading to group articles.
  <pre class="nopreline">
  {
    "type": "heading",
    "text": "More to read"
  },
  </pre>
    <br>
    <figure class="alignment-wrapper half">
      <amp-img src="/static/img/docs/tutorials/amp_story/bookend_heading.png" width="720" height="140" layout="responsive" alt="bookend heading"></amp-img>
    </figure>
    </td>
  </tr>
  <tr>
    <td>small</td>
    <td>Allows you to link to related articles with the option to include an associated small image.
  <pre class="nopreline">
  {
    "type": "small",
    "title": "Learn about cats",
    "url": "https://wikipedia.org/wiki/Cat",
    "image": "assets/bookend_cats.jpg"
  },
  </pre>
    <br>
    <figure class="alignment-wrapper half">
      <amp-img src="/static/img/docs/tutorials/amp_story/bookend_small.png" width="720" height="267" layout="responsive" alt="bookend small article"></amp-img>
    </figure>
  </td>
  </tr>
  <tr>
    <td>landscape</td>
    <td>Allows you to link to articles or other content, like videos. The image associated with this type is larger and in landscape format.
  <pre class="nopreline">
  {
    "type": "landscape",
    "title": "Learn about border collies",
    "url": "https://wikipedia.org/wiki/Border_Collie",
    "image": "assets/bookend_dogs.jpg",
    "category": "Dogs"
  },
  </pre>
    <br>
    <figure class="alignment-wrapper half">
      <amp-img src="/static/img/docs/tutorials/amp_story/bookend_landscape.png" width="720" height="647" layout="responsive" alt="bookend landscape article"></amp-img>
    </figure>
    </td>
  </tr>
  <tr>
    <td>portrait</td>
    <td>Allows you to link to stories or other content.  The image associated with this type is larger and in portrait format.
  <pre class="nopreline">
  {
    "type": "portrait",
    "title": "Learn about macaws",
    "url": "https://wikipedia.org/wiki/Macaw",
    "image": "assets/bookend_birds.jpg",
    "category": "birds"
  },
  </pre>
    <br>
    <figure class="alignment-wrapper half">
      <amp-img src="/static/img/docs/tutorials/amp_story/bookend_portrait.png" width="720" height="1018" layout="responsive" alt="bookend portrait article"></amp-img>
    </figure>
    </td>
  </tr>
  <tr>
    <td>cta-link</td>
    <td>Allows you to specify calls to action links that are displayed as buttons (e.g., read more, Subscribe).
  <pre class="nopreline">
  {
    "type": "cta-link",
    "links": [
      {
        "text": "Learn more",
        "url": "https://amp.dev/about/stories.html"
      }
    ]
  }
  </pre>
    <br>
    <figure class="alignment-wrapper half">
      <amp-img src="/static/img/docs/tutorials/amp_story/bookend_cta.png" width="720" height="137" layout="responsive" alt="bookend cta"></amp-img>
    </figure>
    </td>
  </tr>
</tbody>
</table>
