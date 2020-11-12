---
"$title": Спецификация AMP для рекламы
order: '3'
formats:
- Объявления
teaser:
  text: |2-

    _Если вы хотите предложить изменения в стандарте, прокомментируйте
    [Намерение
toc: правда
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

*Если вы хотите предложить изменения в стандарте, прокомментируйте [намерение внедрить](https://github.com/ampproject/amphtml/issues/4264)* .

Объявления AMPHTML - это механизм для показа быстрых и эффективных объявлений на страницах AMP. Чтобы рекламные документы AMPHTML («креативы AMP») могли быстро и плавно отображаться в браузере и не ухудшать работу пользователей, креативы AMP должны подчиняться набору правил проверки. Подобно[правилам формата AMP](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml) , объявления AMPHTML имеют доступ к ограниченному набору разрешенных тегов, возможностей и расширений.

## Правила формата объявлений AMPHTML<a name="amphtml-ad-format-rules"></a>

Если ниже не указано иное, креатив должен подчиняться всем правилам [формата AMP](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml.html) , приведенным здесь в качестве ссылки. Например, AMPHTML объявление [Boilerplate](#boilerplate) отклоняется от стандартного шаблонного AMP.

Кроме того, креативы должны подчиняться следующим правилам:

<table>
<thead><tr>
  <th>Правило</th>
  <th>Обоснование</th>
</tr></thead>
<tbody>
<tr>
<td>Необходимо использовать <code><html ⚡4ads></code> или <code><html amp4ads></code> качестве включающих тегов.</td>
<td>Позволяет валидаторам идентифицировать креативный документ как общий документ AMP или как ограниченный рекламный документ AMPHTML и отправлять его соответствующим образом.</td>
</tr>
<tr>
<td>Необходимо включить <code><script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script></code> в качестве сценария выполнения вместо <code>https://cdn.ampproject.org/v0.js</code> .</td>
<td>Позволяет настраивать поведение во время выполнения для объявлений AMPHTML, показываемых в окнах iframe из разных источников.</td>
</tr>
<tr>
<td>Не должен включать <code><link rel="canonical"></code> .</td>
<td>У рекламных объявлений нет "канонической версии, отличной от AMP", и они не будут индексироваться в поисковой системе независимо, поэтому ссылки на себя будут бесполезны.</td>
</tr>
<tr>
<td>Can include optional meta tags in HTML head as identifiers, in the format of <code><meta name="amp4ads-id" content="vendor=${vendor},type=${type},id=${id}"></code>. Those meta tags must be placed before the <code>amp4ads-v0.js</code> script. The value of <code>vendor</code> and <code>id</code> are strings containing only [0-9a-zA-Z_-]. The value of <code>type</code> is either <code>creative-id</code> or <code>impression-id</code>.</td>
<td>Those custom identifiers can be used to identify the impression or the creative. They can be helpful for reporting and debugging.<br><br><p>Example:</p>
<pre>
<meta name="amp4ads-id"
  content="vendor=adsense,type=creative-id,id=1283474">
<meta name="amp4ads-id"
  content="vendor=adsense,type=impression-id,id=xIsjdf921S"></pre>
</td>
</tr>
<tr>
<td>
<code><amp-analytics></code> отслеживание видимости может быть нацелено только на селектор полной рекламы с помощью <code>"visibilitySpec": { "selector": "amp-ad" }</code> как определено в <a href="https://github.com/ampproject/amphtml/issues/4018">выпуске № 4018</a> и <a href="https://github.com/ampproject/amphtml/pull/4368">PR № 4368</a> . В частности, он не может настраивать таргетинг на какие-либо селекторы для элементов в рекламном объявлении.</td>
<td>In some cases, AMPHTML ads may choose to render an ad creative in an iframe.In those cases, host page analytics can only target the entire iframe anyway, and won’t have access to any finer-grained selectors.<br><br> <p>Example:</p> <pre>
<amp-analytics id="nestedAnalytics">
  <script type="application/json">
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
  </script>
</amp-analytics>
</pre> <p>This configuration sends a request to the <code>https://example.com/nestedAmpAnalytics</code> URL when 50% of the enclosing ad has been continuously visible on the screen for 1 second.</p> </td>
</tr>
</tbody>
</table>

### Boilerplate<a name="boilerplate"></a>

Для объявлений AMPHTML требуется другая и значительно более простая строка стандартного стиля, чем в [обычных документах AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) :

[sourcecode:html]
<style amp4ads-boilerplate>
  body {
    visibility: hidden;
  }
</style>
[/sourcecode]

*Обоснование:* стиль `amp-boilerplate` скрывает содержимое тела до тех пор, пока среда выполнения AMP не будет готова, и может показать его. Если Javascript отключен или среда выполнения AMP не загружается, шаблон по умолчанию гарантирует, что содержимое в конечном итоге будет отображаться независимо. Однако в объявлениях AMPHTML, если Javascript полностью отключен, объявления AMPHTML не будут показываться, и никакая реклама не будет отображаться, поэтому в разделе `<noscript>` нет необходимости. В отсутствие среды выполнения AMP большая часть оборудования, на котором основана реклама AMPHTML (например, аналитика для отслеживания видимости или `amp-img` для отображения контента), будет недоступна, поэтому лучше не отображать рекламу, чем показывать неисправную.

Наконец, в рекламном шаблоне AMPHTML используется `amp-a4a-boilerplate` а не `amp-boilerplate` поэтому валидаторы могут легко идентифицировать его и создавать более точные сообщения об ошибках, чтобы помочь разработчикам.

Обратите внимание, что к стандартному тексту применяются те же правила, что и к [общему шаблону AMP](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) .

### CSS<a name="css"></a>

<table>
<thead><tr>
  <th>Правило</th>
  <th>Обоснование</th>
</tr></thead>
<tbody>
  <tr>
    <td>
<code>position:fixed</code> и <code>position:sticky</code> запрещены в творческом CSS.</td>
    <td>
<code>position:fixed</code> выходы из теневой модели DOM, от которой зависят объявления AMPHTML. Кроме того, объявления в AMP уже не могут использовать фиксированную позицию.</td>
  </tr>
  <tr>
    <td>
<code>touch-action</code> запрещено.</td>
    <td>Объявление, которое может управлять <code>touch-action</code> может мешать пользователю прокручивать основной документ.</td>
  </tr>
  <tr>
    <td>Креативный CSS ограничен 20 000 байтами.</td>
    <td>Большие блоки CSS раздувают креатив, увеличивают задержку в сети и снижают производительность страницы.</td>
  </tr>
  <tr>
    <td>На переход и анимацию распространяются дополнительные ограничения.</td>
    <td>AMP должен иметь возможность управлять всей анимацией, относящейся к объявлению, чтобы останавливать их, когда реклама не отображается на экране или системные ресурсы очень низки.</td>
  </tr>
  <tr>
    <td>В целях проверки префиксы, зависящие от поставщика, считаются псевдонимами для одного и того же символа без префикса. Это означает, что если символ <code>foo</code> запрещен правилами проверки CSS, то символ <code>-vendor-foo</code> также будет запрещен.</td>
    <td>Некоторые свойства с префиксом поставщика предоставляют функциональные возможности, эквивалентные свойствам, которые в иных случаях запрещены или ограничены этими правилами.<br><br><p> Пример: <code>-webkit-transition</code> и <code>-moz-transition</code> считаются псевдонимами <code>transition</code> . Они будут разрешены только в контекстах, где разрешен простой <code>transition</code> (см. Раздел « <a href="#selectors">Селекторы</a> » ниже).</p>
</td>
  </tr>
</tbody>
</table>

#### CSS-анимации и переходы<a name="css-animations-and-transitions"></a>

##### Селекторы<a name="selectors"></a>

Свойства `transition` и `animation` разрешены только для селекторов, которые:

- Содержат только свойства `transition` , `animation` , `transform` , `visibility` или `opacity` .

    *Обоснование:* это позволяет среде выполнения AMP удалить этот класс из контекста, чтобы деактивировать анимацию, когда это необходимо для производительности страницы.

**Хороший**

[sourcecode:css]
.box {
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

**Плохой**

Свойство запрещено в классе CSS.

[sourcecode:css]
.box {
  color: red; // non-animation property not allowed in animation selector
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

##### Переходные и анимируемые свойства<a name="transitionable-and-animatable-properties"></a>

Единственные свойства, которые могут быть перенесены, - это непрозрачность и преобразование. ( [Обоснование](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) )

**Хороший**

[sourcecode:css]
transition: transform 2s;
[/sourcecode]

**Плохой**

[sourcecode:css]
transition: background-color 2s;
[/sourcecode]

**Хороший**

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

**Плохой**

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

### Разрешенные расширения и встроенные функции AMP<a name="allowed-amp-extensions-and-builtins"></a>

В рекламном объявлении AMPHTML *разрешены* следующие модули расширения AMP и встроенные теги AMP. Расширения или встроенные теги, не указанные явно, запрещены.

- [усилитель-аккордеон](https://amp.dev/documentation/components/amp-accordion)
- [amp-ad-exit](https://amp.dev/documentation/components/amp-ad-exit)
- [amp-analytics](https://amp.dev/documentation/components/amp-analytics)
- [amp-anim](https://amp.dev/documentation/components/amp-anim)
- [amp-animation](https://amp.dev/documentation/components/amp-animation)
- [amp-audio](https://amp.dev/documentation/components/amp-audio)
- [amp-bind](https://amp.dev/documentation/components/amp-bind)
- [amp-carousel](https://amp.dev/documentation/components/amp-carousel)
- [amp-fit-text](https://amp.dev/documentation/components/amp-fit-text)
- [amp-font](https://amp.dev/documentation/components/amp-font)
- [amp-form](https://amp.dev/documentation/components/amp-form)
- [amp-img](https://amp.dev/documentation/components/amp-img)
- [amp-layout](https://amp.dev/documentation/components/amp-layout)
- [amp-lightbox](https://amp.dev/documentation/components/amp-lightbox)
- amp-mraid, на экспериментальной основе. Если вы планируете использовать это, пожалуйста, откройте вопрос на [wg-monetization](https://github.com/ampproject/wg-monetization/issues/new) .
- [усы](https://amp.dev/documentation/components/amp-mustache)
- [amp-пиксель](https://amp.dev/documentation/components/amp-pixel)
- [amp-позиция-наблюдатель](https://amp.dev/documentation/components/amp-position-observer)
- [amp-селектор](https://amp.dev/documentation/components/amp-selector)
- [amp-social-share](https://amp.dev/documentation/components/amp-social-share)
- [amp-video](https://amp.dev/documentation/components/amp-video)

Большинство упущений связано с производительностью или упрощением анализа объявлений AMPHTML.

*Пример:* `<amp-ad>` исключен из этого списка. Это явно запрещено, потому что включение `<amp-ad>` внутри `<amp-ad>` может потенциально привести к неограниченным водопадам загрузки рекламы, что не соответствует целям эффективности объявлений AMPHTML.

*Пример:* `<amp-iframe>` исключен из этого списка. Это запрещено, потому что реклама может использовать его для выполнения произвольного Javascript и загрузки произвольного контента. Объявления, желающие использовать такие возможности, должны возвращать `false` из своей записи [a4aRegistry](https://github.com/ampproject/amphtml/blob/master/ads/_a4a-config.js#L40) и использовать существующий механизм отображения рекламы «3p iframe».

*Пример:* `<amp-facebook>` , `<amp-instagram>` , `<amp-twitter>` и `<amp-youtube>` опущены по той же причине, что и `<amp-iframe>` : все они создают фреймы и потенциально могут потреблять неограниченные ресурсы. в них.

*Пример:* `<amp-ad-network-*-impl>` исключены из этого списка. Тег `<amp-ad>` обрабатывает делегирование этим тегам реализации; креативы не должны пытаться включать их напрямую.

*Пример:* `<amp-lightbox>` еще не добавлен, потому что даже некоторые рекламные файлы AMPHTML могут отображаться в iframe, и в настоящее время нет механизма для расширения объявления за пределы iframe. Поддержка этого может быть добавлена в будущем, если будет продемонстрировано желание.

### HTML-теги<a name="html-tags"></a>

Следующие теги *разрешены* в объявлениях AMPHTML. Не разрешенные явно теги запрещены. Этот список является подмножеством общего [разрешенного списка добавления тегов AMP](https://github.com/ampproject/amphtml/blob/master/extensions/amp-a4a/../../spec/amp-tag-addendum.md) . Как и этот список, он упорядочен в соответствии со спецификацией HTML5 в разделе 4 [«Элементы HTML»](http://www.w3.org/TR/html5/single-page.html#html-elements) .

Большинство упущений связано с производительностью или потому, что теги не соответствуют стандарту HTML5. Например, `<noscript>` опущен, потому что объявления AMPHTML зависят от включенного JavaScript, поэтому блок `<noscript>` никогда не будет выполняться и, следовательно, только приведет к раздуванию креатива и затрат на пропускную способность и задержку. Аналогично, `<acronym>` , `<big>` , et al. запрещены, потому что они несовместимы с HTML5.

#### 4.1 Корневой элемент<a name="41-the-root-element"></a>

4.1.1 `<html>`

- Необходимо использовать типы `<html ⚡4ads>` или `<html amp4ads>`

#### 4.2 Метаданные документа<a name="42-document-metadata"></a>

4.2.1 `<head>`

4.2.2 `<title>`

4.2.4 `<link>`

- Теги `<link rel=...>` запрещены, за исключением `<link rel=stylesheet>` .

- **Примечание.** В отличие от обычных AMP, теги `<link rel="canonical">` запрещены.

    4.2.5 `<style>` 4.2.6 `<meta>`

#### 4.3 Разделы<a name="43-sections"></a>

4.3.1 `<body>` 4.3.2 `<article>` 4.3.3 `<section>` 4.3.4 `<nav>` 4.3.5 `<aside>` 4.3.6 `<h1>` , `<h2>` , `<h3>` , `<h4>` , `<h5>` и `<h6>` 4.3.7 `<header>` 4.3.8 `<footer>` 4.3.9 `<address>`

#### 4.4 Группировка контента<a name="44-grouping-content"></a>

4.4.1 `<p>` 4.4.2 `<hr>` 4.4.3 `<pre>` 4.4.4 `<blockquote>` 4.4.5 `<ol>` 4.4.6 `<ul>` 4.4.7 `<li>` 4.4.8 `<dl>` 4.4. 9 `<dt>` 4.4.10 `<dd>` 4.4.11 `<figure>` 4.4.12 `<figcaption>` `<div>` 4.4.14 `<main>`

#### 4.5 Семантика на уровне текста<a name="45-text-level-semantics"></a>

4.5.1 `<a>` 4.5.2 `<em>` 4.5.3 `<strong>` 4.5.4 `<small>` 4.5.5 `<s>` 4.5.6 `<cite>` 4.5.7 `<q>` 4.5.8 `<dfn>` 4.5. 9 `<abbr>` 4.5.10 `<data>` 4.5.11 `<time>` 4.5.12 `<code>` 4.5.13 `<var>` 4.5.14 `<samp>` 4.5.15 `<kbd >` 4.5.16 `<sub>` и `<sup>` 4.5.17 `<i>` 4.5.18 `<b>` 4.5.19 `<u>` 4.5.20 `<mark>` 4.5.21 `<ruby>` 4.5.22 `<rb>` 4.5.23 `<rt>` 4.5.24 `<rtc>` 4.5. 25 `<rp>` 4.5.26 `<bdi>` 4.5.27 `<bdo>` 4.5.28 `<span>` 4.5.29 `<br>` 4.5.30 `<wbr>`

#### 4.6 Правки<a name="46-edits"></a>

4.6.1 `<ins>` 4.6.2 `<del>`

#### 4.7 Встроенный контент<a name="47-embedded-content"></a>

- Встроенный контент поддерживается только с помощью тегов AMP, таких как `<amp-img>` или `<amp-video>` .

#### 4.7.4 `<source>`<a name="474-source"></a>

#### 4.7.18 SVG<a name="4718-svg"></a>

Теги SVG отсутствуют в пространстве имен HTML5. Они перечислены ниже без идентификаторов разделов.

`<svg>``<g>``<путь>``<глиф>``<glyphref>``<маркер>``<просмотр>``<круг>``<line>``<полигон>``<полилиния>``<правильно>``<текст>``<textpath>``<tref>``<tspan>``<clippath>``<фильтр>``<линейный градиент>``<радиальный градиент>``<маска>``<шаблон>``<vkern>``<hkern>``<defs>``<использовать>``<символ>``<desc>``<название>`

#### 4.9 Табличные данные<a name="49-tabular-data"></a>

4.9.1 `<table>` 4.9.2 `<caption>` 4.9.3 `<colgroup>` 4.9.4 `<col>` 4.9.5 `<tbody>` 4.9.6 `<thead>` 4.9.7 `<tfoot>` 4.9.8 `<tr>` 4.9.9 `<td>` 4.9.10 `<th>`

#### 4.10 Формы<a name="410-forms"></a>

4.10.8 `<button>`

#### 4.11 Создание сценариев<a name="411-scripting"></a>

- Как и в обычном документе AMP, тег `<head>` должен содержать тег `<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>` .
- В отличие от обычных AMP, `<noscript>` запрещен.
    - *Обоснование:* поскольку для работы AMPHTML-объявлений требуется, чтобы Javascript был включен, блоки `<noscript>` бесполезны в объявлениях AMPHTML и только расходуют пропускную способность сети.
- В отличие от обычного AMP, `<script type="application/ld+json">` запрещен.
    - *Обоснование:* JSON LD используется для разметки структурированных данных на страницах хоста, но рекламные объявления не являются отдельными документами и не содержат структурированных данных. Блоки JSON LD в них будут просто стоить пропускной способности сети.
- Все остальные правила сценариев и исключения перенесены из общего AMP.
