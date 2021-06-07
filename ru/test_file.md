# Файлы разметки

![1](https://prikolnye-kartinki.ru/img/picture/Jan/31/cd47b809864a288682ad43078a9642ab/1.jpg)

Вы можете записывать контент в обычные файлы Markdown (например, файлы с `.md` ). Jupyter Book поддерживает любой синтаксис Markdown, поддерживаемый записными книжками Jupyter. Jupyter Notebook Markdown - это расширение [разновидности Markdown](https://commonmark.org/) под названием CommonMark Markdown. В нем есть много элементов для стандартной обработки текста, но не хватает многих функций, используемых для публикации и документации.

```{note}
If you'd like a more in-depth overview and guide to CommonMark Markdown, see
[the CommonMark Markdown tutorial](https://commonmark.org/help/tutorial/).
```

На этой странице описаны некоторые основные функции Jupyter Notebook Markdown и способы их включения в вашу книгу.

```{margin}
Jupyter Book also supports a fancier version of Markdown called **MyST Markdown**. This
is a slightly extended flavour of Jupyter Notebook Markdown. It
allows you to include citations and cross-references, and control more complex
functionality like adding content to the margin. For more
information, check out {doc}`../content/myst`.
```

## Встраивание медиа

### Добавление изображений

Вы можете ссылаться на внешние носители, например изображения из вашего файла Markdown. Если вы используете относительные пути, они будут продолжать работать при копировании файлов Markdown, пока они указывают на файл внутри репозитория.

Вот изображение относительно корня содержания книги

![C-3PO_droid](https://jupyterbook.org/_images/C-3PO_droid.png)

Он был создан с помощью этого кода:

```md
![C-3PO_droid](../images/C-3PO_droid.png)
```

:::{смотрите также}[](../content/figures.md) для дополнительной информации. :::

### Добавление фильмов

Вы даже можете вставлять ссылки на фильмы в Интернете! Например, вот вам маленький GIF!

![giphy](https://media.giphy.com/media/yoJC2A59OCZHs1LXvW/giphy.gif)

Это будет включено в вашу книгу, когда она будет построена.

## Математика

Для вывода HTML Jupyter Book использует отличную [библиотеку MathJax](http://docs.mathjax.org/en/latest/) вместе с конфигурацией Jupyter Notebook по умолчанию для рендеринга математики из синтаксиса в стиле LaTeX.

Например, вот математическое выражение, отображаемое с помощью MathJax:

$$ P (A_1 \ чашка A_2 \ чашка A_3) &amp; = P (B \ чашка A_3) \ &amp; = P (B) + P (A_3) - P (BA_3) \ &amp; = P (A_1) + P (A_2) - P (A_1A_2) + P (A_3) - P (A_1A_3 \ cup A_2A_3) \ &amp; = \ sum_ {i = 1} ^ 3 P (A_i) - \ mathop {\ sum \ sum} _ {1 \ le i &lt; j \ le 3} P (A_iA_j) + P (A_1A_2A_3) $$

:::{смотрите также}[](../content/math.md) для дополнительной информации. :::

### Математика на блочном уровне

Вы можете включить математику на уровне блоков, заключив формулы в символы `$$` Например, следующий блок:

```md
$$
wow = its^{math}
$$
```

Результаты в этом выводе:

$$ wow = это ^ {math} $$

Вы также можете включать математические блоки, используя синтаксис в стиле LaTeX, используя `\begin{align*}` . Например, следующий блок:

```latex
\begin{align*}
yep = its_{more}^{math}
\end{align*}
```

Результаты в:

\ begin {align *} yep = its_ {more} ^ {math} \ end {align *}

::: {important} Для этого необходимо [включить расширение `amsmath`](math:latex) MyST. :::

## Расширенная уценка с MyST Markdown

Помимо CommonMark Markdown, Jupyter Book также поддерживает более полнофункциональную версию Markdown под названием **MyST Markdown** . Это надмножество CommonMark, которое включает синтаксические части, которые полезны для публикации вычислительных описаний. Для получения дополнительной информации о MyST Markdown см.[](../content/myst.md) .
