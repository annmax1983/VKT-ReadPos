# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | [Deutsch](README_de.md) | Español | [Français](README_fr.md)

Posición de lectura y escenas de página para Chrome, Edge y Firefox. Cada página larga recuerda dónde lo dejaste; las páginas con estado — un panel, un formulario a medias, un ticket empezado — se pueden guardar como *escena* y recuperar enteras.

Instalación: https://annmax1983.com/extensions/readpos

## Funciones

### Posiciones de lectura (gratis)

- **Por página, automático** — la posición se guarda 600 ms después de que dejes de desplazarte, y otra vez cuando la pestaña se oculta o se cierra.
- **La píldora de continuación** — al reabrir una página aparece abajo a la derecha *Continuar donde lo dejaste · 62 %*. Un clic desplaza suavemente hasta allí. Nunca se desplaza sola.
- **Discreta** — la píldora se desvanece a los pocos segundos; si llevas el puntero a la esquina inferior derecha, vuelve. No se dibuja nada más en la página.
- **Las páginas cortas siguen calladas** — no se ofrecen posiciones por debajo del umbral (600 px por defecto).
- **Pausar un sitio, borrar una página** — un clic cada uno, en el panel.
- **20 entradas de historial** — el panel las lista con el porcentaje alcanzado; un clic en una fila reabre la página.
- **Rutas SPA separadas** — `/app/inbox` y `/app/settings` son páginas distintas. Las rutas con hash (`#/route`) también.
- **Páginas de carga diferida** — si la posición guardada supera la altura actual del documento, se reintenta dos veces y luego se abandona en silencio.
- **Portátil** — exportación e importación JSON, gratis.
- **Confirmación antes de restaurar** — antes de devolver una escena se lista qué se va a sobrescribir. Gratis y activa por defecto.

### Pro

- **Escenas de página** — guarda la posición más cada campo del formulario bajo un nombre que elijas, y devuélvelos todos después.
- **500 entradas de historial** en lugar de 20.
- **Tiempos de la píldora** — retardo, desvanecido y umbral ajustables.
- **Lista de exclusión** — dominios y expresiones regulares.
- **Varias escenas con nombre por página** — de 1 a 20.
- **Campos de solo lectura** incluidos opcionalmente en una escena.
- **Atajo de teclado** para guardar una escena.
- **Escenas en la exportación** — el archivo JSON lleva posiciones y escenas.

| | Gratis | Pro |
|---|---|---|
| Posiciones de lectura | ✅ | ✅ |
| Entradas de historial | 20 | 500 |
| Píldora de continuación | tiempos fijos | retardo / desvanecido / umbral ajustables |
| Lista de exclusión | dominios | dominios **y** expresiones regulares |
| Escenas de página (guardar + restaurar) | — | ✅ |
| Campos de texto, áreas de texto, selects, casillas, radios, paneles abiertos | — | ✅ |
| Varias escenas con nombre por página | — | ✅ |
| Límite de escenas por página | — | configurable (1–20) |
| Incluir campos de solo lectura | — | opcional |
| Guardar con atajo | — | opcional (desactivado por defecto) |
| Exportación / importación JSON | posiciones | posiciones + escenas |

Una licencia se activa en dos navegadores. Al quitarla se libera la plaza.

## Qué guarda una escena

| Elemento | Se restaura |
|---|---|
| `input` de texto (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | el valor |
| `textarea` | el valor |
| `select` | la opción elegida (si no hay valor, se busca por la etiqueta) |
| `checkbox`, `radio` | el estado de marcado |
| `details` | abierto / cerrado |
| posición de desplazamiento | sí, con la misma lógica de reintentos que la píldora |

Los valores vuelven por el setter nativo y van seguidos de eventos `input` y `change`, así que React y Vue lo notan. Los campos se localizan primero por ruta estructural, luego por `id` y luego por `name`, así que una página que se recoloca un poco se restaura igual.

### Lo que no se guarda

- **Contraseñas, archivos, campos ocultos, códigos de un solo uso, números de tarjeta, CVV, IBAN, SSN** — se excluyen al capturar. Ningún ajuste los reactiva.
- **Campos de solo lectura y desactivados** — salvo que actives la inclusión de solo lectura (Pro).
- **Paneles `role="tab"` y editores `contenteditable`** — no hay forma segura de devolver el estado sin romper el framework, así que se dejan fuera en lugar de a medias.
- **Campos dentro de shadow roots e iframes de otro origen** — no se puede construir una ruta estable a través de esos límites.

## Uso

### Leer

1. Desplázate por cualquier página. Al parar, la posición queda guardada.
2. Vuelve a esa página. Si sigues cerca del principio, la píldora aparece al cabo de un momento abajo a la derecha.
3. Púlsala para desplazarte hasta tu punto, o ignórala y se desvanece.
4. El resto está en el panel: **Seguir leyendo**, **Pausar en este sitio**, **Borrar esta página** y la lista de historial.

### Guardar una escena (Pro)

1. Deja la página en el estado que quieres conservar.
2. Abre el panel, pulsa **Guardar escena**, ponle nombre y guarda. O usa el atajo si lo activaste.
3. Después, en la misma página, el panel lista todas las escenas guardadas allí.
4. Pulsa **Restaurar**. Un panel muestra qué se va a sobrescribir; confirma y vuelven todos los campos y la posición.

La escena sigue siendo editable: renómbrala o bórrala desde su fila cuando quieras.

### Gestionar los datos

- **Fila del historial** — un clic reabre esa página; el icono de papelera quita solo ese registro.
- **Sección de datos** — muestra cuántas posiciones y escenas hay guardadas y cuánto ocupan.
- **Borrar todas las posiciones** / **Eliminar todas las escenas** — ambos piden confirmación.
- **Exportar** escribe posiciones (y escenas con Pro) en un archivo JSON. **Importar** combina en lugar de reemplazar y conserva el registro más reciente cuando ambos lados conocen la misma página.

### Ajustes

- **Gratis** — pausar un sitio, borrar una página, exportar e importar.
- **Pro** — retardo, desvanecido y umbral de la píldora; dominios y expresiones regulares excluidas; escenas por página; campos de solo lectura; atajo de teclado.

En Firefox el atajo opcional se define en el Gestor de complementos: **about:addons → engranaje → Gestionar atajos de la extensión**.

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
