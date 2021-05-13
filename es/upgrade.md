TensorFlow 2.0 incluye muchos cambios en la API, como reordenar argumentos, cambiar el nombre de los símbolos y cambiar los valores predeterminados de los parámetros. Realizar manualmente todas estas modificaciones sería tedioso y propenso a errores. Para agilizar los cambios y hacer que su transición a TF 2.0 sea lo más fluida posible, el equipo de TensorFlow ha creado la `tf_upgrade_v2` para ayudar en la transición del código heredado a la nueva API

Nota: `tf_upgrade_v2` se instala automáticamente para TensorFlow 1.13 y versiones posteriores (incluidas todas las compilaciones de TF 2.0).

El uso típico es así:

<pre class="devsite-terminal devsite-click-to-copy prettyprint lang-bsh">tf_upgrade_v2
--intree my_project
--outtree my_project_v2
--reportfile report.txt
</pre>

Acelerará su proceso de actualización al convertir los scripts Python de TensorFlow 1.x existentes a TensorFlow 2.0.

# ¡HOLA! ASDASDSAD!

dsfsdfdsf h

# ¡Hola2! ASDASDSAD2!

dsfsdfdsf2 h2

El script de conversión se automatiza tanto como sea posible, pero todavía hay cambios sintácticos y estilísticos que el script no puede realizar.
