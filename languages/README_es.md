# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | [Deutsch](README_de.md) | Español | [Français](README_fr.md)

Posición de lectura y puntos de restauración de página para Chrome, Edge y Firefox. Cada página larga recuerda dónde lo dejaste; las páginas con estado — un panel, un formulario a medias, un ticket empezado — se pueden guardar como *punto de restauración* y recuperar enteras.

Instalación: https://annmax1983.com/extensions/readpos

## Funciones

### Posiciones de lectura (gratis)

- **Por página, automático** — la posición se guarda 600 ms después de que dejes de desplazarte, y otra vez cuando la pestaña se oculta o se cierra.
- **Continuar, automático** — la misma página se abre directamente en tu posición. Cualquier otra muestra la píldora *Continuar donde lo dejaste · 62 %*, a un clic. El salto automático se puede desactivar en Ajustes.
- **Discreta** — la píldora se desvanece a los pocos segundos; si llevas el puntero a la esquina inferior derecha, vuelve. No se dibuja nada más en la página.
- **Las páginas cortas siguen calladas** — no se ofrecen posiciones por debajo del umbral (600 px por defecto).
- **Pausar un sitio, borrar una página** — un clic cada uno, en el panel.
- **Grabar solo los sitios que elijas** — añade dominios a mano o con un clic en «Añadir sitio actual»; el resto se omite. No hay interruptor para grabar todo: una lista vacía no graba nada. Gratis.
- **Excluir una página, o un patrón** — «Excluir esta página» en el panel es un clic: esa dirección no se vuelve a grabar nunca, y el resto del sitio sigue igual. Las reglas de patrón (expresiones regulares) quitan secciones enteras en todos los sitios sin nombrarlas una a una. Ambas listas son gratis y viven en la pestaña **Excluidas** de la lista completa.
- **20 entradas de historial** — el panel lista las páginas del sitio en el que estás, con el porcentaje alcanzado; un clic en una fila reabre la página.
- **Rutas SPA separadas** — `/app/inbox` y `/app/settings` son páginas distintas. Las rutas con hash (`#/route`) también.
- **Páginas de carga diferida** — si la posición guardada supera la altura actual del documento, se reintenta dos veces y luego se abandona en silencio.
- **Portátil** — exportación e importación JSON, gratis.
- **Confirmación antes de restaurar** — antes de devolver un punto de restauración se lista qué se va a sobrescribir. Gratis y activa por defecto.

### Pro

- **Escenas de página** — guarda la posición más cada campo del formulario bajo un nombre que elijas, y devuélvelos todos después.
- **500 entradas de historial** en lugar de 20.
- **Tiempos de la píldora** — retardo, desvanecido y umbral ajustables.
- **Varias puntos de restauración con nombre por página** — de 1 a 20.
- **Campos de solo lectura** incluidos opcionalmente en un punto de restauración.
- **Atajo de teclado** para guardar un punto de restauración.
- **Las puntos de restauración viajan con la exportación** — el archivo JSON lleva posiciones y puntos de restauración; volver a importar puntos de restauración requiere Pro.

| | Gratis | Pro |
|---|---|---|
| Posiciones de lectura | ✅ | ✅ |
| Entradas de historial | 20 | 500 |
| Píldora de continuación | tiempos fijos | retardo / desvanecido / umbral ajustables |
| Escenas de página (guardar + restaurar) | — | ✅ |
| Campos de texto, áreas de texto, selects, casillas, radios, paneles abiertos | — | ✅ |
| Varias puntos de restauración con nombre por página | — | ✅ |
| Puntos de restauración por página | — | configurable (1–20) |
| Incluir campos de solo lectura | — | opcional |
| Guardar con atajo | — | opcional (desactivado por defecto) |
| Exportación / importación JSON | posiciones | posiciones + puntos de restauración |

Una licencia se activa en dos navegadores. Al quitarla se libera la plaza.

## Qué guarda un punto de restauración

| Elemento | Se restaura |
|---|---|
| `input` de texto (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | el valor |
| `textarea` | el valor |
| `select` | la opción elegida (si no hay valor, se busca por la etiqueta) |
| `select multiple` | todas las opciones elegidas |
| desplegables de componente (Element Plus, Ant Design, Arco, Naive, Semi, iView) | la etiqueta elegida |
| editores `contenteditable` | el texto |
| campos dentro de shadow roots abiertos (Web Components) | valor |
| `checkbox`, `radio` | el estado de marcado |
| `details` | abierto / cerrado |
| posición de desplazamiento | sí, con la misma lógica de reintentos que la píldora |

Los valores vuelven por el setter nativo y van seguidos de eventos `input` y `change`, así que React y Vue lo notan; si un framework lo ignora, el texto se vuelve a escribir con `insertText`. Los campos se localizan primero por un selector CSS corto (llega dentro de shadow roots abiertos), luego por `id`, luego por `name` más el valor actual y, por último, por ruta estructural. Antes de escribir se compara la etiqueta con la guardada, así que un formulario reordenado no recibe valores cruzados. Si un campo se resiste incluso a eso, el texto se escribe con el depurador del navegador: conectar, insertar una vez, desconectar; esos componentes solo aceptan entrada real.

### Lo que no se guarda

- **Contraseñas, archivos, campos ocultos, códigos de un solo uso, números de tarjeta, CVV, IBAN, SSN** — se excluyen al capturar. Ningún ajuste los reactiva.
- **Campos de solo lectura y desactivados** — salvo que actives la inclusión de solo lectura (Pro).
- **Paneles `role="tab"`** — cambiar de pestaña dispara la lógica de carga del framework, así que no hay forma segura de escribirlo.
- **Campos dentro de shadow roots cerrados e iframes de otro origen** — no se llega a esos nodos, así que no hay ruta posible.

## Uso

### Leer

1. Desplázate por cualquier página. Al parar, la posición queda guardada.
2. Vuelve a esa página. La misma página vuelve sola a tu posición; si no es la misma, aparece la píldora abajo a la derecha.
3. Con la píldora, pulsa para volver o ignórala y se desvanece. Si prefieres pulsar siempre, desactiva el salto automático en Ajustes.
4. El resto está en el panel: **Seguir leyendo**, **Pausar en este sitio**, **Excluir esta página**, **Borrar esta página** y la lista de historial.

### Guardar un punto de restauración (Pro)

1. Deja la página en el estado que quieres conservar.
2. Abre el panel, pulsa **Guardar punto de restauración**, ponle nombre y guarda. O usa el atajo si lo activaste.
3. Después, en la misma página, el panel lista todos los puntos de restauración guardados allí.
4. Pulsa **Restaurar**. Un panel muestra qué se va a sobrescribir; confirma y vuelven todos los campos y la posición.

El punto de restauración sigue siendo editable: renómbralo o bórralo desde su fila cuando quieras.

### Gestionar los datos

- **Fila del historial** — un clic reabre esa página; el icono de papelera quita solo ese registro.
- **Pestaña «Excluidas»** — está en la lista completa: pega una dirección o añade una regla de patrón, y cada entrada se puede quitar desde su fila. Una regla indica cuántas páginas guardadas atrapa ahora mismo.
- **Sección de datos** — muestra cuántas posiciones y puntos de restauración hay guardadas y cuánto ocupan.
- **Borrar todas las posiciones** / **Eliminar todos los puntos de restauración** — ambos piden confirmación.
- **Exportar** escribe posiciones (y puntos de restauración con Pro) en un archivo JSON. **Importar** combina en lugar de reemplazar y conserva el registro más reciente cuando ambos lados conocen la misma página.

### Ajustes

- **Gratis** — pausar un sitio, excluir páginas y patrones, borrar una página, exportar e importar.
- **Pro** — retardo, desvanecido y umbral de la píldora; puntos de restauración por página; campos de solo lectura; atajo de teclado.

En Firefox el atajo opcional se define en el Gestor de complementos: **about:addons → engranaje → Gestionar atajos de la extensión**.

## Permisos

- **`storage`** — posiciones, puntos de restauración, ajustes y estado de licencia. Solo en local.
- **Acceso a páginas http y https** — el script de contenido debe estar antes de que te desplaces; si no, no habría nada que recordar.
- **`debugger`** — para una sola cosa: escribir en un campo que descarta cualquier escritura por script. Conectar, insertar una vez, desconectar. Nunca lee la página ni se queda conectado.

## Navegadores

| Navegador | Versión mínima |
|---|---|
| Chrome | 116 |
| Edge | 116 |
| Firefox | 140 |

La versión de Firefox está en `codeup/VKT-ReadPos`. Una exportación JSON de una se importa sin cambios en la otra.

## Enlaces

- Sitio web: https://annmax1983.com/extensions/readpos
- Política de privacidad: [privacy-policy.html](../privacy-policy.html)
