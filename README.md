# Cartuchos

Dos páginas estáticas para inventariar cartuchos de tinta por su número de serie.

- **`index.html`** — estación de escaneo. Registra lotes, descarta duplicados y lecturas que no son un número de serie, muestra la hoja en vivo y exporta a CSV.
- **`fecha.html`** — consulta suelta de un serial, sin registrar nada.

Sin dependencias, sin CDN y sin backend.

## Uso

Tres formas de introducir un serial: a mano, con un lector USB de códigos de barras, o con la cámara.

La estación valida cada lectura y la rechaza si no tiene formato de número de serie, con aviso visual y sonoro. Los duplicados se detectan dentro del lote en curso y también contra los lotes ya cerrados.

El campo **Lote** etiqueta la tanda y acompaña a cada fila. Exportar descarga el CSV sin vaciar nada; para pasar a la tanda siguiente está **Cerrar lote y vaciar hoja**, que conserva los seriales en memoria para seguir detectando repetidos.

El CSV sale con separador `;` y BOM UTF-8, que es lo que abre Excel en columnas sin pasar por el asistente de importación.

## Lector de códigos de barras

Pensado para un lector USB, que funciona como un teclado y no necesita drivers.

- Conviene configurar el **sufijo Enter (CR)** en el lector. Casi todos lo traen de fábrica.
- Si no lo manda, la página lo detecta igual: mide la velocidad de entrada y, si llega en ráfaga, valida sola tras una pausa breve.
- El foco vuelve al campo de entrada al hacer clic en cualquier parte.

## Cámara

Usa el detector de códigos del propio navegador.

- Requiere contexto seguro: HTTPS o `localhost`. Con `file://` el navegador la bloquea.
- Va en Chrome y Edge de escritorio y en Chrome para Android. Safari e iOS no llevan la API y el botón no aparece.

Para tandas largas, el lector USB es bastante más rápido.

## Datos

Todo vive en el navegador (`localStorage`). No se envía nada a ningún servidor. Exportar cada cierto tiempo es la única copia de seguridad: borrar los datos de navegación vacía el registro.

Los CSV exportados no se versionan, ver `.gitignore`.

## Desarrollo local

```bash
python3 -m http.server 8000
```

Y abrir `http://localhost:8000`.
