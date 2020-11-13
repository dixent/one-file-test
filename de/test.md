project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml Beschreibung: Die Application Shell-Architektur hält Ihre Benutzeroberfläche lokal und lädt Inhalte dynamisch, ohne die Verknüpfbarkeit und Erkennbarkeit des Webs zu beeinträchtigen.

{# wf_updated_on: 2019-05-02 #} {# wf_published_on: 2016-09-27 #} {# wf_blink_components: N / A #}

# Das App-Shell-Modell {: .page-title}

{% include "web / _shared / contributors / addyosmani.html"%}

Eine **Application Shell** (oder App Shell) -Architektur ist eine Möglichkeit, eine Progressive Web App zu erstellen, die zuverlässig und sofort auf den Bildschirmen Ihrer Benutzer geladen wird, ähnlich wie dies in nativen Anwendungen der Fall ist.

Die App "Shell" ist das minimale HTML, CSS und JavaScript, das für die Stromversorgung der Benutzeroberfläche erforderlich ist. Wenn sie offline zwischengespeichert wird, kann sie Benutzern bei wiederholten Besuchen eine **sofortige und zuverlässig gute Leistung** gewährleisten. Dies bedeutet, dass die Anwendungsshell nicht bei jedem Besuch des Benutzers aus dem Netzwerk geladen wird. Es werden nur die erforderlichen Inhalte aus dem Netzwerk benötigt.

Für [einseitige Anwendungen](https://en.wikipedia.org/wiki/Single-page_application) mit JavaScript-lastigen Architekturen ist eine Anwendungsshell ein praktischer Ansatz. Dieser Ansatz basiert auf dem aggressiven Zwischenspeichern der Shell (mithilfe eines [Servicemitarbeiters](/web/fundamentals/primers/service-worker/) ), um die Anwendung zum Laufen zu bringen. Als Nächstes wird der dynamische Inhalt für jede Seite mithilfe von JavaScript geladen. Eine App-Shell ist nützlich, um schnelles HTML ohne Netzwerk schnell auf den Bildschirm zu bringen.

<img src="images/appshell.png" alt="Application Shell-Architektur">

Anders ausgedrückt, die App-Shell ähnelt dem Code-Bundle, das Sie beim Erstellen einer nativen App in einem App Store veröffentlichen würden. Es ist das Grundgerüst Ihrer Benutzeroberfläche und die Kernkomponenten, die erforderlich sind, um Ihre App auf den Markt zu bringen, enthält jedoch wahrscheinlich keine Daten.

Hinweis: Probieren Sie das Codelab [First Progressive Web App](https://codelabs.developers.google.com/codelabs/your-first-pwapp/#0) aus, um zu erfahren, wie Sie Ihre erste Anwendungsshell für eine Wetter-App erstellen und implementieren. Das Video zum [sofortigen Laden mit dem App Shell-Modell](https://www.youtube.com/watch?v=QhUzmR8eZAo) führt dieses Muster ebenfalls durch.

### Wann wird das App-Shell-Modell verwendet?

Das Erstellen einer PWA bedeutet nicht, von vorne zu beginnen. Wenn Sie eine moderne einseitige App erstellen, verwenden Sie wahrscheinlich bereits etwas Ähnliches wie eine App-Shell, unabhängig davon, ob Sie es so nennen oder nicht. Die Details können etwas variieren, je nachdem, welche Bibliotheken oder Frameworks Sie verwenden, aber das Konzept selbst ist Framework-unabhängig.

Eine Anwendungs-Shell-Architektur ist am sinnvollsten für Apps und Websites mit relativ unveränderter Navigation, aber sich änderndem Inhalt. Eine Reihe moderner JavaScript-Frameworks und -Bibliotheken empfehlen bereits, Ihre Anwendungslogik vom Inhalt zu trennen, wodurch die Anwendung dieser Architektur einfacher wird. Für eine bestimmte Klasse von Websites, die nur statischen Inhalt haben, können Sie immer noch demselben Modell folgen, aber die Website besteht zu 100% aus einer App-Shell.

Informationen zum Erstellen einer App-Shell-Architektur durch Google finden Sie unter [Erstellen der progressiven Web-App für Google I / O 2016](/web/showcase/2016/iowa2016) . Diese reale App wurde mit einem SPA gestartet, um eine PWA zu erstellen, die Inhalte mithilfe eines Servicemitarbeiters vorspeichert, neue Seiten dynamisch lädt, ordnungsgemäß zwischen Ansichten wechselt und Inhalte nach dem ersten Laden wiederverwendet.

### Vorteile {: # App-Shell-Vorteile}

Zu den Vorteilen einer App-Shell-Architektur mit einem Servicemitarbeiter gehören:

- **Zuverlässige Leistung, die konstant schnell ist** . Wiederholte Besuche sind extrem schnell. Statische Assets und die Benutzeroberfläche (z. B. HTML, JavaScript, Bilder und CSS) werden beim ersten Besuch zwischengespeichert, sodass sie bei wiederholten Besuchen sofort geladen werden. Inhalte *können* beim ersten Besuch zwischengespeichert werden, werden jedoch normalerweise geladen, wenn sie benötigt werden.

- **Native-ähnliche Interaktionen** . Durch die Übernahme des App-Shell-Modells können Sie Erfahrungen mit sofortiger, nativer, anwendungsähnlicher Navigation und Interaktion mit Offline-Unterstützung erstellen.

- **Wirtschaftliche Nutzung von Daten** . Entwerfen Sie für eine minimale Datennutzung und gehen Sie beim Zwischenspeichern umsichtig vor, da das Auflisten nicht notwendiger Dateien (z. B. große Bilder, die nicht auf jeder Seite angezeigt werden) dazu führt, dass Browser mehr Daten herunterladen, als unbedingt erforderlich sind. Obwohl Daten in westlichen Ländern relativ billig sind, ist dies in Schwellenländern nicht der Fall, in denen Konnektivität teuer und Daten teuer sind.

## Anforderungen {: # App-Shell-Anforderungen}

Die App-Shell sollte idealerweise:

- Schnell laden
- Verwenden Sie so wenig Daten wie möglich
- Verwenden Sie statische Assets aus einem lokalen Cache
- Trennen Sie den Inhalt von der Navigation
- Abrufen und Anzeigen von seitenspezifischen Inhalten (HTML, JSON usw.)
- Optional können Sie dynamischen Inhalt zwischenspeichern

Die App-Shell hält Ihre Benutzeroberfläche lokal und ruft Inhalte dynamisch über eine API ab, ohne jedoch die Verknüpfbarkeit und Erkennbarkeit des Webs zu beeinträchtigen. Wenn der Benutzer das nächste Mal auf Ihre App zugreift, wird die neueste Version automatisch angezeigt. Es ist nicht erforderlich, neue Versionen herunterzuladen, bevor Sie es verwenden.

Hinweis: Mit der [Lighthouse-](https://github.com/googlechrome/lighthouse) Überwachungserweiterung können Sie überprüfen, ob Ihre PWA, die eine App-Shell verwendet, eine hohe Messlatte für die Leistung erreicht. [To the Lighthouse](https://www.youtube.com/watch?v=LZjQ25NRV-E) ist ein Vortrag, in dem die Optimierung einer PWA mit diesem Tool beschrieben wird.

## Erstellen Ihrer App-Shell {: # Erstellen Ihrer App-Shell}

Strukturieren Sie Ihre App für eine klare Unterscheidung zwischen der Seiten-Shell und dem dynamischen Inhalt. Im Allgemeinen sollte Ihre App die einfachste Shell laden, aber beim ersten Download genügend aussagekräftigen Seiteninhalt enthalten. Bestimmen Sie für jede Ihrer Datenquellen das richtige Gleichgewicht zwischen Geschwindigkeit und Datenaktualität.

<figure>
  <img src="images/wikipedia.jpg"
    alt="Offline Wikipedia app using an application shell with content caching">
  <figcaption>Jake Archibald’s <a href="https://wiki-offline.jakearchibald.com/wiki/Rick_and_Morty">offline Wikipedia application</a> is a good example of a PWA that uses an app shell model. It loads instantly on repeat visits, but dynamically fetches content using JS. This content is then cached offline for future visits.
</figcaption>
</figure>

### Beispiel-HTML für eine App-Shell {: # example-html-for-appshell}

In diesem Beispiel werden die Kernanwendungsinfrastruktur und die Benutzeroberfläche von den Daten getrennt. Es ist wichtig, das anfängliche Laden so einfach wie möglich zu halten, um nur das Seitenlayout anzuzeigen, sobald die Web-App geöffnet wird. Ein Teil davon stammt aus der Indexdatei Ihrer Anwendung (Inline-DOM, Stile), der Rest wird aus externen Skripten und Stylesheets geladen.

Die gesamte Benutzeroberfläche und Infrastruktur wird lokal mithilfe eines Servicemitarbeiters zwischengespeichert, sodass beim nachfolgenden Laden nur neue oder geänderte Daten abgerufen werden, anstatt alles laden zu müssen.

Ihre `index.html` Datei in Ihrem Arbeitsverzeichnis sollte ungefähr so aussehen wie der folgende Code. Dies ist eine Teilmenge des tatsächlichen Inhalts und keine vollständige Indexdatei. Schauen wir uns an, was es enthält.

- HTML und CSS für das "Skelett" Ihrer Benutzeroberfläche mit Navigations- und Inhaltsplatzhaltern.
- Eine externe JavaScript-Datei (app.js) zur Verarbeitung der Navigations- und Benutzeroberflächenlogik sowie der Code zum Anzeigen von vom Server abgerufenen Posts und zum lokalen Speichern mithilfe eines Speichermechanismus wie IndexedDB.
- Ein Web-App-Manifest und ein Service Worker Loader, um Offline-Funktionen zu aktivieren.

<div class="clearfix"></div>

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>App Shell</title>
  <link rel="manifest" href="/manifest.json">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" type="text/css" href="styles/inline.css">
</head>

<body>
  <header class="header">
    <h1 class="header__title">App Shell</h1>
  </header>

  <nav class="nav">
  ...
  </nav>

  <main class="main">
  ...
  </main>

  <div class="dialog-container">
  ...
  </div>

  <div class="loader">
    <!-- Show a spinner or placeholders for content -->
  </div>

  <script src="app.js" async></script>
  <script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      // Registration was successful
      console.log('ServiceWorker registration successful with scope: ', registration.scope);
    }).catch(function(err) {
      // registration failed :(
      console.log('ServiceWorker registration failed: ', err);
    });
  }
  </script>
</body>
</html>
```

<div class="clearfix"></div>

Note: See [Your First Progressive Web App](/web/fundamentals/codelabs/your-first-pwapp/) for more on using an application shell and server-side rendering for content. An app shell can be implemented using any library or framework as covered in our <a href="https://www.youtube.com/watch?v=srdKq0DckXQ">Progressive Web Apps across all frameworks</a> talk. Samples are available using Polymer (<a href="https://shop.polymer-project.org">Shop</a>) and React (<a href="https://github.com/insin/react-hn">ReactHN</a>, <a href="https://github.com/GoogleChrome/sw-precache/tree/master/app-shell-demo">iFixit</a>).

### Zwischenspeichern der Anwendungsshell {: # App-Shell-Caching}

Eine App-Shell kann mithilfe eines manuell geschriebenen Service Workers oder eines generierten Service Workers mithilfe eines statischen Asset-Precaching-Tools wie [sw-precache zwischengespeichert werden](https://github.com/googlechrome/sw-precache) .

Hinweis: Die Beispiele dienen nur zur allgemeinen Information und zur Veranschaulichung. Die tatsächlich verwendeten Ressourcen unterscheiden sich wahrscheinlich für Ihre Anwendung.

#### Manuelles Zwischenspeichern der App-Shell

Im Folgenden finden Sie einen Beispielcode für Service Worker, mit dem statische Ressourcen aus der App-Shell mithilfe des `install` von Service Worker in der [Cache-API zwischengespeichert](https://developer.mozilla.org/en-US/docs/Web/API/Cache) werden:

```
var cacheName = 'shell-content';
var filesToCache = [
  '/css/styles.css',
  '/js/scripts.js',
  '/images/logo.svg',

  '/offline.html',

  '/',
];

self.addEventListener('install', function(e) {
  console.log('[ServiceWorker] Install');
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log('[ServiceWorker] Caching app shell');
      return cache.addAll(filesToCache);
    })
  );
});
```

#### Verwenden von sw-precache zum Zwischenspeichern der App-Shell

Der von sw-precache generierte Service Worker speichert die Ressourcen, die Sie im Rahmen Ihres Erstellungsprozesses konfigurieren, zwischen und stellt sie bereit. Sie können jede HTML-, JavaScript- und CSS-Datei, aus der Ihre App-Shell besteht, vorab zwischenspeichern. Alles funktioniert sowohl offline als auch wird bei nachfolgenden Besuchen ohne zusätzlichen Aufwand schnell geladen.

Hier ist ein grundlegendes Beispiel für die Verwendung von sw-precache als Teil eines [gulp-](http://gulpjs.com) Erstellungsprozesses:

```
gulp.task('generate-service-worker', function(callback) {
  var path = require('path');
  var swPrecache = require('sw-precache');
  var rootDir = 'app';

  swPrecache.write(path.join(rootDir, 'service-worker.js'), {
    staticFileGlobs: [rootDir + '/**/*.{js,html,css,png,jpg,gif}'],
    stripPrefix: rootDir
  }, callback);
});
```

To learn more about static asset caching, see the [Adding a Service Worker with sw-precache](https://codelabs.developers.google.com/codelabs/sw-precache/index.html?index=..%2F..%2Findex#0) codelab.

Hinweis: sw-precache ist nützlich für das Offline-Caching Ihrer statischen Ressourcen. Für Laufzeit- / dynamische Ressourcen empfehlen wir die Verwendung unserer kostenlosen Bibliothek [sw-toolbox](https://github.com/googlechrome/sw-toolbox) .

## Schlussfolgerung {: #conclusion}

Eine App-Shell, die Service Worker verwendet, ist ein leistungsstarkes Muster für das Offline-Caching, bietet jedoch auch erhebliche Leistungssteigerungen in Form von sofortigem Laden für wiederholte Besuche bei Ihrer PWA. Sie können Ihre Anwendungsshell zwischenspeichern, damit sie offline funktioniert, und ihren Inhalt mit JavaScript füllen.

Bei wiederholten Besuchen können Sie auf diese Weise ohne Netzwerk aussagekräftige Pixel auf dem Bildschirm anzeigen, selbst wenn Ihr Inhalt irgendwann von dort stammt.

## Feedback Feedback }

{% include "web / _shared / hilfreiche.html"%}
