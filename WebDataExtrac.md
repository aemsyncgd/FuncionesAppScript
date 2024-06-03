# Función de extracción de datos de un sitio web, en Google Sheet

```
function extraerInformacion() {
  var hoja = SpreadsheetApp.getActiveSheet();
  var filaInicial = 2;
  var ultimaFila = hoja.getLastRow();
  
  for (var fila = filaInicial; fila <= ultimaFila; fila++) {
    var url = hoja.getRange(fila, 1).getValue();
    var contenido = UrlFetchApp.fetch(url).getContentText();
    
    var $ = Cheerio.load(contenido);
    
    var title = $("title").text();
    var description = $("meta[name='description']").attr("content") || "";
    var keywords = $("meta[name='keywords']").attr("content") || "";
    // Extraer el h1 dentro del div con clase product__title
    var h1Title = $('.product__title > h1').text() || "";
    
    hoja.getRange(fila, 6).setValue(title);
    hoja.getRange(fila, 7).setValue(description);
    hoja.getRange(fila, 8).setValue(keywords);
    hoja.getRange(fila, 9).setValue(h1Title);
  }
}
```

## Utilizando la librería Cheerio

Para utilizar Cheerio, primero debes agregar la biblioteca a tu proyecto de Google Apps Script. Para hacer esto, sigue estos pasos:

- Abre tu proyecto de Google Apps Script.
- Haz clic en el menú "Recursos" y selecciona "Bibliotecas".
- En el cuadro "Agregar biblioteca", ingresa el siguiente identificador de biblioteca: 1ReeQ6WO8kKNxoaA_O0XEQ589cIrRvEBA9qcWpNqdOP17i47u6N9M5Xh0 y haz clic en "Agregar".
- En el cuadro "Versión", selecciona la versión más reciente y haz clic en "Guardar".

Una vez que hayas agregado la biblioteca Cheerio a tu proyecto, puedes utilizarla para analizar el HTML de cada URL y extraer el título, la meta-descripción y la palabra clave. Aquí te dejo un ejemplo de cómo podríamos hacerlo:

## Resultado

Esta función utiliza la biblioteca Cheerio para analizar el contenido HTML de cada URL y extraer el título, la meta-descripción y la palabra clave. Luego, los valores extraídos se colocan en las columnas correspondientes (F, G y H ).

    hoja.getRange(fila, 6).setValue(title);
    hoja.getRange(fila, 7).setValue(description);
    hoja.getRange(fila, 8).setValue(keywords);

Aquí, puede cambiar las celdas donde se obtendrá el resultado. Esto va de acuerdo a las necesidades de uso.


## Actualización

En algunos sitios web, donde el código para el caso de alguno de estos items a extraer, el CMS agrega espacios, o saltos de línea, los cuales hacen que no sea posible detectar correctamente el contenido de las etiquetas consultadas.

A continuación, una modificación de la función, para mejorar la extracción de títulos.

```
function extraerInformacion() {
  var hoja = SpreadsheetApp.getActiveSheet();
  var filaInicial = 1;
  var ultimaFila = hoja.getLastRow();
  
  for (var fila = filaInicial; fila <= ultimaFila; fila++) {
    var url = hoja.getRange(fila, 1).getValue();
    
    try {
      var contenido = obtenerContenido(url);
      var title = extraerTitulo(contenido);
      var description = extraerMetaContenido(contenido, "description");
      var keywords = extraerMetaContenido(contenido, "keywords");
      
      hoja.getRange(fila, 2).setValue(title);
      hoja.getRange(fila, 3).setValue(description);
      hoja.getRange(fila, 4).setValue(keywords);
    } catch (error) {
      Logger.log("Error al procesar la URL: " + url + ". Error: " + error.message);
    }
  }
}

function obtenerContenido(url) {
  var response = UrlFetchApp.fetch(url);
  if (response.getResponseCode() === 200) {
    return response.getContentText();
  } else {
    throw new Error("No se pudo obtener el contenido de la URL. Código de respuesta: " + response.getResponseCode());
  }
}

function extraerTitulo(contenido) {
  var $ = Cheerio.load(contenido);
  var title = $("title").html();  // Obtén el contenido HTML de la etiqueta title
  
  // Limpia el título eliminando caracteres especiales y espacios adicionales
  title = title.replace(/<title>/i, '').replace(/<\/title>/i, '').trim();
  
  return title;
}

function extraerMetaContenido(contenido, metaName) {
  var $ = Cheerio.load(contenido);
  return $("meta[name='" + metaName + "']").attr("content") || "";
}

```
