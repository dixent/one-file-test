---
layout: post
title: Control focus with tabindex
authors:
- robdodson
date: 2018-11-18
description: Native HTML elements such as <button> or <input> have keyboard accessibility built-in for free. If you''re building custom interactive components, use tabindex to ensure that they''re keyboard accessible.
---

Нативные элементы HTML, такие как `<button>` или `<input>` имеют встроенную доступность клавиатуры бесплатно. Если вы создаете *пользовательские* интерактивные компоненты, используйте атрибут `tabindex` чтобы обеспечить их доступность с клавиатуры.

{% Aside %} Whenever possible, use a native HTML element rather than building your own custom version. `<button>`, for example, is very easy to style and already has full keyboard support. This will save you from needing to manage `tabindex` or to add semantics with ARIA. {% endAside %}

## Проверьте, доступны ли элементы управления с клавиатуры

Такой инструмент, как Lighthouse, отлично подходит для обнаружения определенных проблем с доступностью, но некоторые вещи могут быть проверены только человеком.

Попробуйте нажать клавишу `TAB` для навигации по вашему сайту. Вы можете получить доступ ко всем интерактивным элементам управления на странице? Если нет, вам может понадобиться использовать [`tabindex`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex) для улучшения фокусировки этих элементов управления.

{% Aside 'warning' %} If you don't see a focus indicator at all, it may be hidden by your CSS. Check for any styles that mention `:focus { outline: none; }`. You can learn how to fix this in our guide on [styling focus](/style-focus). {% endAside %}

## Вставить элемент в порядок вкладок

Вставьте элемент в `tabindex="0"` порядок табуляции, используя `tabindex="0"` . Например:

```html
<div tabindex="0">Focus me with the TAB key</div>
```

Чтобы сфокусировать элемент, нажмите клавишу `TAB` или вызовите метод `focus()` элемента.

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/tabindex-zero?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-zero on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

## Удалить элемент из табуляции

Удалите элемент, используя `tabindex="-1"` . Например:

```html
<button tabindex="-1">Can't reach me with the TAB key!</button>
```

Это удаляет элемент из естественного порядка табуляции, но элемент все еще можно сфокусировать, вызвав его метод `focus()` .

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/tabindex-negative-one?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

## Избегайте `tabindex > 0`

Любой `tabindex` больше 0 `tabindex` элемент в начало естественного порядка табуляции. Если имеется несколько элементов с `tabindex` больше 0, порядок табуляции начинается с наименьшего значения, большего нуля, и повышается.

Использование `tabindex` больше 0 считается **анти-модель** , потому что читатели экрана навигации страницу в DOM порядке, а не вкладка заказа. Если вам нужно, чтобы элемент появлялся раньше в порядке табуляции, он должен быть перемещен на более раннее место в DOM.

Lighthouse упрощает идентификацию элементов с помощью `tabindex` > 0. Запустите аудит специальных возможностей (Lighthouse> Options> Accessibility) и найдите результаты аудита «Нет элемента, значение [tabindex] которого больше 0».

## Создавайте доступные компоненты с помощью " `tabindex` "

Если вы создаете сложный компонент, вам может потребоваться добавить дополнительную поддержку клавиатуры вне фокуса. Рассмотрим родной элемент `select` . Он фокусируемый, и вы можете использовать клавиши со стрелками для предоставления дополнительных функций (выбираемые параметры).

Для реализации аналогичной функциональности в ваших собственных компонентах используйте технику, известную как « `tabindex` ». Перемещение tabindex работает, устанавливая `tabindex` в -1 для всех детей, кроме активного в данный момент. Затем компонент использует прослушиватель событий клавиатуры, чтобы определить, какая клавиша нажата пользователем.

Когда это происходит, компонент устанавливает для `tabindex` ранее сфокусированного дочернего `tabindex` значение -1, для `tabindex` для сфокусированного дочернего `tabindex` значение 0 и вызывает для него метод `focus()` .

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

---

макет: заголовок сообщения: Управление фокусом с авторами tabindex:

- Дата Робдодсона: 2018-11-18 описание: | Нативные элементы HTML, такие как <button>или <input> иметь встроенную доступность клавиатуры бесплатно. Если вы создаете пользовательские интерактивные компоненты, используйте tabindex, чтобы обеспечить их доступность с клавиатуры.</button>

---

Нативные элементы HTML, такие как `<button>` или `<input>` имеют встроенную доступность клавиатуры бесплатно. Если вы создаете *пользовательские* интерактивные компоненты, используйте атрибут `tabindex` чтобы обеспечить их доступность с клавиатуры.

{% Aside %} Whenever possible, use a native HTML element rather than building your own custom version. `<button>`, for example, is very easy to style and already has full keyboard support. This will save you from needing to manage `tabindex` or to add semantics with ARIA. {% endAside %}

## Проверьте, доступны ли элементы управления с клавиатуры

Такой инструмент, как Lighthouse, отлично подходит для обнаружения определенных проблем с доступностью, но некоторые вещи могут быть проверены только человеком.

Попробуйте нажать клавишу `TAB` для навигации по вашему сайту. Вы можете получить доступ ко всем интерактивным элементам управления на странице? Если нет, вам может понадобиться использовать [`tabindex`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex) для улучшения фокусировки этих элементов управления.

{% Aside 'warning' %} If you don't see a focus indicator at all, it may be hidden by your CSS. Check for any styles that mention `:focus { outline: none; }`. You can learn how to fix this in our guide on [styling focus](/style-focus). {% endAside %}

## Вставить элемент в порядок вкладок

Вставьте элемент в `tabindex="0"` порядок табуляции, используя `tabindex="0"` . Например:

```html
<div tabindex="0">Focus me with the TAB key</div>
```

Чтобы сфокусировать элемент, нажмите клавишу `TAB` или вызовите метод `focus()` элемента.

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/tabindex-zero?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-zero on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

## Удалить элемент из табуляции

Удалите элемент, используя `tabindex="-1"` . Например:

```html
<button tabindex="-1">Can't reach me with the TAB key!</button>
```

Это удаляет элемент из естественного порядка табуляции, но элемент все еще можно сфокусировать, вызвав его метод `focus()` .

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/tabindex-negative-one?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

## Избегайте `tabindex > 0`

Любой `tabindex` больше 0 `tabindex` элемент в начало естественного порядка табуляции. Если имеется несколько элементов с `tabindex` больше 0, порядок табуляции начинается с наименьшего значения, большего нуля, и повышается.

Использование `tabindex` больше 0 считается **анти-модель** , потому что читатели экрана навигации страницу в DOM порядке, а не вкладка заказа. Если вам нужно, чтобы элемент появлялся раньше в порядке табуляции, он должен быть перемещен на более раннее место в DOM.

Lighthouse упрощает идентификацию элементов с помощью `tabindex` > 0. Запустите аудит специальных возможностей (Lighthouse> Options> Accessibility) и найдите результаты аудита «Нет элемента, значение [tabindex] которого больше 0».

## Создавайте доступные компоненты с помощью " `tabindex` "

Если вы создаете сложный компонент, вам может потребоваться добавить дополнительную поддержку клавиатуры вне фокуса. Рассмотрим родной элемент `select` . Он фокусируемый, и вы можете использовать клавиши со стрелками для предоставления дополнительных функций (выбираемые параметры).

Для реализации аналогичной функциональности в ваших собственных компонентах используйте технику, известную как « `tabindex` ». Перемещение tabindex работает, устанавливая `tabindex` в -1 для всех детей, кроме активного в данный момент. Затем компонент использует прослушиватель событий клавиатуры, чтобы определить, какая клавиша нажата пользователем.

Когда это происходит, компонент устанавливает для `tabindex` ранее сфокусированного дочернего `tabindex` значение -1, для `tabindex` для сфокусированного дочернего `tabindex` значение 0 и вызывает для него метод `focus()` .

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

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/roving-tabindex?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

{% Aside %} Curious what those `role=""` attributes are for? They let you change the semantics of an element so it will be announced properly by a screen reader. You can learn more about them in our guide on [screen reader basics](/semantics-and-screen-readers). {% endAside %}

## Рецепты доступа с клавиатуры

Если вы не уверены, какой уровень поддержки клавиатуры может понадобиться вашим пользовательским компонентам, вы можете обратиться к [ARIA Authoring Practices 1.1](https://www.w3.org/TR/wai-aria-practices-1.1/) . В этом удобном руководстве перечислены распространенные шаблоны пользовательского интерфейса и указаны ключи, которые должны поддерживать ваши компоненты.

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">   <iframe src="https://glitch.com/embed/#!/embed/roving-tabindex?path=index.html&previewSize=100&attributionHidden=true" alt="tabindex-negative-one on Glitch" style="height: 100%; width: 100%; border: 0;">   </iframe> </div>

{% Aside %} Curious what those `role=""` attributes are for? They let you change the semantics of an element so it will be announced properly by a screen reader. You can learn more about them in our guide on [screen reader basics](/semantics-and-screen-readers). {% endAside %}

## Рецепты доступа с клавиатуры

Если вы не уверены, какой уровень поддержки клавиатуры может понадобиться вашим пользовательским компонентам, вы можете обратиться к [ARIA Authoring Practices 1.1](https://www.w3.org/TR/wai-aria-practices-1.1/) . В этом удобном руководстве перечислены распространенные шаблоны пользовательского интерфейса и указаны ключи, которые должны поддерживать ваши компоненты.
