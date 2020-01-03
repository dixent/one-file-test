---
layout: Почта
title: Управление фокусом с помощью tabindex
authors:
- robdodson
date: '2018-11-18'
description: 'Собственные элементы HTML, такие как & lt; button & gt; или & lt; input
  & gt; иметь доступ к клавиатуре '
---

Собственные элементы HTML, такие как ` & lt; button & gt;  code> или ` & lt; input & gt;  code>, имеют доступ с клавиатуры < span translate = "no">
 span> встроенный бесплатно. Если вы создаете * custom  em> интерактивные компоненты, используйте <span translate="no">
Атрибут  span> <code data-md-type="codespan"> tabindex  code>, обеспечивающий их доступность с клавиатуры.</code></span>*``

{% Aside%} <span translate="no">
 span> Когда это возможно, используйте собственный HTML-элемент, а не создавайте <span translate="no">
 span> собственная пользовательская версия. Например, <code data-md-type="codespan"> & lt; button & gt;  code> очень просто стилизовать и <span translate="no">
 span> уже имеет полную поддержку клавиатуры. Это избавит вас от необходимости управлять <span translate="no">
 span> <code data-md-type="codespan"> tabindex  code> или для добавления семантики с помощью ARIA. <span translate="no">
 span> {% endAside%}</span></code></span></span></code></span></span>

## Проверьте, доступны ли элементы управления с клавиатуры

Такой инструмент, как Lighthouse, отлично подходит для обнаружения определенных проблем с доступностью, но <span translate="no">
 span> некоторые вещи могут быть проверены только человеком.</span>

Попробуйте нажать клавишу ` TAB  code> для навигации по сайту. Можете ли вы связаться с <span translate="no">
 span> все интерактивные элементы управления на странице? Если нет, вам может понадобиться использовать <span translate="no">
 span> <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex" data-md-type="link">  tabindex  code> </a> <span translate="no">
 span> для улучшения фокусировки этих элементов управления.</span></span></span>`

{% Aside 'warning'%} <span translate="no">
 span> Если вы вообще не видите индикатор фокуса, он может быть скрыт вашим <span translate="no">
 SPAN> CSS. Проверьте наличие стилей, в которых упоминается <code data-md-type="codespan">: focus {outline: none; }  code>. <span translate="no">
 span> Вы можете узнать, как это исправить, в нашем руководстве по <span translate="no">
 span> <a href="/style-focus" data-md-type="link"> фокусировка стиля </a>. <span translate="no">
 span> {% endAside%}</span></span></span></code></span></span>

## Вставить элемент в порядок вкладок

Вставьте элемент в обычный порядок табуляции, используя ` tabindex = "0"  code>. Например:`

```html
<div tabindex="0">Focus me with the TAB key</div>
```

Чтобы сфокусировать элемент, нажмите клавишу ` TAB  code> или вызовите элемент ` focus ()  code>.``

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">
  <iframe src="https://glitch.com/embed/#!/embed/tabindex-zero?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-zero on Glitch" style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

## Удалить элемент из табуляции

Удалите элемент, используя ` tabindex = "- 1"  code>. Например:`

```html
<button tabindex="-1">Can't reach me with the TAB key!</button>
```

Это удаляет элемент из естественного порядка табуляции, но элемент все еще может быть <span translate="no">
 span> фокусируется, вызывая его метод <code data-md-type="codespan"> focus ()  code>.</code></span>

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">
  <iframe src="https://glitch.com/embed/#!/embed/tabindex-negative-one?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

## Избегайте ` tabindex & gt; 0  код>`

Любое значение ` tabindex  code> больше 0 переводит элемент в начало естественной вкладки <span translate="no">
 SPAN> порядок. Если имеется несколько элементов с <code data-md-type="codespan"> tabindex  code> больше 0, вкладка <span translate="no">
 span> ордер начинается с самого низкого значения больше нуля и идет вверх.</span></code></span>`

Использование ` tabindex  code> больше 0 считается ** анти-паттерном  strong>, потому что <span translate=" нет ">
 span> программы чтения с экрана перемещаются по странице в DOM-порядке, а не в виде табуляции. Если вам нужен <span translate="no">
Элемент  span> должен появляться раньше в порядке табуляции, его следует переместить в более раннее место <span translate="no">
 span> в DOM.</span></span></span>**`

Lighthouse позволяет легко идентифицировать элементы с помощью ` tabindex  code> & gt; 0. Запустите <span translate="no">
 span> Аудит доступности (Lighthouse & gt; Опции & gt; Доступность) и найдите <span translate="no">
Результаты  span> аудита «Нет элемента имеет значение [tabindex] больше 0».</span></span>`

## Создавайте доступные компоненты с помощью "roving <code data-md-type=" codespan "> tabindex  code>"</code>

Если вы создаете сложный компонент, вам может потребоваться добавить дополнительную клавиатуру <span translate="no">
Поддержка  span> вне фокуса. Рассмотрим собственный элемент <code data-md-type="codespan"> select  code>. Это фокусируемое и <span translate="no">
 span> вы можете использовать клавиши со стрелками для предоставления дополнительных функций (выбираемый <span translate="no">
 SPAN> Параметры).</span></span></code></span>

Чтобы реализовать аналогичные функции в ваших собственных компонентах, используйте метод, известный как <span translate="no">
 span> как "roving <code data-md-type=" codespan "> tabindex  code>". Подвижный tabindex работает, устанавливая <code data-md-type="codespan"> tabindex  code> в -1 для <span translate="no">
 span> все дочерние элементы, кроме активного в данный момент. Затем компонент использует клавиатуру <span translate="no">
Слушатель событий  span>, чтобы определить, какая клавиша нажата пользователем.</span></span></code></code></span>

Когда это происходит, компонент устанавливает ранее сфокусированный дочерний элемент ` tabindex  code> <span translate="no">
 span>, равный -1, устанавливает значение <code data-md-type="codespan"> tabindex  code> для целевого дочернего элемента равным 0 и вызывает <code data-md-type="codespan"> focus ()  code> <span translate="no">
 span> метод на это.</span></code></code></span>`

**Перед**

```html/2-3
<div role="toolbar">
  <button tabindex="-1">Undo</div>
  <button tabindex="0">Redo</div>
  <button tabindex="-1">Cut</div>
</div>
```

**После**

```html/2-3
<div role="toolbar">
  <button tabindex="-1">Undo</div>
  <button tabindex="-1">Redo</div>
  <button tabindex="0">Cut</div>
</div>
```

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">
  <iframe src="https://glitch.com/embed/#!/embed/roving-tabindex?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

{% Aside%} <span translate="no">
 span> Любопытно, для чего эти атрибуты <code data-md-type="codespan"> role = ""  code>? Они позволяют вам изменить <span translate="no">
 span> семантика элемента, поэтому он будет правильно объявлен программой чтения с экрана. <span translate="no">
 span> Подробнее о них вы можете узнать в нашем руководстве на <span translate="no">
 span> <a href="/semantics-and-screen-readers" data-md-type="link"> основы чтения с экрана </a>. <span translate="no">
 span> {% endAside%}</span></span></span></span></code></span>

## Рецепты доступа с клавиатуры

Если вы не уверены, на каком уровне поддержки клавиатуры ваши пользовательские компоненты могут <span translate="no">
 span> нужно, вы можете обратиться к <span translate="no">
 span> <a href="https://www.w3.org/TR/wai-aria-practices-1.1/" data-md-type="link"> ARIA Authoring Practices 1.1 </a>. . < span translate = "no">
 span> В этом удобном руководстве перечислены распространенные шаблоны пользовательского интерфейса и указаны ключи, которые ваш <span translate="no">
Компоненты  span> должны поддерживать.</span></span></span>
