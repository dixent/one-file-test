---
$title: Agregar carruseles
$order: '3'
description: Otra característica común en las páginas móviles es un carrusel. Puede
  agregar carruseles fácilmente a las páginas AMP utilizando el componente amp-carrusel.
---

Otra característica común en las páginas móviles es un carrusel. Puede agregar carruseles fácilmente a las páginas AMP utilizando el componente [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) . Comencemos con un ejemplo simple, como un carrusel de imágenes.

## Carrusel de imagen simple

Recuerde incluir la biblioteca de componentes [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) **agregando** la siguiente solicitud de JavaScript a la etiqueta `<head>` de su documento:

```html
<script async custom-element="amp-carousel" src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"></script>
```

A continuación, incrustemos un carrusel simple de imágenes con un diseño receptivo y un ancho y alto predefinidos. **Agregue** lo siguiente a su página:

```html
<amp-carousel layout="fixed-height" height="168" type="carousel" >
  <amp-img src="mountains-1.jpg" width="300" height="168"></amp-img>
  <amp-img src="mountains-2.jpg" width="300" height="168"></amp-img>
  <amp-img src="mountains-3.jpg" width="300" height="168"></amp-img>
</amp-carousel>
```

**Actualice** su página y debería ver un carrusel:

{{ image('/static/img/docs/tutorials/tut-advanced-carousel-simple.png', 412, 403, align='center half', caption='Simple images carousel') }}

El componente [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) se puede configurar de varias maneras. Cambiemos la interfaz de usuario para mostrar solo una imagen a la vez y hacer que el diseño del carrusel responda.

Para hacer esto, primero **cambie** el `type` de [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) de `carousel` a `slides` , **cambie** el `layout` a `responsive` y **configure** el `width` en 300 (asegurándose de que tenga definidos tanto la `height` como el `width` ). **Agregue** el atributo `"layout=responsive"` a los hijos [`amp-img`](../../../../documentation/components/reference/amp-img.md) del [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) .

**Recarga** tu página. Ahora, en lugar de una lista desplazable de elementos, verá un elemento a la vez. Intenta **deslizar** horizontalmente para moverte por los elementos. Si desliza el dedo hacia el tercer elemento, no podrá deslizar más.

A continuación, **agregue** el atributo de `loop` . **Actualice** la página e intente deslizar hacia la izquierda inmediatamente. El carrusel gira sin parar.

Por último, hagamos que este carrusel se reproduzca automáticamente a una velocidad de cada 2 segundos. **Agregue** el atributo de `autoplay` y el atributo de `delay` con un valor de `2000` (por ejemplo, `delay="2000"` ) al [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) .

Su resultado final debería verse más o menos así:

```html
<amp-carousel layout="responsive" width="300" height="168" type="slides" autoplay delay="2000" loop>
  <amp-img src="mountains-1.jpg" width="300" height="168" layout="responsive"></amp-img>
  <amp-img src="mountains-2.jpg" width="300" height="168" layout="responsive"></amp-img>
  <amp-img src="mountains-3.jpg" width="300" height="168" layout="responsive"></amp-img>
</amp-carousel>
```

**¡Actualiza** la página y dale una vuelta!

[tip type = "note"]
**NOTA:** es posible que haya notado que cuando el [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) tenía el tipo de `carousel` , usamos el `fixed-height` diseño de `fixed-height` . Los tipos de diseño admitidos para el tipo de `carousel` son limitados; por ejemplo, el tipo de `carousel` no admite el diseño `responsive` . Como su nombre lo indica, los elementos de altura fija toman el espacio disponible para ellos, pero mantienen la altura sin cambios. Para los elementos de altura fija, debe definir el atributo de `height` , mientras que el atributo de `width` debe ser `auto` o no establecido.
[/propina]

## Contenido de carrusel mixto

Los carruseles de imagen son geniales, pero ¿qué pasa si queremos que aparezca contenido más complejo en nuestro carrusel? Intentemos mezclar un poco las cosas colocando un anuncio, texto y una imagen en un solo carrusel. ¿Puede [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) manejar realmente tal mezcla de una vez? ¡Absolutamente!

Primero, **agreguemos** este estilo a su `<style amp-custom>` para garantizar que los componentes [`amp-fit-text`](../../../../documentation/components/reference/amp-fit-text.md) y [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) funcionen juntos de manera segura:

```css
amp-fit-text {
    white-space: normal;
}
```

Ahora, **reemplace** su carrusel simple con esto:

```html
<amp-carousel layout="fixed-height" height="250" type="carousel" >
    <amp-img src="blocky-mountains-1.jpg" width="300" height="250"></amp-img>

    <amp-ad width="300" height="250"
      type="doubleclick"
      data-slot="/35096353/amptesting/image/static">
        <div placeholder>This ad is still loading.</div>
    </amp-ad>

    <amp-fit-text width="300" height="250" layout="fixed">
        Big, bold article quote goes here.
    </amp-fit-text>
</amp-carousel>
```

**Actualice** la página y debería ver algo como esto:

{{ image('/static/img/docs/tutorials/tut-advanced-carousel-complex.gif', 412, 403, align='center half', caption='A carousel of mixed content') }}

Para obtener más información, consulte la documentación de referencia del componente [`amp-carousel`](../../../../documentation/components/reference/amp-carousel.md) .

[tip type = "note"]
**NOTA:** en nuestro último ejemplo, es posible que haya notado que el componente [`amp-ad`](../../../../documentation/components/reference/amp-ad.md) incluye un elemento `div` secundario con el atributo de `placeholder` . Anteriormente en el tutorial, encontramos un escenario similar con [`amp-ad`](../../../../documentation/components/reference/amp-ad.md) usando una `fallback` . ¿Cuál es la diferencia entre marcador de posición y reserva? `Fallback` elementos de `Fallback` aparecen cuando el elemento padre no se carga, es decir, si no había ningún anuncio disponible. `placeholder` elementos de `placeholder` aparecen en lugar del elemento principal, mientras se está cargando. En cierto sentido, estos elementos refuerzan el proceso de carga del elemento padre. Puede obtener más información en la guía [Marcadores de posición y retrocesos](../../../../documentation/guides-and-tutorials/develop/style_and_layout/placeholders.md) .
[/propina]
