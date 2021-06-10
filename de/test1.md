---
"$title": AMP for Ads-Spezifikation
order: '3'
formats:
  - Anzeigen
teaser:
  text: |2-

    _Wenn Sie Änderungen an der Norm vorschlagen möchten, kommentieren Sie bitte die
    [Absicht
toc: wahr
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/extensions/amp-a4a/amp-a4a-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
      http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

*Wenn Sie Änderungen am Standard vorschlagen möchten, kommentieren Sie bitte die [Absicht zur Implementierung](https://github.com/ampproject/amphtml/issues/4264)* .

AMPHTML-Anzeigen sind ein Mechanismus zum Rendern schneller, leistungsstarker Anzeigen auf AMP-Seiten. Damit AMPHTML-Anzeigendokumente ("AMP-Creatives") schnell und reibungslos im Browser gerendert werden können und die Nutzererfahrung nicht beeinträchtigen, müssen AMP-Creatives eine Reihe von Validierungsregeln einhalten. Ähnlich wie bei den[AMP-Formatregeln](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml) haben AMPHTML-Anzeigen Zugriff auf eine begrenzte Anzahl zulässiger Tags, Funktionen und Erweiterungen.

## Regeln für AMPHTML-Anzeigenformate<a name="amphtml-ad-format-rules"></a>

Sofern unten nicht anders angegeben, muss das Creative allen Regeln der [AMP-Formatregeln entsprechen](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml.html) , die hier durch Verweis aufgenommen werden. Beispielsweise weicht die AMPHTML-Anzeigen- [Boilerplate](#boilerplate) von der AMP-Standard-Boilerplate ab.

Darüber hinaus müssen Creatives die folgenden Regeln einhalten:

<table>
<thead><tr>
  <th>Regel</th>
  <th>Begründung</th>
</tr></thead>
<tbody>
<tr>
<td>Muss <code>&lt;html ⚡4ads&gt;</code> oder <code>&lt;html amp4ads&gt;</code> als einschließende Tags verwenden.</td>
<td>Ermöglicht Validatoren, ein Creative-Dokument entweder als allgemeines AMP-Dokument oder als eingeschränktes AMPHTML-Anzeigendokument zu identifizieren und entsprechend zu versenden.</td>
</tr>
<tr>
<td>Muss <code>&lt;script async src="https://cdn.ampproject.org/amp4ads-v0.js"&gt;&lt;/script&gt;</code> als Laufzeitskript anstelle von <code>https://cdn.ampproject.org/v0.js</code> .</td>
<td>Ermöglicht angepasstes Laufzeitverhalten für AMPHTML-Anzeigen, die in ursprungsübergreifenden Iframes geschaltet werden.</td>
</tr>
<tr>
<td>Darf kein <code>&lt;link rel="canonical"&gt;</code> Tag enthalten.</td>
<td>Anzeigen-Creatives haben keine "nicht AMP-kanonische Version" und werden nicht unabhängig suchindexiert, sodass eine Selbstreferenzierung nutzlos wäre.</td>
</tr>
<tr>
<td>Kann optionale Meta-Tags im HTML- <code>&lt;meta name="amp4ads-id" content="vendor=${vendor},type=${type},id=${id}"&gt;</code> . Diese Meta-Tags müssen vor dem Skript <code>amp4ads-v0.js</code> Der Wert von <code>vendor</code> und <code>id</code> sind Zeichenfolgen, die nur [0-9a-zA-Z_-] enthalten. Der Wert von <code>type</code> ist entweder <code>creative-id</code> oder <code>impression-id</code> .</td>
<td>Those custom identifiers can be used to identify the impression or the creative. They can be helpful for reporting and debugging.<br><br><p>Example:</p>
<pre>
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=creative-id,id=1283474"&gt;
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=impression-id,id=xIsjdf921S"&gt;</pre>
</td>
</tr>
<tr>
<td>
<code>&lt;amp-analytics&gt;</code> -Sichtbarkeits-Tracking kann nur auf die vollständige Anzeigenauswahl ausgerichtet werden, über <code>"visibilitySpec": { "selector": "amp-ad" }</code> wie in <a href="https://github.com/ampproject/amphtml/issues/4018">Issue #4018</a> und <a href="https://github.com/ampproject/amphtml/pull/4368">PR #4368 definiert</a> . Insbesondere darf es keine Selektoren für Elemente innerhalb des Anzeigenmotivs gezielt beliefern.</td>
<td>In some cases, AMPHTML ads may choose to render an ad creative in an iframe.In those cases, host page analytics can only target the entire iframe anyway, and won’t have access to any finer-grained selectors.<br><br> <p>Example:</p> <pre>
&lt;amp-analytics id="nestedAnalytics"&gt;
  &lt;script type="application/json"&gt;
  {
    "requests": {
      "visibility": "https://example.com/nestedAmpAnalytics"
    },
    "triggers": {
      "visibilitySpec": {
      "selector": "amp-ad",
      "visiblePercentageMin": 50,
      "continuousTimeMin": 1000
      }
    }
  }
  &lt;/script&gt;
&lt;/amp-analytics&gt;
</pre> <p>This configuration sends a request to the <code>https://example.com/nestedAmpAnalytics</code> URL when 50% of the enclosing ad has been continuously visible on the screen for 1 second.</p>
</td>
</tr>
</tbody>
</table>

### Kesselplatte<a name="boilerplate"></a>

AMPHTML-Anzeigen-Creatives erfordern eine andere und wesentlich einfachere Boilerplate-Stilzeile als [allgemeine AMP-Dokumente](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) :

[sourcecode:html]
<style amp4ads-boilerplate>
  body {
    visibility: hidden;
  }
</style>
[/sourcecode]

*Begründung:* Der `amp-boilerplate` Stil blendet Körperinhalte aus, bis die AMP-Laufzeit bereit ist und sie einblenden kann. Wenn Javascript deaktiviert ist oder die AMP-Laufzeit nicht geladen wird, stellt der Standard-Boilerplate sicher, dass der Inhalt trotzdem angezeigt wird. In AMPHTML-Anzeigen werden jedoch, wenn Javascript vollständig deaktiviert ist, AMPHTML-Anzeigen nicht geschaltet und es wird keine Anzeige geschaltet, sodass der Abschnitt `<noscript>` Ohne die AMP-Laufzeit sind die meisten Maschinen, auf denen AMPHTML-Anzeigen basieren (z. B. Analysen für die Sichtbarkeitsverfolgung oder `amp-img` für die Inhaltsanzeige), nicht verfügbar. Daher ist es besser, keine Anzeige zu schalten als eine fehlerhafte.

Schließlich verwendet die AMPHTML-Anzeigen-Boilerplate `amp-a4a-boilerplate` anstelle von `amp-boilerplate` damit Validatoren sie leicht identifizieren und genauere Fehlermeldungen erzeugen können, um Entwicklern zu helfen.

Beachten Sie, dass die gleichen Regeln für Mutationen im Boilerplate-Text gelten wie im [allgemeinen AMP-Boilerplate](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) .

### CSS<a name="css"></a>

<table>
<thead><tr>
  <th>Regel</th>
  <th>Begründung</th>
</tr></thead>
<tbody>
  <tr>
    <td>
<code>position:fixed</code> und <code>position:sticky</code> sind im Creative-CSS verboten.</td>
    <td>
<code>position:fixed</code> bricht aus dem Schatten-DOM aus, von dem AMPHTML-Anzeigen abhängig sind. Außerdem dürfen Anzeigen in AMP bereits keine feste Position verwenden.</td>
  </tr>
  <tr>
    <td>
<code>touch-action</code> sind verboten.</td>
    <td>Eine Anzeige, die <code>touch-action</code> manipulieren kann, kann die Fähigkeit des Benutzers beeinträchtigen, das Hostdokument zu scrollen.</td>
  </tr>
  <tr>
    <td>Creative-CSS ist auf 20.000 Byte beschränkt.</td>
    <td>Große CSS-Blöcke blähen das Creative auf, erhöhen die Netzwerklatenz und verschlechtern die Seitenleistung.</td>
  </tr>
  <tr>
    <td>Übergang und Animation unterliegen zusätzlichen Einschränkungen.</td>
    <td>AMP muss in der Lage sein, alle zu einer Anzeige gehörenden Animationen zu steuern, damit es diese stoppen kann, wenn die Anzeige nicht auf dem Bildschirm ist oder die Systemressourcen sehr gering sind.</td>
  </tr>
  <tr>
    <td>Herstellerspezifische Präfixe werden zu Validierungszwecken als Aliase für dasselbe Symbol ohne das Präfix betrachtet. Dies bedeutet, dass wenn ein Symbol <code>foo</code> durch CSS-Validierungsregeln verboten ist, auch das Symbol <code>-vendor-foo</code> verboten ist.</td>
    <td>Einige Eigenschaften mit Anbieterpräfix bieten eine gleichwertige Funktionalität wie Eigenschaften, die ansonsten gemäß diesen Regeln verboten oder eingeschränkt sind.<br><br><p> Beispiel: <code>-webkit-transition</code> und <code>-moz-transition</code> gelten beide als Aliase für <code>transition</code> . Sie sind nur in Kontexten zulässig, in denen ein bloßer <code>transition</code> zulässig wäre (siehe <a href="#selectors">Abschnitt Selektoren</a> unten).</p>
</td>
  </tr>
</tbody>
</table>

#### CSS-Animationen und Übergänge<a name="css-animations-and-transitions"></a>

##### Selektoren<a name="selectors"></a>

Die `transition` und `animation` sind nur für Selektoren zulässig, die:

- Enthalten nur die Eigenschaften `transition` , `animation` , `transform` , `visibility` oder `opacity`

    *Begründung:* Dadurch kann die AMP-Laufzeit diese Klasse aus dem Kontext entfernen, um Animationen zu deaktivieren, wenn dies für die Seitenleistung erforderlich ist.

**Gut**

[sourcecode:css]
.box {
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

**Schlecht**

Eigenschaft in CSS-Klasse nicht zulässig.

[sourcecode:css]
.box {
  color: red; // non-animation property not allowed in animation selector
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

##### Übergangs- und animierbare Eigenschaften<a name="transitionable-and-animatable-properties"></a>

Die einzigen Eigenschaften, die geändert werden können, sind Deckkraft und Transformation. ( [Begründung](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) )

**Gut**

[sourcecode:css]
transition: transform 2s;
[/sourcecode]

**Schlecht**

[sourcecode:css]
transition: background-color 2s;
[/sourcecode]

**Gut**

[sourcecode:css]
@keyframes turn {
  from {
    transform: rotate(180deg);
  }

  to {
    transform: rotate(90deg);
  }
}
[/sourcecode]

**Schlecht**

[sourcecode:css]
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
[/sourcecode]

### Zulässige AMP-Erweiterungen und -Integrationen<a name="allowed-amp-extensions-and-builtins"></a>

Folgendes sind *zulässige* AMP-Erweiterungsmodule und integrierte AMP-Tags in einem AMPHTML-Anzeigen-Creative. Erweiterungen oder eingebaute Tags, die nicht explizit aufgeführt sind, sind verboten.

- [Amp-Akkordeon](https://amp.dev/documentation/components/amp-accordion)
- [amp-ad-exit](https://amp.dev/documentation/components/amp-ad-exit)
- [amp-analytik](https://amp.dev/documentation/components/amp-analytics)
- [Amp-Anime](https://amp.dev/documentation/components/amp-anim)
- [Amp-Animation](https://amp.dev/documentation/components/amp-animation)
- [Verstärker-Audio](https://amp.dev/documentation/components/amp-audio)
- [amp-bind](https://amp.dev/documentation/components/amp-bind)
- [Amp-Karussell](https://amp.dev/documentation/components/amp-carousel)
- [amp-fit-text](https://amp.dev/documentation/components/amp-fit-text)
- [amp-font](https://amp.dev/documentation/components/amp-font)
- [Verstärkerform](https://amp.dev/documentation/components/amp-form)
- [amp-img](https://amp.dev/documentation/components/amp-img)
- [Verstärker-Layout](https://amp.dev/documentation/components/amp-layout)
- [Amp-Lightbox](https://amp.dev/documentation/components/amp-lightbox)
- amp-mraid, auf experimenteller Basis. Wenn Sie dies in Erwägung ziehen, öffnen Sie bitte ein Problem bei [wg-ads](https://github.com/ampproject/wg-ads/issues/new) .
- [Amp-Schnurrbart](https://amp.dev/documentation/components/amp-mustache)
- [Ampere-Pixel](https://amp.dev/documentation/components/amp-pixel)
- [Amp-Position-Beobachter](https://amp.dev/documentation/components/amp-position-observer)
- [Amp-Selektor](https://amp.dev/documentation/components/amp-selector)
- [amp-social-share](https://amp.dev/documentation/components/amp-social-share)
- [Verstärker-Video](https://amp.dev/documentation/components/amp-video)

Die meisten Auslassungen dienen entweder der Leistung oder sollen die Analyse von AMPHTML-Anzeigen vereinfachen.

*Beispiel:* `<amp-ad>` wird in dieser Liste weggelassen. Dies ist ausdrücklich nicht zulässig, da das Zulassen einer `<amp-ad>` innerhalb einer `<amp-ad>` potenziell zu unbegrenzten Wasserfällen beim Laden von Anzeigen führen könnte, was die Leistungsziele von AMPHTML-Anzeigen nicht erfüllt.

*Beispiel:* `<amp-iframe>` wird in dieser Liste weggelassen. Es ist nicht zulässig, da Anzeigen es verwenden könnten, um beliebiges Javascript auszuführen und beliebigen Inhalt zu laden. Anzeigen, die solche Funktionen verwenden möchten, sollten aus ihrem [a4aRegistry-](https://github.com/ampproject/amphtml/blob/master/ads/_a4a-config.js#L40) `false` zurückgeben und den vorhandenen '3p-iframe'-Anzeigen-Rendering-Mechanismus verwenden.

*Beispiel:* `<amp-facebook>` , `<amp-instagram>` , `<amp-twitter>` und `<amp-youtube>` werden aus dem gleichen Grund wie `<amp-iframe>` weggelassen: Sie alle erstellen Iframes und können potenziell unbegrenzte Ressourcen verbrauchen in ihnen.

*Beispiel:* `<amp-ad-network-*-impl>` wird in dieser Liste weggelassen. Das `<amp-ad>` -Tag verarbeitet die Delegierung an diese Implementierungs-Tags; Creatives sollten nicht versuchen, sie direkt einzubinden.

*Beispiel:* `<amp-lightbox>` ist noch nicht enthalten, da sogar einige AMPHTML-Anzeigen-Creatives in einem Iframe gerendert werden können und es derzeit keinen Mechanismus gibt, mit dem eine Anzeige über einen Iframe hinaus erweitert werden kann. Dies kann in Zukunft unterstützt werden, wenn der Wunsch dazu besteht.

### HTML-Tags<a name="html-tags"></a>

Im Folgenden ist *erlaubt* Tags in einem AMPHTML Anzeigen kreativ. Nicht ausdrücklich erlaubte Tags sind verboten. Diese Liste ist eine Teilmenge der allgemeinen [Zulassungsliste für AMP-Tag-Anhänge](https://github.com/ampproject/amphtml/blob/master/extensions/amp-a4a/../../spec/amp-tag-addendum.md) . Wie diese Liste ist sie in Übereinstimmung mit den HTML5-Spezifikationen in Abschnitt 4 [Die Elemente von HTML](http://www.w3.org/TR/html5/single-page.html#html-elements) geordnet.

Die meisten Auslassungen dienen entweder der Leistung oder weil die Tags nicht dem HTML5-Standard entsprechen. `<noscript>` wird beispielsweise weggelassen, weil AMPHTML-Anzeigen davon abhängig sind, dass JavaScript aktiviert ist, sodass ein `<noscript>` -Block niemals ausgeführt wird und daher nur das Creative aufbläht und Bandbreite und Latenz kostet. In ähnlicher Weise haben `<acronym>` , `<big>` et al. sind verboten, da sie nicht HTML5-kompatibel sind.

#### 4.1 Das Wurzelelement<a name="41-the-root-element"></a>

4.1.1 `<html>`

- Es müssen die Typen `<html ⚡4ads>` oder `<html amp4ads>`

#### 4.2 Dokument-Metadaten<a name="42-document-metadata"></a>

4.2.1 `<head>`

4.2.2 `<title>`

4.2.4 `<link>`

- `<link rel=...>` Tags sind nicht zulässig, mit Ausnahme von `<link rel=stylesheet>` .

- **Hinweis:** Im Gegensatz zu allgemeinem AMP sind `<link rel="canonical">` Tags verboten.

    4.2.5 `<style>` 4.2.6 `<meta>`

#### 4.3 Abschnitte<a name="43-sections"></a>

4.3.1 `<body>` 4.3.2 `<article>` 4.3.3 `<section>` 4.3.4 `<nav>` 4.3.5 `<aside>` 4.3.6 `<h1>` , `<h2>` , `<h3>` , `<h4>` , `<h5>` und `<h6>` 4.3.7 `<header>` 4.3.8 `<footer>` 4.3.9 `<address>`

#### 4.4 Inhalte gruppieren<a name="44-grouping-content"></a>

4.4.1 `<p>` 4.4.2 `<hr>` 4.4.3 `<pre>` 4.4.4 `<blockquote>` 4.4.5 `<ol>` 4.4.6 `<ul>` 4.4.7 `<li>` 4.4.8 `<dl>` 4.4. 9 `<dt>` 4.4.10 `<dd>` 4.4.11 `<figure>` 4.4.12 `<figcaption>` 4.4.13 `<div>` 4.4.14 `<main>`

#### 4.5 Semantik auf Textebene<a name="45-text-level-semantics"></a>

4.5.1 `<a>` 4.5.2 `<em>` 4.5.3 `<strong>` 4.5.4 `<small>` 4.5.5 `<s>` 4.5.6 `<cite>` cite `<q>` 4.5.8 `<dfn>` 4.5. 9 `<abbr>` 4.5.10 `<data>` 4.5.11 `<time>` 4.5.12 `<code>` 4.5.13 `<var>` 4.5.14 `<samp>` 4.5.15 `<kbd >` 4.5.16 `<sub>` und `<sup>` 4.5.17 `<i>` 4.5.18 `<b>` 4.5.19 `<u>` 4.5.20 `<mark>` 4.5.21 `<ruby>` rubin `<rb>` 4.5.23 `<rt>` 4.5.24 `<rtc>` 4.5. 25 `<rp>` 4.5.26 `<bdi>` 4.5.27 `<bdo>` 4.5.28 `<span>` 4.5.29 `<br>` 4.5.30 `<wbr>`

#### 4.6 Bearbeitungen<a name="46-edits"></a>

4.6.1 `<ins>` 4.6.2 `<del>`

#### 4.7 Eingebettete Inhalte<a name="47-embedded-content"></a>

- Eingebettete Inhalte werden nur über AMP-Tags wie `<amp-img>` oder `<amp-video>` .

#### 4.7.4 `<source>`<a name="474-source"></a>

#### 4.7.18 SVG<a name="4718-svg"></a>

SVG-Tags befinden sich nicht im HTML5-Namespace. Sie sind unten ohne Abschnitts-IDs aufgeführt.

`&lt;svg&gt;``&lt;g&gt;``&lt;Pfad&gt;``&lt;Glyphe&gt;``&lt;glyphref&gt;``&lt;Markierung&gt;``&lt;Ansicht&gt;``&lt;Kreis&gt;``&lt;Linie&gt;``&lt;polygon&gt;``&lt;Polylinie&gt;``&lt;recht&gt;``&lt;text&gt;``&lt;Textpfad&gt;``&lt;tref&gt;``&lt;tspan&gt;``&lt;clippfad&gt;``&lt;filter&gt;``&lt;lineargradient&gt;``&lt;radialgradient&gt;``&lt;Maske&gt;``&lt;Muster&gt;``&lt;vkern&gt;``&lt;hkern&gt;``&lt;defs&gt;``&lt;benutzen&gt;``&lt;Symbol&gt;``&lt;desc&gt;``&lt;Titel&gt;`

#### 4.9 Tabellendaten<a name="49-tabular-data"></a>

4.9.1 `<table>` 4.9.2 `<caption>` 4.9.3 `<colgroup>` 4.9.4 `<col>` 4.9.5 `<tbody>` 4.9.6 `<thead>` 4.9.7 `<tfoot>` 4.9.8 `<tr>` 4.9. 9 `<td>` 4.9.10 `<th>`

#### 4.10 Formulare<a name="410-forms"></a>

4.10.8 `<button>`

#### 4.11 Skripting<a name="411-scripting"></a>

- Wie ein allgemeines AMP-Dokument muss das `<head>` -Tag `<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>` -Tag enthalten.
- Im Gegensatz zu allgemeinem AMP ist `<noscript>` verboten.
    - *Begründung:* Da für AMPHTML-Anzeigen Javascript aktiviert sein muss, um überhaupt zu funktionieren, `<noscript>` -Blöcke in AMPHTML-Anzeigen keinen Zweck und kosten nur Netzwerkbandbreite.
- Im Gegensatz zu allgemeinem AMP ist `<script type="application/ld+json">` verboten.
    - *Begründung:* JSON LD wird für strukturiertes Daten-Markup auf Hostseiten verwendet, aber Anzeigen-Creatives sind keine eigenständigen Dokumente und enthalten keine strukturierten Daten. JSON LD-Blöcke in ihnen würden nur Netzwerkbandbreite kosten.
- Alle anderen Skriptregeln und Ausschlüsse werden aus dem allgemeinen AMP übernommen.
