# Cartuchos Memjet

Dos páginas estáticas para trabajar con los números de serie de los cartuchos de tinta Memjet.

- **`index.html`** — estación de escaneo en serie. Registra cientos o miles de cartuchos, rechaza duplicados y P/N, y exporta a Excel.
- **`fecha.html`** — consulta suelta. Un serial, su fecha de fabricación y el desglose de qué significa cada tramo.

Sin dependencias, sin CDN y sin backend. Todo va embebido en cada archivo.

## La regla del número de serie

Ejemplo real: serial `A2509714161002`, etiqueta `Manufacture Date: Apr.07.2025`.

| Posición | Ejemplo | Significado |
|---|---|---|
| 1 | `A` | Prefijo. Sin identificar, no afecta a la fecha. |
| 2-3 | `25` | Año, interpretado como `2000 + YY` → 2025. |
| 4-6 | `097` | Día del año, contando desde el 1 de enero → 7 de abril. |
| 7 en adelante | `14161002` | Sin identificar. Se ignoran. |

El día se valida contra el año: 365 días en año normal, 366 en bisiesto. Un día fuera de rango se avisa como error en vez de devolver una fecha inventada.

## Cómo se distingue el SN del P/N

La etiqueta lleva dos códigos de barras, el P/N arriba y el SN abajo. Se diferencian sin ambigüedad:

- El **P/N** es solo dígitos (`10009110`).
- El **SN** empieza por letra seguida de cinco dígitos (`A25097...`).

Cualquier lectura puramente numérica se rechaza. Aunque el lector apunte al código equivocado, no entra en la lista.

## Estación de escaneo

- **Verde**: aceptado, pitido agudo corto.
- **Ámbar**: repetido, doble pitido, indica cuándo se escaneó la primera vez. No se añade.
- **Rojo**: P/N o lectura incompleta, pitido grave. No se añade.

Guarda en `localStorage` después de cada escaneo, agrupando escrituras cada 400 ms para que el lector no se frene con miles de filas. Al cerrar la pestaña vuelca lo pendiente y pide confirmación.

La exportación genera un CSV con separador `;` y BOM UTF-8, que es lo que abre el Excel español en columnas y con los acentos correctos sin pasar por el asistente de importación. Columnas: número de serie, fecha de fabricación, día del año, año y momento del escaneo.

## Lector de códigos de barras

Pensado para un lector USB de pistola, que funciona como un teclado y no necesita drivers.

- Configurar el **sufijo Enter (CR)** en el lector. Casi todos lo traen de fábrica; si no, se activa escaneando el código correspondiente del manual. Con Enter, cada lectura se valida al instante.
- Si el lector no manda Enter, la página lo detecta igual: mide la velocidad de entrada y, si llega en ráfaga, valida sola tras 140 ms de pausa. El tecleo manual sigue esperando a Enter.
- El foco vuelve al campo de entrada al hacer clic en cualquier parte, así que el lector nunca escribe en el vacío.

## Cámara

Alternativa para móvil, usando el detector de códigos del propio navegador. En la estación de escaneo la cámara queda abierta y va encadenando cartuchos.

- Requiere contexto seguro: HTTPS o `localhost`. Abriendo el archivo con doble clic (`file://`) el navegador bloquea la cámara.
- Funciona en Chrome y Edge de escritorio y en Chrome para Android. Safari e iOS no llevan la API; ahí el botón no aparece.

Para 3.000 unidades el lector USB es bastante más rápido que el móvil.

## Desarrollo local

```bash
python3 -m http.server 8000
```

Y abrir `http://localhost:8000`. `localhost` cuenta como contexto seguro aunque no sea HTTPS.

## Publicación

Pensado para GitHub Pages sirviendo la raíz de `main`. `index.html` queda como portada.

## Pendiente

Los dígitos a partir de la posición 7 no están identificados. En el único ejemplo disponible son `14161002`, que podrían ser hora de llenado, línea de producción y contador de unidad. Para confirmarlo hacen falta varios cartuchos con fechas de etiqueta distintas: al escanear el lote de 3.000 habrá material de sobra para deducirlo.

---

Systeems · uso interno
