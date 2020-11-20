---
"$title": Cree páginas AMP receptivas
"$order": '5'
description: 'El diseño web receptivo consiste en crear páginas web fluidas que respondan a las necesidades de los usuarios: páginas que se adapten al tamaño y la orientación de la pantalla de su dispositivo. Puedes lograr ...'
formats:
- sitios web
- correo electrónico
- anuncios
- cuentos
components:
- iframe
- Youtube
author: bpaduch
contributors:
- pbakaus
---

## Introducción

El diseño web receptivo consiste en crear páginas web fluidas que respondan a las necesidades del usuario: páginas que se adapten al tamaño y la orientación de la pantalla de su dispositivo. Puede lograr esto fácilmente en AMP. AMP es compatible con todas las categorías de pantallas y dispositivos y proporciona componentes de respuesta integrados.

En esta guía, le mostraremos cómo puede implementar fácilmente estos fundamentos receptivos en AMP:

- [Controlando la ventana gráfica](#controlling-the-viewport)
- [Crear un diseño receptivo](#creating-a-responsive-layout)
- [Medios de escala](#scaling-media-for-the-page)

[video src = 'https: //www.youtube.com/watch? v = XDvbJ2apaiA' caption = 'Obtén más información sobre el diseño adaptable en AMP en este video.']

## Controlando la ventana gráfica<a name="controlling-the-viewport"></a>

[filter formats="websites, ads, stories"] Para optimizar su página web de modo que el contenido se adapte a la ventana del navegador y se adapte a cualquier dispositivo, debe especificar un elemento de ventana gráfica `meta` . El elemento viewport le indica al navegador cómo escalar y dimensionar el área visible (el viewport) de la página web.

Pero, ¿qué valores debería utilizar? Bueno, en AMP, eso ya está explicado. Como parte del [marcado obligatorio](../../../../documentation/guides-and-tutorials/learn/spec/amp-boilerplate.md) para las páginas AMP, debe especificar la siguiente ventana gráfica:

```html
<meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
```

Estas son las configuraciones típicas de la ventana gráfica que usaría para un sitio receptivo. Aunque `initial-scale=1` no es necesario para una página AMP válida, se recomienda porque establece el nivel de zoom en 1 cuando la página se carga por primera vez. [/filter]

[filter formats="email"] Esta sección solo es válida para sitios web, anuncios e historias de AMP. [/filter]

## Crear un diseño receptivo<a name="creating-a-responsive-layout"></a>

En el diseño receptivo, puede usar consultas CSS [`@media`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media) para adaptar el estilo de su página web a varias dimensiones de pantalla sin tener que alterar el contenido de la página. En AMP, puede seguir utilizando esas mismas consultas CSS `@media` . Además, para un control más preciso sobre un elemento AMP, puede especificar el atributo de `media` en el elemento. Esto es particularmente útil cuando necesita mostrar u ocultar un elemento basado en una consulta de medios. Consulte la sección [Cambiar la dirección de arte de una imagen](#changing-the-art-direction-of-an-image) para ver un ejemplo que utiliza el atributo `media` .

Hacer que cada elemento cambie de tamaño para adaptarse a una pantalla puede ser complicado <sup><a href="#fn1" id="ref1">*</a></sup> . Sin embargo, en AMP, puede hacer que un elemento responda fácilmente con solo especificar el atributo `"layout=responsive"` junto con los atributos de `width` y `height` del elemento. Cuando aplica el diseño `responsive` a un elemento, ese elemento cambiará automáticamente de tamaño al ancho de su elemento contenedor, y la altura cambiará según la relación de aspecto especificada por el `width` y la `height` . Casi todos los elementos de AMP admiten un diseño `responsive` ; consulte la documentación de referencia del elemento para ver qué diseños son compatibles.

Aunque puede hacer que los elementos respondan fácilmente con `"layout=responsive"` , aún debe considerar cómo aparecen sus elementos en todos los tamaños de pantalla, incluidos el escritorio y la tableta. Un error común es permitir que una imagen ocupe todo el ancho de la pantalla, lo que alarga la imagen más allá de su tamaño previsto, lo que provoca una mala experiencia para los usuarios de pantalla ancha. De forma predeterminada, los elementos con `layout=responsive` tomarán el ancho completo del contenedor del elemento, que a menudo no tiene restricciones de ancho (es decir, ancho = 100%). Puede mejorar la apariencia de las imágenes simplemente restringiendo el ancho del contenedor de la imagen. Por ejemplo, al establecer una regla de "ancho máximo" en el "cuerpo" o "principal", puede restringir todas las imágenes a un ancho máximo específico.

##### Ejemplo: restringir el ancho de las imágenes receptivas

En el siguiente ejemplo, tenemos una imagen de flores (640 x 427 px) que queremos mostrar en todos los tamaños de pantalla, por lo que especificamos el `width` y el `height` y configuramos el diseño para que `responsive` .

[example preview="top-frame" playground="true"]
```html
<div class="resp-img">
  <amp-img alt="flowers"
    src="{{server_for_email}}/static/inline-examples/images/flowers.jpg"
    layout="responsive"
    width="640"
    height="427"></amp-img>
</div>
```
[/example]

Sin embargo, queremos que la imagen no se extienda más allá de su tamaño previsto, por lo que establecemos el `max-width` en el contenedor en 700 px mediante CSS personalizado:

```html
<style amp-custom>
.resp-img {
    max-width: 700px;
  }
</style>
```

[tip type="read-on"] SIGUE **LEER:** Para obtener más información sobre los diferentes diseños en AMP, consulte la guía de [consultas de diseño y medios](control_layout.md#the-layout-attribute) . [/tip]

<a id="fn1"></a>[tip type="note"] **¿Por qué es complicado cambiar el tamaño de los elementos para que se ajusten a la pantalla cuando puedo hacerlo fácilmente con el estilo `width=100%` ?**

La parte complicada es que los elementos de respuesta se representen en la página sin afectar negativamente las métricas de rendimiento o la experiencia del usuario. Sí, puede obtener imágenes fácilmente para que se ajusten a la pantalla con "ancho = 100%", pero hay golpes de rendimiento. El navegador debe descargar la imagen primero para obtener las dimensiones de la imagen, luego cambiar el tamaño de la imagen de manera apropiada para el tamaño de la pantalla y, finalmente, reajustar y volver a pintar la página. En AMP, la ruta de renderizado se optimiza para que primero se distribuya la página, dejando de lado los marcadores de posición para las imágenes según las dimensiones proporcionadas en [`amp-img`](../../../../documentation/components/reference/amp-img.md) (usando esos números para establecer la relación de aspecto), luego se descargan los recursos y el La página está pintada. No se requiere reflujo. [/tip]

## Escalado de medios para la página<a name="scaling-media-for-the-page"></a>

Probablemente el aspecto más desafiante del diseño receptivo es mostrar los medios correctamente en la página para que responda a las características de la pantalla. En esta sección, veremos cómo puede incrustar videos e imágenes receptivos en páginas AMP.

### Insertar videos

Cuando incluye un video en su página web, desea asegurarse de que el usuario pueda ver el contenido del video y los controles del video (es decir, sin desbordes). Por lo general, lo logrará con una combinación de consultas de medios CSS, un contenedor y otro CSS. En AMP, solo necesita agregar el elemento de video a su página y especificar `layout=responsive` en el elemento, sin CSS adicional.

##### Ejemplo: incrustar un video de YouTube

En el siguiente ejemplo, queremos mostrar un video de YouTube incrustado que responde al tamaño y orientación de la pantalla del dispositivo. Al agregar `"layout=responsive"` al elemento [`amp-youtube`](../../../../documentation/components/reference/amp-youtube.md) , el video cambia de tamaño para adaptarse a la viuda y su relación de aspecto se mantiene de acuerdo con el `width` y `height` especificados.

[example preview="top-frame" playground="true" imports="amp-youtube:0.1"]
```html
<amp-youtube data-videoid="lBTCB7yLs8Y"
  layout="responsive"
  width="560"
  height="315">
</amp-youtube>
```
[/example]

Hay muchos tipos de videos que puede agregar a sus páginas AMP. Para obtener detalles, consulte la lista de [componentes de medios](../../../../documentation/components/index.html#media) disponibles.

### Visualización de imágenes receptivas<a name="displaying-responsive-images"></a>

Las imágenes constituyen una gran parte de una página web (aproximadamente el [65% de los bytes de la página](http://httparchive.org/interesting.php#bytesperpage) ). Como mínimo, sus imágenes deben ser visibles en varios tamaños y orientaciones de pantalla (es decir, el usuario no tiene que desplazarse, pellizcar / hacer zoom para ver la imagen completa). Eso se hace fácilmente en AMP mediante el atributo `"layout=responsive"` (consulte [Incluir imágenes en AMP](../../../../documentation/guides-and-tutorials/develop/media_iframes_3p/index.md) ). Además de la imagen receptiva básica, es posible que desee ofrecer varios recursos de imagen para:

- [Sirva imágenes nítidas con la resolución correcta](#serving-crisp-images-for-the-right-resolution)
- [Cambiar la dirección de arte de una imagen](#changing-the-art-direction-of-an-image)
- [Proporcionar formatos de imagen optimizados](#providing-optimized-images)

#### Sirve imágenes nítidas con la resolución correcta<a name="serving-crisp-images-for-the-right-resolution"></a>

Para pantallas de alta resolución (por ejemplo, pantalla Retina), debe proporcionar imágenes que se vean nítidas y nítidas; sin embargo, no desea utilizar esa misma imagen en dispositivos de baja resolución porque eso provocará un tiempo de carga adicional innecesario. En las páginas que no son AMP y AMP, puede mostrar la imagen correcta para la densidad de píxeles de la pantalla utilizando `srcset` con el descriptor de ancho ( `w` ).

[tip type="note"] **NOTA -** El selector srcset basado en DPR ( `x` ) también funciona; sin embargo, para mayor flexibilidad, recomendamos usar el selector `w` . Anteriormente (en la antigua propuesta de srcset), el descriptor `w` describía el ancho de la ventana gráfica, pero ahora describe el ancho del archivo fuente de la imagen, lo que permite al agente de usuario calcular la densidad de píxeles efectiva de cada imagen y elegir la imagen adecuada para renderizar. . [/tip]

##### Ejemplo: visualización de una imagen nítida que se ajusta a la pantalla

En el siguiente ejemplo, tenemos varios archivos de imagen que tienen la misma relación de aspecto pero diferentes resoluciones. Al proporcionar varias resoluciones de imagen, el navegador puede elegir la imagen que mejor se adapte a la resolución del dispositivo. Además, hemos especificado el tamaño para renderizar la imagen en:

- Para un ancho de ventana de hasta 400 px, renderice la imagen al 100% del ancho de la ventana.
- Para un ancho de ventana gráfica de hasta 900 px, renderice la imagen al 75% del ancho de la ventana.
- Para todo lo que supere los 900 px, renderiza la imagen a 600 px de ancho.

[example preview="top-frame" playground="true"]
```html
<amp-img alt="apple"
  src="{{server_for_email}}/static/inline-examples/images/apple.jpg"
  height="596"
  width="900"
  srcset="{{server_for_email}}/static/inline-examples/images/apple-900.jpg 900w,
            {{server_for_email}}/static/inline-examples/images/apple-800.jpg 800w,
            {{server_for_email}}/static/inline-examples/images/apple-700.jpg 700w,
            {{server_for_email}}/static/inline-examples/images/apple-600.jpg 600w,
            {{server_for_email}}/static/inline-examples/images/apple-500.jpg 500w,
            {{server_for_email}}/static/inline-examples/images/apple-400.jpg 400w"
  sizes="(max-width: 400px) 100vw,
            (max-width: 900px) 75vw, 600px">
</amp-img>
```
[/example]

Por ejemplo, digamos que tenemos un dispositivo que tiene un ancho de ventana de 412 px y un DPR de 2.6. Según el código anterior, la imagen debe mostrarse al 75% del ancho de la ventana gráfica, por lo que el navegador elige una imagen cercana a 803 px (412 * .75 * 2.6), que resulta ser `apple-800.jpg` .

[tip type="read-on"] SIGUE **LEER:** Para obtener más información sobre el uso de srcset y tamaños en AMP, consulte la guía [Dirección de arte con srcset, tamaños y alturas](art_direction.md) . [/tip]

#### Cambiar la dirección de arte de una imagen<a name="changing-the-art-direction-of-an-image"></a>

La dirección de arte se refiere a adaptar las características visuales de una imagen para ciertos puntos de corte. Por ejemplo, en lugar de simplemente escalar una imagen a medida que la pantalla se estrecha, es posible que desee mostrar una versión recortada de la imagen que restrinja el enfoque de la imagen o puede que desee mostrar imágenes completamente diferentes en los diferentes puntos de interrupción. En HTML, puede lograr esto utilizando el elemento de `picture` . En AMP, la dirección de arte se puede lograr utilizando el atributo `media` .

##### Ejemplo: imágenes de diferentes tamaños para diferentes puntos de interrupción

En el siguiente ejemplo, tenemos 3 imágenes recortadas diferentes de un gato que queremos mostrar en diferentes puntos de interrupción. Entonces, si el ancho de la ventana gráfica es:

- 670 px o más, muestra `cat-large.jpg` (650 x 340 px)
- 470 - 669 px, mostrar `cat-medium.jpg` (450 x 340 px)
- 469 px o menos, muestra `cat-small.jpg` (226 x 340 px)

[tip type="note"] **NOTA -** Como queríamos que las imágenes fueran de tamaño fijo (es decir, no sesgadas), no especificamos un valor de diseño, que de forma predeterminada se establecerá en `layout=fixed` porque establecemos el ancho y el alto. Para obtener más información, consulte ["¿Qué sucede si no se especifica el atributo de diseño?"](control_layout.md#what-if-the-layout-attribute-isnt-specified) . [/tip]

[example preview="top-frame" playground="true"]
```html
<amp-img alt="grey cat"
    media="(min-width: 670px)"
    width="650"
    height="340"
    src="{{server_for_email}}/static/inline-examples/images/cat-large.jpg"></amp-img>
  <amp-img alt="grey cat"
    media="(min-width: 470px) and (max-width: 669px)"
    width="450"
    height="340"
    src="{{server_for_email}}/static/inline-examples/images/cat-medium.jpg"></amp-img>
  <amp-img alt="grey cat"
    media="(max-width: 469px)"
    width="226"
    height="340"
    src="{{server_for_email}}/static/inline-examples/images/cat-small.jpg"></amp-img>
```
[/example]

[tip type="read-on"] SIGUE **LEER:** Para obtener más información sobre la dirección de arte en AMP, consulte la guía [Dirección de arte con srcset, tamaños y alturas](art_direction.md) . [/tip]

#### Proporcionar imágenes optimizadas<a name="providing-optimized-images"></a>

La entrega de páginas de carga rápida requiere imágenes optimizadas, en tamaño, calidad y formato. Reduzca siempre el tamaño de los archivos al nivel de calidad más bajo aceptable. Hay varias herramientas que puede utilizar para "procesar" imágenes (por ejemplo, [ImageAlph](http://pngmini.com/lossypng.html) o [TinyPNG](https://tinypng.com/) ). En términos de formatos de imagen, algunos formatos de imagen proporcionan mejores capacidades de compresión que otros (por ejemplo, WebP y JPEG XR frente a JPEG). Querrá proporcionar la imagen más optimizada para su usuario, además de asegurarse de que la imagen sea compatible con el navegador del usuario (es decir, [no todos los navegadores admiten todos los formatos de imagen](https://en.wikipedia.org/wiki/Comparison_of_web_browsers#Image_format_support) ).

En HTML, puede servir diferentes formatos de imagen utilizando la etiqueta de `picture` . En AMP, aunque la etiqueta de `picture` no es compatible, puede publicar diferentes imágenes mediante el atributo de `fallback` .

[tip type="read-on"] SIGUE **LEER:** Para obtener más información sobre los recursos alternativos, consulte la guía [Marcadores de posición y](placeholders.md) recursos alternativos. [/tip]

En AMP, hay dos formas de lograr la publicación de imágenes optimizadas:

- Los desarrolladores que utilizan formatos de imagen que no son ampliamente compatibles, como WebP, pueden configurar su servidor para procesar los encabezados de `Accept` navegador y responder con bytes de imagen y el [encabezado de `Content-Type`](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/client-hints/) adecuado. Esto evita que el navegador descargue tipos de imágenes que no admite. Leer más sobre[negociación de contenido](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation) .

[sourcecode:html]
Accept: image/webp,image/apng,image/*,*/*;q=0.8
[/sourcecode]

- Proporcione alternativas de imagen anidadas, como el ejemplo siguiente.

##### Ejemplo: servir diferentes formatos de imagen

En el siguiente ejemplo, si el navegador es compatible con WebP, sirva montañas.webp, de lo contrario sirva montañas.jpg.

[example preview="top-frame" playground="true"]
```html
<amp-img alt="Mountains"
  width="550"
  height="368"
  layout="responsive"
  src="{{server_for_email}}/static/inline-examples/images/mountains.webp">
  <amp-img alt="Mountains"
    fallback
    width="550"
    height="368"
    layout="responsive"
    src="{{server_for_email}}/static/inline-examples/images/mountains.jpg"></amp-img>
</amp-img>
```
[/example]

Como una buena ventaja, algunos cachés, como Google AMP Cache, comprimen y convierten automáticamente imágenes a WebP y las resoluciones correctas si no lo hace. Sin embargo, no todas las plataformas usan cachés, por lo que aún debe optimizar las imágenes manualmente.

[tip type="read-on"] SIGUE **LEER:** Para obtener más información sobre las optimizaciones de imagen que aplica la caché de AMP de Google, consulte la publicación de blog ["Google AMP Cache, AMP Lite y la necesidad de velocidad"](https://developers.googleblog.com/2017/01/google-amp-cache-amp-lite-and-need-for.html) . [/tip]

## Ejemplos para inspirarte

A continuación, se muestran algunos ejemplos que esperamos lo inspiren a crear páginas AMP receptivas:

#### Producción

- [Getty Images "2016 en foco"](http://www.gettyimages.com/2016/)
- [Guía de regalos navideños de BRIT + CO](http://www.brit.co/the-coolest-tech-gadget-holiday-gift-guide/amp/)
- [El guardián](https://amp.theguardian.com/travel/2017/feb/26/trekking-holidays-in-patagonia)

#### Hecho por AMP

- [Ejemplos](../../../../documentation/examples/index.html)
- [Plantillas](../../../../documentation/templates/index.html)
- [Codelab del taller AMP Conf: creación de AMP bonitos](https://codelabs.developers.google.com/codelabs/amp-beautiful-interactive-canonical)
