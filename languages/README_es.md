# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | [Deutsch](README_de.md) | Español | [Français](README_fr.md)

Posición de lectura y escenas de página para Chrome y Edge. Cada página larga recuerda dónde lo dejaste; las páginas cortas con estado — un panel, un formulario a medias, un ticket empezado — se pueden guardar como *escena* y recuperar enteras.

Todo se calcula y se guarda en tu dispositivo. Sin cuenta, sin servicio de sincronización, sin telemetría. La única petición de red es la validación de la licencia.

## Gratis

- **Posición por página** — se escribe 600 ms después de dejar de desplazarte, y otra vez al ocultar o cerrar la pestaña. Nunca escrituras por píxel.
- **Botón de continuación** — al reabrir la página aparece abajo a la derecha *Sigue donde lo dejaste · 62%*. Un clic desplaza suavemente hasta allí. **Nunca mueve la página por su cuenta.**
- **Discreto** — el botón se desvanece a los pocos segundos; mover el puntero a la esquina inferior derecha lo recupera. No dibuja nada más.
- **Las páginas cortas callan** — una posición por debajo del umbral (600 px por defecto) no se ofrece.
- **Pausar este sitio / Borrar esta página** — a un clic desde el popup.
- **20 páginas de historial** — con el porcentaje alcanzado; clic en una fila para reabrirla.
- **Rutas SPA separadas** — `/app/inbox` y `/app/settings` son páginas distintas; los routers con hash (`#/route`) también.
- **Carga diferida contemplada** — si la posición guardada supera la altura actual del documento, se reintenta dos veces y luego se abandona en silencio.
- **Tus datos son portables** — exportar e importar JSON, gratis.
- **Preguntar antes de restaurar** — siempre se muestra qué se va a sobrescribir. Activado por defecto y gratis: una confirmación no es un extra de pago.

## Pro (9,90 USD, pago único)

| | Gratis | Pro |
|---|---|---|
| Posiciones de lectura | ✅ | ✅ |
| Páginas de historial | 20 | 500 |
| Botón de continuación | tiempos fijos | retardo / desvanecido / umbral ajustables |
| Excepciones | dominios | dominios **y** expresiones regulares |
| Escenas (guardar + restaurar) | — | ✅ |
| Campos de texto, textareas, selects, casillas, radios, paneles abiertos | — | ✅ |
| Varias escenas con nombre por página | — | ✅ |
| Escenas por página | — | configurable (1–20) |
| Incluir campos de solo lectura | — | opcional |
| Atajo de teclado para guardar | — | opcional (desactivado por defecto) |
| Exportar / importar JSON | ✅ posiciones | ✅ posiciones + escenas |

Una licencia funciona en dos navegadores. Quitarla libera la plaza. La validación se limita a una vez cada dos horas, funciona sin conexión con la licencia en caché y un fallo nunca te deja fuera.

## Qué captura una escena

Valores de `input` (text, email, url, tel, number, search, date…), `textarea`, la opción elegida de `select`, el estado de `checkbox`/`radio`, los `details` abiertos y la posición de desplazamiento. Se escribe mediante el setter nativo y se disparan `input` y `change`, así que React y Vue lo notan. Los campos se localizan por ruta estructural, luego por `id`, luego por `name`.

### Lo que deliberadamente no toca

- **Contraseñas, archivos, campos ocultos, códigos de un solo uso, números de tarjeta, CVV, IBAN, SSN** — excluidos al capturar, no filtrados al restaurar. No hay ajuste para activarlos.
- **Campos de solo lectura y deshabilitados** — omitidos por defecto (Pro: solo lectura opcional).
- **Paneles `role="tab"`** — estaba en el plan original y se eliminó: el estado de pestañas lo gestiona el framework, y escribir `aria-selected` no hace nada o provoca navegación. Una función que parece funcionar es peor que ninguna.
- **Editores `contenteditable`** — no hay forma fiable y segura con frameworks de devolver el texto; se omite.
- **Campos en shadow roots e iframes de otro origen** — no se puede construir una ruta estable; se omiten en lugar de adivinar.

## Privacidad

Posiciones, escenas y ajustes viven solo en `chrome.storage.local`. Sin estadísticas, sin telemetría, sin cuentas. Los valores capturados no salen del navegador; la única conexión es la validación de licencia contra `api.annmax1983.com`.

## Permisos

| Permiso | Motivo |
|---|---|
| `storage` | Posiciones, escenas, ajustes y estado de licencia — todo local |
| `http://*/*`, `https://*/*` | El script de contenido debe existir en cada página **antes** de que te desplaces, o no habrá nada que recordar. A diferencia de un marcador, aquí no se puede pedir permiso sitio a sitio más tarde: de ahí el acceso amplio, indicado con claridad en la ficha de la tienda. |

Sin `tabs`, `scripting`, `debugger` ni `activeTab`. Nada se ejecuta en páginas `chrome://` (Chrome), `edge://` (Edge), `file://` ni páginas de otras extensiones.

## Desarrollo

```bash
npm install
npm test            # node --test (99 aserciones)
npm run build       # minificar + zip → publish/readpos-v{version}.zip
npm run locales     # regenerar los seis idiomas
npm run icons       # regenerar iconos
npm run shoot       # capturas para la tienda (requiere Chrome)
```
