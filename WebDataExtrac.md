# Función de extracción de datos de un sitio web, en Google Sheet

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
    
    hoja.getRange(fila, 6).setValue(title);
    hoja.getRange(fila, 7).setValue(description);
    hoja.getRange(fila, 8).setValue(keywords);
  }
}

## Utilizando la librería Cheerio

Para utilizar Cheerio, primero debes agregar la biblioteca a tu proyecto de Google Apps Script. Para hacer esto, sigue estos pasos:

- Abre tu proyecto de Google Apps Script.
- Haz clic en el menú "Recursos" y selecciona "Bibliotecas".
- En el cuadro "Agregar biblioteca", ingresa el siguiente identificador de biblioteca: 1ReeQ6WO8kKNxoaA_O0XEQ589cIrRvEBA9qcWpNqdOP17i47u6N9M5Xh0 y haz clic en "Agregar".
- En el cuadro "Versión", selecciona la versión más reciente y haz clic en "Guardar".

Una vez que hayas agregado la biblioteca Cheerio a tu proyecto, puedes utilizarla para analizar el HTML de cada URL y extraer el título, la meta-descripción y la palabra clave. Aquí te dejo un ejemplo de cómo podríamos hacerlo:

## Resultado

Esta función utiliza la biblioteca Cheerio para analizar el contenido HTML de cada URL y extraer el título, la meta-descripción y la palabra clave. Luego, los valores extraídos se colocan en las columnas correspondientes (F, G y H).
