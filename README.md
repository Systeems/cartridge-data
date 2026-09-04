# Fecha del cartucho

Calcula la fecha de fabricación de un cartucho de tinta Memjet a partir de su número de serie.

Es una página estática de un solo archivo: HTML, CSS y JavaScript embebidos, sin dependencias, sin CDN y sin conexión a internet. Se puede abrir con doble clic o servir desde cualquier sitio.

## La regla

Ejemplo real: serial `A2509714161002`, etiqueta `Manufacture Date: Apr.07.2025`.

| Posición | Ejemplo | Significado |
|---|---|---|
| 1 | `A` | Prefijo. Sin identificar, no afecta a la fecha. |
| 2-3 | `25` | Año, interpretado como `2000 + YY` → 2025. |
| 4-6 | `097` | Día del año, contando desde el 1 de enero → 7 de abril. |
| 7 en adelante | `14161002` | Sin identificar. Se ignoran. |

Con los seis primeros caracteres es suficiente. Si se pega el serial entero, el resto se descarta.

El día se valida contra el año: en un año no bisiesto el máximo es 365 y en uno bisiesto 366. Un día fuera de rango se avisa como error en vez de devolver una fecha inventada.

## Uso

Tres formas de introducir el serial:

- **A mano.** Se escribe en el campo y el resultado se recalcula al teclear.
- **Lector USB de códigos de barras.** Estos lectores actúan como teclado. En escritorio el cursor ya está puesto en el campo al cargar la página, y al enfocarlo se selecciona todo el texto, así que se pueden encadenar cartuchos sin borrar el anterior.
- **Cámara.** Botón «Escanear con la cámara», usando el detector de códigos que trae el propio navegador. Lee Code 128 (el de la etiqueta), Code 39, ITF y QR.

El escaneo con cámara tiene dos límites:

- Requiere un contexto seguro. Abriendo el archivo con doble clic (`file://`) el navegador bloquea la cámara. Hay que servirlo por HTTPS o desde `localhost`.
- Funciona en Chrome y Edge de escritorio y en Chrome para Android. Safari e iOS no llevan la API; ahí el botón no aparece y queda la entrada manual.

## Desarrollo local

Para probar la cámara sin publicar nada:

```bash
python3 -m http.server 8000
```

Y abrir `http://localhost:8000`. `localhost` cuenta como contexto seguro aunque no sea HTTPS.

## Publicación

El repo está pensado para GitHub Pages sirviendo la raíz de `main`. El archivo se llama `index.html` para que la URL quede limpia.

## Pendiente

Los dígitos a partir de la posición 7 no están identificados. En el único ejemplo disponible son `14161002`, que podrían ser hora de llenado, línea de producción y contador de unidad, pero hace falta comparar varios cartuchos con fechas de etiqueta distintas para confirmarlo.

---

Systeems · uso interno
