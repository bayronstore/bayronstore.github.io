# Baystore Shoes Store — catálogo con Google Sheets y Google Drive

La tienda es una página estática. Ya no utiliza Supabase ni necesita un panel administrativo. La persona encargada administra el catálogo desde Google Sheets y las fotografías desde Google Drive. Los clientes consultan el catálogo y envían sus pedidos a WhatsApp.

## Archivos
- `index.html`: tienda pública.
- `styles.css`: diseño adaptable a celular y computador.
- `store.js`: lee el catálogo CSV de Google Sheets, agrupa las tallas y prepara pedidos por WhatsApp.
- `config.js`: enlace CSV de Google Sheets y WhatsApp.
- `logo-baystore.jpeg`: logo de Baystore.

## 1. Preparar Google Sheets
1. Sube `Plantilla_Catalogo_Baystore.xlsx` a Google Drive.
2. Haz clic derecho sobre el archivo y elige **Abrir con → Hojas de cálculo de Google**.
3. En la pestaña `Catalogo`, elimina las filas de ejemplo que dicen EJEMPLO y agrega los productos reales.
4. Conserva los encabezados y las columnas de la plantilla. Registra **una fila por cada talla**. Si un mismo modelo tiene tallas 38, 39 y 40, crea tres filas con el mismo nombre, marca, precio y foto, cambiando talla y cantidad.
5. En `Publicado`, selecciona `Sí` para mostrar el producto y `No` para ocultarlo.
6. La columna `Disponibilidad` se calcula a partir de la cantidad. No escribas sobre las fórmulas de esa columna.
7. Para que el sitio pueda leer el catálogo, ve a **Archivo → Compartir → Publicar en la web**. Selecciona únicamente la pestaña `Catalogo`, elige formato **Valores separados por comas (.csv)** y pulsa **Publicar**.
8. Copia el enlace publicado. Debe ser un enlace de Google Sheets que entregue el CSV.

> Publicar en la web hace que los datos de esa pestaña sean visibles para cualquiera que tenga el enlace o acceda al sitio. No pongas allí teléfonos privados, costos internos, datos de clientes ni información confidencial. Publica únicamente el catálogo que quieres mostrar a compradores.

## 2. Preparar Google Drive para las fotos
1. Crea una carpeta llamada `Fotos catalogo Baystore`.
2. Sube las fotografías de los productos.
3. Para cada foto, haz clic derecho → **Compartir** → cambia el acceso general a **Cualquier persona con el enlace – Lector**.
4. Copia el enlace de la foto y pégalo en la columna `Foto (enlace público)` de Google Sheets.
5. No borres ni muevas las fotos después de publicar el catálogo. El sitio convierte los enlaces de archivos de Drive para mostrarlos como imágenes.

## 3. Conectar la página con la hoja
1. Abre `config.js` con un editor de texto.
2. Busca `PEGA_AQUI_EL_ENLACE_CSV_DE_GOOGLE_SHEETS`.
3. Reemplaza únicamente ese texto por el enlace CSV que copiaste al publicar la pestaña `Catalogo`. Conserva las comillas.
4. Guarda el archivo.
5. El número de WhatsApp ya está configurado como `573187137116`.

Ejemplo de configuración (usa tu propio enlace publicado):

```js
window.BAYSTORE_CONFIG = {
  GOOGLE_SHEETS_CSV_URL: "https://docs.google.com/spreadsheets/d/e/ID_PUBLICADO/pub?output=csv",
  WHATSAPP: "573187137116",
  REFRESH_SECONDS: 60
};
```

La página consulta la hoja al abrirse y vuelve a actualizar el catálogo cada 60 segundos. No es necesario volver a publicar la página cuando se cambian precios o cantidades en la hoja, siempre que se mantenga el mismo documento y la publicación siga activa.

## 4. Cómo agregar o modificar productos
- **Agregar:** agrega una fila por talla al final de la pestaña `Catalogo`.
- **Cambiar precio:** cambia el precio en todas las filas/tallas del mismo modelo.
- **Cambiar stock:** modifica la cantidad de la talla correspondiente.
- **Ocultar producto:** cambia `Publicado` a `No` en todas las filas de ese modelo.
- **Agotado:** coloca cantidad `0` en las tallas sin unidades.
- **Foto:** pega el enlace compartido de Drive en cada fila del modelo.

La web agrupa automáticamente las filas del mismo modelo para mostrar un solo producto con sus tallas disponibles.

## 5. Pedidos e inventario
Al pulsar **Pedir por WhatsApp**, se abre un mensaje con producto, talla, cantidad, precio y subtotal estimado. El pedido todavía debe ser confirmado por la tienda. WhatsApp no descuenta el inventario automáticamente: después de confirmar una venta, la encargada debe actualizar la cantidad en Google Sheets.

## 6. Publicar la página
Sube la carpeta completa `Baystore_Shoes_Store` a tu servicio de alojamiento estático (por ejemplo, Netlify). No subas solamente `index.html`: también se necesitan `styles.css`, `store.js`, `config.js` y `logo-baystore.jpeg`.

## Solución de problemas
- **No aparecen productos:** confirma que publicaste la pestaña `Catalogo` como CSV, que el enlace esté completo en `config.js` y que los encabezados no hayan sido cambiados.
- **No aparece una foto:** revisa que el enlace corresponda a un archivo de imagen y que el acceso de Drive sea `Cualquier persona con el enlace – Lector`.
- **No se actualiza:** espera hasta un minuto o recarga la página. Confirma que la publicación CSV siga activa.
- **No aparecen tallas:** revisa que cada fila tenga nombre de producto y talla, y que `Publicado` diga `Sí`.

## Privacidad
El enlace CSV publicado es público. Comparte la hoja original como editora únicamente con la dueña y personas autorizadas. No incluyas información privada en la pestaña publicada. Los enlaces públicos de imágenes permiten que cualquiera que tenga el enlace vea esas imágenes.
