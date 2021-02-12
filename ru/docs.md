# Внесите вклад в документацию TensorFlow

TensorFlow приветствует вклад в документацию - если вы улучшите документацию, вы улучшите саму библиотеку TensorFlow. Документация на tenorflow.org подразделяется на следующие категории:

- *Справочник по* [API. Справочные документы по API](https://www.tensorflow.org/api_docs/) создаются из строк документации в [исходном коде TensorFlow](https://github.com/tensorflow/tensorflow) .
- *Описательная документация* - это [учебные пособия](https://www.tensorflow.org/tutorials) , [руководства](https://www.tensorflow.org/guide) и другие материалы, не являющиеся частью кода TensorFlow. Эта документация находится в [репозитории tenorflow / docs на](https://github.com/tensorflow/docs) GitHub.
- *Community translations* —These are guides and tutorials translated by the community. All community translations live in the [tensorflow/docs](https://github.com/tensorflow/docs/tree/master/site) repo.

Некоторые [проекты TensorFlow](https://github.com/tensorflow) хранят исходные файлы документации рядом с кодом в отдельном репозитории, обычно в каталоге `docs/` . См. Файл `CONTRIBUTING.md` проекта или свяжитесь с сопровождающим, чтобы внести свой вклад.

Чтобы принять участие в сообществе документации TensorFlow:

- Смотрите [репозиторий tensorflow / docs на](https://github.com/tensorflow/docs) GitHub.
- Подпишитесь на [docs@tensorflow.org](https://groups.google.com/a/tensorflow.org/forum/#!forum/docs) .

## Справочник по API

Для обновления справочной документации, найти [исходный файл](https://www.tensorflow.org/code/tensorflow/python/) и изменение символа в <a href="https://www.python.org/dev/peps/pep-0257/" class="external">строке документации</a> . Многие справочные страницы API на tenorflow.org включают ссылку на исходный файл, в котором определен символ. Строки документации поддерживают <a href="https://help.github.com/en/articles/about-writing-and-formatting-on-github" class="external">Markdown</a> и могут быть (приблизительно) предварительно просмотрены с помощью любого средства <a href="http://tmpvar.com/markdown.html" class="external">предварительного просмотра Markdown</a> .

Для получения информации о качестве справочной документации и о том, как принять участие в спринтах документации и сообществе, см. Совет [TensorFlow 2 API Docs](https://docs.google.com/document/d/1e20k9CuaZ_-hp25-sSd8E8qldxKPKQR-SkwojYr_r-U/preview) .

### Версии и ветки

The site's [API reference](https://www.tensorflow.org/api_docs/python/tf) version defaults to the latest stable binary—this matches the package installed with `pip install tensorflow`.

The default TensorFlow package is built from the stable branch `rX.x` in the main <a href="https://github.com/tensorflow/tensorflow" class="external">tensorflow/tensorflow</a> repo. The reference documentation is generated from code comments and docstrings in the source code for <a href="https://www.tensorflow.org/code/tensorflow/python/" class="external">Python</a>, <a href="https://www.tensorflow.org/code/tensorflow/cc/" class="external">C++</a>, and <a href="https://www.tensorflow.org/code/tensorflow/java/" class="external">Java</a>.

Previous versions of the TensorFlow documentation are available as [rX.x branches](https://github.com/tensorflow/docs/branches) in the TensorFlow Docs repository. These branches are added when a new version is released.

### Документация по API сборки

Примечание. Этот шаг не требуется для редактирования или предварительного просмотра строк документации API, он необходим только для создания HTML-кода, используемого на tenorflow.org.

#### Справочник по Python

Пакет `tensorflow_docs` включает генератор [справочной документации Python API](https://www.tensorflow.org/api_docs/python/tf) . Установить:

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_5 &amp;lt;/code&amp;gt;
</pre>

To generate the TensorFlow 2 reference docs, use the `tensorflow/tools/docs/generate2.py` script:

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_7 &amp;lt;/code&amp;gt;
&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_8 &amp;lt;/code&amp;gt;
&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_9 &amp;lt;/code&amp;gt;
&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_10 &amp;lt;/code&amp;gt;
</pre>

Примечание. Этот сценарий использует *установленный* пакет TensorFlow для создания документов и работает только для TensorFlow 2.x.

## Описательная документация

TensorFlow [guides](https://www.tensorflow.org/guide) and [tutorials](https://www.tensorflow.org/tutorials) are written as <a href="https://guides.github.com/features/mastering-markdown/" class="external">Markdown</a> files and interactive <a href="https://jupyter.org/" class="external">Jupyter</a> notebooks. Notebooks can be run in your browser using <a href="https://colab.research.google.com/notebooks/welcome.ipynb" class="external">Google Colaboratory</a>. The narrative docs on [tensorflow.org](https://www.tensorflow.org) are built from the <a href="https://github.com/tensorflow/docs" class="external">tensorflow/docs</a> `master` branch. Older versions are available in GitHub on the `rX.x` release branches.

### Простые изменения

Самый простой способ напрямую обновлять документацию для файлов Markdown - использовать <a href="https://help.github.com/en/articles/editing-files-in-your-repository" class="external">веб-редактор файлов</a> GitHub. Просмотрите репозиторий [tensorflow / docs](https://github.com/tensorflow/docs/tree/master/site/en) и найдите Markdown, который примерно соответствует структуре URL-адреса <a href="https://www.tensorflow.org">tensorflow.org</a> . В правом верхнем углу окна просмотра файлов щелкните значок карандаша <svg version="1.1" width="14" height="16" viewbox="0 0 14 16" class="octicon octicon-pencil" aria-hidden="true"></svg><path fill-rule="evenodd" d="M0 12v3h3l8-8-3-3-8 8zm3 2H1v-2h1v1h1v1zm10.3-9.3L12 6 9 3l1.3-1.3a.996.996 0 0 1 1.41 0l1.59 1.59c.39.39.39 1.02 0 1.41z"></path> , чтобы открыть редактор файлов. Отредактируйте файл, а затем отправьте новый запрос на перенос.

### Настроить локальное репозиторий Git

Для редактирования нескольких файлов или более сложных обновлений лучше использовать локальный рабочий процесс Git для создания запроса на перенос.

Примечание. <a href="https://git-scm.com/" class="external">Git</a> - это система контроля версий с открытым исходным кодом (VCS), используемая для отслеживания изменений исходного кода. <a href="https://github.com" class="external">GitHub</a> - это онлайн-сервис, который предоставляет инструменты для совместной работы, которые работают с Git. См. <a href="https://help.github.com" class="external">Справку GitHub,</a> чтобы настроить учетную запись GitHub и начать работу.

Следующие шаги Git требуются только при первой настройке локального проекта.

#### Форк репозитория tensorflow / docs

На <a href="https://github.com/tensorflow/docs" class="external">странице tenorflow / docs</a> GitHub нажмите кнопку *Fork* <svg class="octicon octicon-repo-forked" viewbox="0 0 10 16" version="1.1" width="10" height="16" aria-hidden="true"></svg><path fill-rule="evenodd" d="M8 1a1.993 1.993 0 0 0-1 3.72V6L5 8 3 6V4.72A1.993 1.993 0 0 0 2 1a1.993 1.993 0 0 0-1 3.72V6.5l3 3v1.78A1.993 1.993 0 0 0 5 15a1.993 1.993 0 0 0 1-3.72V9.5l3-3V4.72A1.993 1.993 0 0 0 8 1zM2 4.2C1.34 4.2.8 3.65.8 3c0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2zm3 10c-.66 0-1.2-.55-1.2-1.2 0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2zm3-10c-.66 0-1.2-.55-1.2-1.2 0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2z"></path> чтобы создать собственную копию репо под вашей учетной записью GitHub. После разветвления вы несете ответственность за поддержание актуальности своей копии репо с помощью вышестоящего репозитория TensorFlow.

#### Клонируйте свое репо

Download a copy of *your* remote <var>username</var>/docs repo to your local machine. This is the working directory where you will make changes:

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_13 &amp;lt;/code&amp;gt;
&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_14 &amp;lt;/code&amp;gt;
</pre>

#### Добавьте вышестоящее репо, чтобы быть в курсе (необязательно)

Чтобы ваш локальный репозиторий синхронизировался с `tensorflow/docs` , добавьте *вышестоящий* пульт для загрузки последних изменений.

Примечание. Обязательно обновите локальное репо *перед тем, как* начать вклад. Регулярная синхронизация с восходящим потоком снижает вероятность <a href="https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line" class="external">конфликта слияния</a> при отправке запроса на перенос.

Добавьте пульт:

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_16 &amp;lt;/code&amp;gt;

# Просмотр удаленных репозиториев

&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_17 &amp;lt;/code&amp;gt;
origin git@github.com: &amp;lt;var&amp;gt; имя пользователя &amp;lt;/var&amp;gt; /docs.git (выборка)
origin git@github.com: &amp;lt;var&amp;gt; имя пользователя &amp;lt;/var&amp;gt; /docs.git (push)
&amp;lt;var&amp;gt; восходящий поток &amp;lt;/var&amp;gt; git@github.com: tensorflow / docs.git (выборка)
&amp;lt;var&amp;gt; восходящий поток &amp;lt;/var&amp;gt; git@github.com: tensorflow / docs.git (push)
</pre>

Обновлять:

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_18 &amp;lt;/code&amp;gt;
&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_19 &amp;lt;/code&amp;gt;

&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_20 &amp;lt;/code&amp;gt; # Отправка изменений в вашу учетную запись GitHub (по умолчанию origin)
</pre>

### Рабочий процесс GitHub

#### 1. Создайте новую ветку

После обновления репо из `tensorflow/docs` создайте новую ветку из локальной *главной* ветки:

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_22 &amp;lt;/code&amp;gt;

&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_23 &amp;lt;/code&amp;gt; # Список локальных веток
владелец
* &amp;lt;var&amp;gt; название-функции &amp;lt;/var&amp;gt;
</pre>

#### 2. Внесите изменения

Отредактируйте файлы в своем любимом редакторе и следуйте [руководству по стилю документации TensorFlow](./docs_style.md) .

Зафиксируйте изменение файла:

<pre class="prettyprint lang-bsh"># Просмотреть изменения

&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_24 &amp;lt;/code&amp;gt; # Посмотрите, какие файлы были изменены
&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_25 &amp;lt;/code&amp;gt; # Просмотр изменений в файлах

&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_26 &amp;lt;/code&amp;gt;
&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_27 &amp;lt;/code&amp;gt;
</pre>

При необходимости добавьте больше коммитов.

#### 3. Создайте запрос на перенос.

Загрузите локальную ветку в удаленное репозиторий GitHub (github.com/ <var>username</var> / docs):

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_28 &amp;lt;/code&amp;gt;
</pre>

After the push completes, a message may display a URL to automatically submit a pull request to the upstream repo. If not, go to the <a href="https://github.com/tensorflow/docs" class="external">tensorflow/docs</a> repo—or your own repo—and GitHub will prompt you to create a pull request.

#### 4. Обзор

Сопровождающие и другие участники рассмотрят ваш запрос на перенос. Примите участие в обсуждении и внесите требуемые изменения. Когда ваш запрос на вытягивание будет одобрен, он будет объединен с вышестоящим репозиторием документации TensorFlow.

Успех: ваши изменения приняты в документацию TensorFlow.

There is a separate publishing step to update [tensorflow.org](https://www.tensorflow.org) from the GitHub repo. Typically, changes are batched together and the site is updated on a regular cadence.

## Интерактивные блокноты

Хотя можно редактировать файл JSON записной книжки с помощью <a href="https://help.github.com/en/articles/editing-files-in-your-repository" class="external">веб-редактора</a> файлов GitHub, это не рекомендуется, поскольку искаженный JSON может повредить файл. Обязательно проверьте ноутбук перед отправкой запроса на перенос.

<a href="https://colab.research.google.com/notebooks/welcome.ipynb" class="external">Google Colaboratory</a> - это размещенная среда для записных книжек, которая упрощает редактирование и запуск документации для записных книжек. Блокноты в GitHub загружаются в Google Colab путем передачи пути к URL-адресу Colab, например блокнот, расположенный в GitHub здесь: <a href="https://github.com/tensorflow/docs/blob/master/site/en/tutorials/keras/classification.ipynb">https://github.com/tensorflow/docs/blob/master/site/en/tutorials/keras /classification.ipynb</a><br> можно загрузить в Google Colab по этому URL-адресу: <a href="https://colab.research.google.com/github/tensorflow/docs/blob/master/site/en/tutorials/keras/classification.ipynb">https://colab.research.google.com/github/tensorflow/docs/blob/master/site/en/tutorials/keras/classification.ipynb</a>

<!-- github.com path intentionally formatted to hide from import script. -->

Существует расширение <a href="https://chrome.google.com/webstore/detail/open-in-colab/iogfkhleblhcpcekbiedikdehleodpjo" class="external">Open in Colab для</a> Chrome, которое выполняет эту замену URL-адреса при просмотре записной книжки на GitHub. Это полезно при открытии записной книжки в вилке репо, потому что верхние кнопки всегда ссылаются на `master` ветку TensorFlow Docs.

### Форматирование ноутбука

Чтобы создать новую записную книжку, скопируйте и отредактируйте <a href="https://github.com/tensorflow/docs/blob/master/tools/templates/notebook.ipynb" external="class">шаблон записной книжки TensorFlow</a> .

Блокноты хранятся на диске в формате JSON, а различные реализации блокнотов по-разному форматируют JSON. Инструменты различий и контроль версий не очень хорошо справляются с этим, поэтому TensorFlow Docs обеспечивает стандартное форматирование записной книжки.

[Скрипт nbfmt.py](https://github.com/tensorflow/docs/blob/master/tools/nbfmt.py) применяет это форматирование:

```
# Run it on a single file

python nbfmt.py path/to/notebooks/example.ipynb

# Run  it on a whole directory

python nbfmt.py path/to/notebooks/
```

По тем же причинам выходные данные записной книжки должны быть очищены перед отправкой (за исключением некоторых особых случаев). В Colab для этого используется опция «частные выходы». Другие реализации ноутбуков не распознают эту опцию. Вы можете очистить выходные данные, передав `--preserve_outputs=False` в `nbfmt` :

```
python nbfmt.py --preserve_outputs=False path/to/notebooks/example.ipynb
```

### Редактировать в Colab

В среде Google Colab дважды щелкните ячейки, чтобы изменить текст и блоки кода. Текстовые ячейки используют Markdown и должны соответствовать [руководству по стилю документации TensorFlow](./docs_style.md) .

Загрузите файлы записной книжки из Colab с помощью *File&gt; Download .pynb* . Зафиксируйте этот файл в своем [локальном репозитории Git](##set_up_a_local_git_repo) и отправьте запрос на перенос.

Чтобы создать новую записную книжку, скопируйте и отредактируйте <a href="https://github.com/tensorflow/docs/blob/master/tools/templates/notebook.ipynb" external="class">шаблон записной книжки TensorFlow</a> .

### Рабочий процесс Colab-GitHub

Вместо того, чтобы загружать файл записной книжки и использовать локальный рабочий процесс Git, вы можете редактировать и обновлять разветвленное репозиторий GitHub прямо из Google Colab:

1. В репозитории с разветвленным <var>именем пользователя</var> / документами используйте веб-интерфейс GitHub для <a href="https://help.github.com/articles/creating-and-deleting-branches-within-your-repository" class="external">создания новой ветки</a> .
2. Перейдите к файлу записной книжки для редактирования.
3. Откройте блокнот в Google Colab: используйте замену URL или расширение *Open in Colab* Chrome.
4. Отредактируйте записную книжку в Colab.
5. Зафиксируйте изменения в своем репо из Colab с помощью *File&gt; Save a copy in GitHub ...* Диалог сохранения должен ссылаться на соответствующее репо и ветку. Добавьте содержательное сообщение о фиксации.
6. After saving, browse to your repo or the <a href="https://github.com/tensorflow/docs" class="external">tensorflow/docs</a> repo, GitHub should prompt you to create a pull request.
7. Запрос на вытягивание проверяется сопровождающими.

Успех: ваши изменения приняты в документацию TensorFlow.

## Переводы сообщества

Переводы сообщества - отличный способ сделать TensorFlow доступным во всем мире. Чтобы обновить перевод, найдите или добавьте файл в [языковом каталоге,](https://github.com/tensorflow/docs/tree/master/site) который соответствует той же структуре каталогов в каталоге `en/` . Документы на английском языке являются *источником истины,* и переводы должны как можно точнее следовать этим руководствам. Тем не менее, переводы написаны для сообществ, которым они служат. Если английская терминология, фразы, стиль или тон не переводятся на другой язык, используйте перевод, подходящий для читателя.

Примечание. Справочник по API *не* переведен для tenorflow.org.

Существуют языковые группы документов, которые упрощают организацию работы авторов перевода. Присоединяйтесь, если вы автор, рецензент или просто заинтересованы в создании контента TensorFlow.org для сообщества:

- Китайский (упрощенный): [docs-zh-cn@tensorflow.org](https://groups.google.com/a/tensorflow.org/forum/#!forum/docs-zh-cn)
- Итальянский: [docs-it@tensorflow.org](https://groups.google.com/a/tensorflow.org/forum/#!forum/docs-it)
- Японский: [docs-ja@tensorflow.org](https://groups.google.com/a/tensorflow.org/forum/#!forum/docs-ja)
- Корейский: [docs-ko@tensorflow.org](https://groups.google.com/a/tensorflow.org/forum/#!forum/docs-ko)
- Русский: [docs-ru@tensorflow.org](https://groups.google.com/a/tensorflow.org/forum/#!forum/docs-ru)
- Турецкий: [docs-tr@tensorflow.org](https://groups.google.com/a/tensorflow.org/forum/#!forum/docs-tr)

### Просмотр уведомлений

Все обновления документации требуют проверки. Чтобы более эффективно сотрудничать с сообществами переводчиков TensorFlow, вот несколько способов следить за деятельностью, зависящей от языка:

- Join a language group listed above to receive an email for any *created* pull request that touches the <code>&lt;a data-md-type="raw_html" href="https://github.com/tensorflow/docs/tree/master/site"&gt;site/&lt;var data-md-type="raw_html"&gt;lang&lt;/var&gt;&lt;/a&gt;</code> directory for that language.
- Add your GitHub username to the `site/<lang>/REVIEWERS` file to get automatically comment-tagged in a pull request. When comment-tagged, GitHub will send you notifications for all changes and discussion in that pull request.

### Поддерживайте актуальность кода при переводах

Для проекта с открытым исходным кодом, такого как TensorFlow, поддерживать документацию в актуальном состоянии непросто. После разговора с сообществом читатели переведенного контента будут терпеть текст, который немного устарел, но устаревший код расстраивает. Чтобы упростить синхронизацию кода, используйте инструмент [nb-code-sync](https://github.com/tensorflow/docs/blob/master/tools/nb_code_sync.py) для переведенных записных книжек:

<pre class="prettyprint lang-bsh">&amp;lt;code class = &amp;quot;devsite-terminal&amp;quot;&amp;gt; GL_CODE_37 &amp;lt;/code&amp;gt;
</pre>

Этот сценарий считывает ячейки кода языковой записной книжки и сравнивает их с английской версией. После удаления комментариев он сравнивает блоки кода и обновляет языковую записную книжку, если они отличаются. Этот инструмент особенно полезен с интерактивным рабочим процессом git для выборочного добавления фрагментов файла в коммит, используя: `git add --patch site/lang/notebook.ipynb`
