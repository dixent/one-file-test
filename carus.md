---
$title: Creating the cover page
$order: 4
description: "To create a page, add the <amp-story-page> element as a child of amp-story. Assign a unique id to the page. For our first page, which is the cover page, let's assign a unique id of cover: ..."
author: bpaduch
---
<table class="noborder">
<tr>
    <td colspan="2"><h5 id="fill">Template: Fill</h5></td>
</tr>
<tr>
    <td width="65%">The <strong>fill</strong> template fills the screen with the first child element in the layer. Any other children in this layer aren't shown.

    The fill template works well for backgrounds, including images and videos.
   <code class="nopad"><pre>&lt;amp-story-grid-layer template="fill">
  &lt;amp-img src="dog.png"
      width="720" height="1280"
      layout="responsive">
  &lt;/amp-img>
&lt;/amp-story-grid-layer></pre></code>
    </td>
    <td>
    {{ image('/static/img/docs/tutorials/amp_story/layer-fill.png', 216, 341) }}
    </td>
</tr>
</table>
