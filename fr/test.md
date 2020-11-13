project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml description: L&#39;architecture du shell d&#39;application maintient votre interface utilisateur locale et charge le contenu de manière dynamique sans sacrifier la possibilité de liaison et de découverte du Web.

{# wf_updated_on: 2019-05-02 #} {# wf_published_on: 2016-09-27 #} {# wf_blink_components: N / A #}

# Le modèle App Shell {: .page-title}

{% include "web / _shared / contributors / addyosmani.html"%}

Une architecture **shell d&#39;application** (ou shell d&#39;application) est un moyen de créer une application Web progressive qui se charge de manière fiable et instantanée sur les écrans de vos utilisateurs, comme vous le voyez dans les applications natives.

Le «shell» de l&#39;application est le minimum de HTML, CSS et JavaScript requis pour alimenter l&#39;interface utilisateur et, lorsqu&#39;il est mis en cache hors ligne, peut garantir **des performances instantanées et fiables** aux utilisateurs lors de visites répétées. Cela signifie que le shell de l&#39;application n&#39;est pas chargé depuis le réseau à chaque visite de l&#39;utilisateur. Seul le contenu nécessaire du réseau est nécessaire.

Pour les [applications d&#39;une seule page](https://en.wikipedia.org/wiki/Single-page_application) avec des architectures lourdes de JavaScript, un shell d&#39;application est une approche incontournable. Cette approche repose sur la mise en cache agressive du shell (à l&#39;aide d&#39;un [service worker](/web/fundamentals/primers/service-worker/) ) pour que l&#39;application s&#39;exécute. Ensuite, le contenu dynamique se charge pour chaque page à l&#39;aide de JavaScript. Un shell d&#39;application est utile pour afficher rapidement du HTML initial à l&#39;écran sans réseau.

<img src="images/appshell.png" alt="Architecture du shell d&#39;application">

En d&#39;autres termes, le shell de l&#39;application est similaire à l&#39;ensemble de code que vous publieriez dans un magasin d&#39;applications lors de la création d&#39;une application native. Il s&#39;agit du squelette de votre interface utilisateur et des composants de base nécessaires pour faire décoller votre application, mais ne contient probablement pas les données.

Note: Try the [First Progressive Web App](https://codelabs.developers.google.com/codelabs/your-first-pwapp/#0) codelab to learn how to architectect and implement your first application shell for a weather app. The [Instant Loading with the App Shell model](https://www.youtube.com/watch?v=QhUzmR8eZAo) video also walks through this pattern.

### Quand utiliser le modèle de shell d&#39;application

Construire une PWA ne signifie pas partir de zéro. Si vous créez une application moderne d&#39;une seule page, vous utilisez probablement déjà quelque chose de similaire à un shell d&#39;application, que vous l&#39;appeliez ainsi ou non. Les détails peuvent varier un peu en fonction des bibliothèques ou des frameworks que vous utilisez, mais le concept lui-même est indépendant du framework.

Une architecture de shell d&#39;application est la plus logique pour les applications et les sites avec une navigation relativement stable mais un contenu changeant. Un certain nombre de frameworks et de bibliothèques JavaScript modernes encouragent déjà à séparer la logique de votre application de son contenu, ce qui rend cette architecture plus simple à appliquer. Pour une certaine classe de sites Web qui n&#39;ont que du contenu statique, vous pouvez toujours suivre le même modèle, mais le site est 100% shell d&#39;application.

Pour voir comment Google a construit une architecture de shell d&#39;application, jetez un œil à la [création de l&#39;application Web progressive Google I / O 2016](/web/showcase/2016/iowa2016) . Cette application du monde réel a démarré avec un SPA pour créer une PWA qui met en cache le contenu à l&#39;aide d&#39;un service worker, charge dynamiquement de nouvelles pages, transite gracieusement entre les vues et réutilise le contenu après le premier chargement.

### Avantages {: # app-shell-Benefits}

Les avantages d&#39;une architecture de shell d&#39;application avec un service worker incluent:

- **Des performances fiables et toujours rapides** . Les visites répétées sont extrêmement rapides. Les actifs statiques et l&#39;interface utilisateur (par exemple HTML, JavaScript, images et CSS) sont mis en cache lors de la première visite afin de se charger instantanément lors des visites répétées. Le contenu *peut* être mis en cache lors de la première visite, mais il est généralement chargé lorsqu&#39;il est nécessaire.

- **Interactions de type natif** . En adoptant le modèle de shell d&#39;application, vous pouvez créer des expériences avec une navigation et des interactions instantanées de type application native, avec une prise en charge hors ligne.

- **Utilisation économique des données** . Concevez pour une utilisation minimale des données et soyez judicieux dans ce que vous mettez en cache, car la liste des fichiers qui ne sont pas essentiels (de grandes images qui ne sont pas affichées sur chaque page, par exemple) conduit les navigateurs à télécharger plus de données que ce qui est strictement nécessaire. Même si les données sont relativement bon marché dans les pays occidentaux, ce n&#39;est pas le cas dans les marchés émergents où la connectivité est chère et les données coûteuses.

## Exigences {: # app-shell-requirements}

Le shell de l&#39;application devrait idéalement:

- Chargez vite
- Utilisez le moins de données possible
- Utiliser des actifs statiques à partir d&#39;un cache local
- Séparer le contenu de la navigation
- Récupérer et afficher du contenu spécifique à la page (HTML, JSON, etc.)
- En option, mettez en cache le contenu dynamique

Le shell de l&#39;application maintient votre interface utilisateur locale et extrait le contenu de manière dynamique via une API, mais ne sacrifie pas la possibilité de liaison et de découverte du Web. La prochaine fois que l&#39;utilisateur accède à votre application, la dernière version s&#39;affiche automatiquement. Il n&#39;est pas nécessaire de télécharger de nouvelles versions avant de l&#39;utiliser.

Remarque: L&#39;extension d&#39;audit [Lighthouse](https://github.com/googlechrome/lighthouse) peut être utilisée pour vérifier si votre PWA utilisant un shell d&#39;application atteint un niveau élevé de performances. [To the Lighthouse](https://www.youtube.com/watch?v=LZjQ25NRV-E) est un exposé qui décrit l&#39;optimisation d&#39;une PWA à l&#39;aide de cet outil.

## Construire votre application shell {: # building-your-app-shell}

Structurez votre application pour une distinction claire entre le shell de page et le contenu dynamique. En général, votre application doit charger le shell le plus simple possible, mais inclure suffisamment de contenu de page significatif avec le téléchargement initial. Déterminez le juste équilibre entre la vitesse et la fraîcheur des données pour chacune de vos sources de données.

&lt;figure&gt;
  &lt;img src="images/wikipedia.jpg"
    alt="Offline Wikipedia app using an application shell with content caching"&gt;
  &lt;figcaption&gt;Jake Archibald’s &lt;a href="https://wiki-offline.jakearchibald.com/wiki/Rick_and_Morty"&gt;offline Wikipedia application&lt;/a&gt; is a good example of a PWA that uses an app shell model. It loads instantly on repeat visits, but dynamically fetches content using JS. This content is then cached offline for future visits.
&lt;/figcaption&gt;
&lt;/figure&gt;

### Exemple de code HTML pour un shell d&#39;application {: # example-html-for-appshell}

Cet exemple sépare l&#39;infrastructure d&#39;application principale et l&#39;interface utilisateur des données. Il est important de garder le chargement initial aussi simple que possible pour n&#39;afficher que la mise en page de la page dès que l&#39;application Web est ouverte. Une partie provient du fichier d&#39;index de votre application (DOM en ligne, styles) et le reste est chargé à partir de scripts externes et de feuilles de style.

L&#39;ensemble de l&#39;interface utilisateur et de l&#39;infrastructure est mis en cache localement à l&#39;aide d&#39;un service worker afin que lors des chargements suivants, seules les données nouvelles ou modifiées soient récupérées, au lieu d&#39;avoir à tout charger.

Votre fichier `index.html` dans votre répertoire de travail doit ressembler au code suivant. Il s&#39;agit d&#39;un sous-ensemble du contenu réel et non d&#39;un fichier d&#39;index complet. Regardons ce qu&#39;il contient.

- HTML et CSS pour le «squelette» de votre interface utilisateur avec navigation et espaces réservés de contenu.
- Un fichier JavaScript externe (app.js) pour gérer la navigation et la logique de l&#39;interface utilisateur, ainsi que le code pour afficher les publications récupérées sur le serveur et les stocker localement à l&#39;aide d&#39;un mécanisme de stockage tel que IndexedDB.
- Un manifeste d&#39;application Web et un chargeur de service worker pour activer les fonctionnalités hors ligne.

<div class="clearfix"></div>

```
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;meta charset="utf-8"&gt;
  &lt;title&gt;App Shell&lt;/title&gt;
  &lt;link rel="manifest" href="/manifest.json"&gt;
  &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
  &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
  &lt;link rel="stylesheet" type="text/css" href="styles/inline.css"&gt;
&lt;/head&gt;

&lt;body&gt;
  &lt;header class="header"&gt;
    &lt;h1 class="header__title"&gt;App Shell&lt;/h1&gt;
  &lt;/header&gt;

  &lt;nav class="nav"&gt;
  ...
  &lt;/nav&gt;

  &lt;main class="main"&gt;
  ...
  &lt;/main&gt;

  &lt;div class="dialog-container"&gt;
  ...
  &lt;/div&gt;

  &lt;div class="loader"&gt;
    &lt;!-- Show a spinner or placeholders for content --&gt;
  &lt;/div&gt;

  &lt;script src="app.js" async&gt;&lt;/script&gt;
  &lt;script&gt;
  if (&#39;serviceWorker&#39; in navigator) {
    navigator.serviceWorker.register(&#39;/sw.js&#39;).then(function(registration) {
      // Registration was successful
      console.log(&#39;ServiceWorker registration successful with scope: &#39;, registration.scope);
    }).catch(function(err) {
      // registration failed :(
      console.log(&#39;ServiceWorker registration failed: &#39;, err);
    });
  }
  &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```

<div class="clearfix"></div>

Remarque: consultez [Votre première application Web progressive](/web/fundamentals/codelabs/your-first-pwapp/) pour en savoir plus sur l&#39;utilisation d&#39;un shell d&#39;application et le rendu côté serveur pour le contenu. Un shell d&#39;application peut être implémenté à l&#39;aide de n&#39;importe quelle bibliothèque ou framework comme décrit dans nos <a href="https://www.youtube.com/watch?v=srdKq0DckXQ">Progressive Web Apps sur tous les frameworks</a> . Les échantillons sont disponibles en utilisant Polymer ( <a href="https://shop.polymer-project.org">Shop</a> ) et React ( <a href="https://github.com/insin/react-hn">ReactHN</a> , <a href="https://github.com/GoogleChrome/sw-precache/tree/master/app-shell-demo">iFixit</a> ).

### Mise en cache du shell d&#39;application {: # app-shell-caching}

Un shell d&#39;application peut être mis en cache à l&#39;aide d&#39;un service worker écrit manuellement ou d&#39;un service worker généré à l&#39;aide d&#39;un outil de précaching d&#39;actif statique tel que [sw-precache](https://github.com/googlechrome/sw-precache) .

Remarque: les exemples sont fournis à titre d&#39;information générale et à des fins d&#39;illustration uniquement. Les ressources réelles utilisées seront probablement différentes pour votre application.

#### Mettre en cache le shell de l&#39;application manuellement

Vous trouverez ci-dessous un exemple de code de service worker qui met en cache les ressources statiques du shell de l&#39;application dans l&#39; [API Cache à l&#39;](https://developer.mozilla.org/en-US/docs/Web/API/Cache) aide de l&#39;événement d&#39; `install` l&#39;agent de service:

```
var cacheName = &#39;shell-content&#39;;
var filesToCache = [
  &#39;/css/styles.css&#39;,
  &#39;/js/scripts.js&#39;,
  &#39;/images/logo.svg&#39;,

  &#39;/offline.html&#39;,

  &#39;/&#39;,
];

self.addEventListener(&#39;install&#39;, function(e) {
  console.log(&#39;[ServiceWorker] Install&#39;);
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log(&#39;[ServiceWorker] Caching app shell&#39;);
      return cache.addAll(filesToCache);
    })
  );
});
```

#### Utilisation de sw-precache pour mettre en cache le shell de l&#39;application

Le service worker généré par sw-precache mettra en cache et servira les ressources que vous configurez dans le cadre de votre processus de génération. Vous pouvez le mettre en cache avant chaque fichier HTML, JavaScript et CSS qui constitue le shell de votre application. Tout fonctionnera à la fois hors ligne et se chargera rapidement lors des visites suivantes sans effort supplémentaire.

Voici un exemple de base de l&#39; utilisation sw-precache dans le cadre d&#39;un [gulp](http://gulpjs.com) processus de construction:

```
gulp.task(&#39;generate-service-worker&#39;, function(callback) {
  var path = require(&#39;path&#39;);
  var swPrecache = require(&#39;sw-precache&#39;);
  var rootDir = &#39;app&#39;;

  swPrecache.write(path.join(rootDir, &#39;service-worker.js&#39;), {
    staticFileGlobs: [rootDir + &#39;/**/*.{js,html,css,png,jpg,gif}&#39;],
    stripPrefix: rootDir
  }, callback);
});
```

Pour en savoir plus sur la mise en cache des actifs statiques, consultez l&#39; [onglet Ajout d&#39;un Service Worker avec sw-](https://codelabs.developers.google.com/codelabs/sw-precache/index.html?index=..%2F..%2Findex#0) precache.

Remarque: sw-precache est utile pour la mise en cache hors ligne de vos ressources statiques. Pour les ressources d&#39;exécution / dynamiques, nous vous recommandons d&#39;utiliser notre bibliothèque gratuite [sw-toolbox](https://github.com/googlechrome/sw-toolbox) .

## Conclusion {: #conclusion}

Un shell d&#39;application utilisant Service worker est un modèle puissant pour la mise en cache hors ligne, mais il offre également des gains de performances significatifs sous la forme d&#39;un chargement instantané pour les visites répétées sur votre PWA. Vous pouvez mettre en cache votre shell d&#39;application pour qu&#39;il fonctionne hors ligne et alimenter son contenu à l&#39;aide de JavaScript.

Lors de visites répétées, cela vous permet d&#39;obtenir des pixels significatifs sur l&#39;écran sans le réseau, même si votre contenu vient finalement de là.

## Commentaires {: #feedback}

{% include "web / _shared / help.html"%}
