Para abrir una lista de urls en una columna, es posible automatizarlo con una función de AppScript, de la siguiente manera:

```
function openUrlsInNewTabs() {
  // Obtiene la hoja de cálculo activa
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  
  // Obtiene los datos de la columna A en el rango A2:A26
  var data = sheet.getRange("A2:A25").getValues();
  
  // Crea una matriz para almacenar las URL
  var urls = [];
  
  // Recorre los datos y agrega las URL a la matriz
  for (var i = 0; i < data.length; i++) {
    var url = data[i][0];
    if (url) {
      urls.push(url);
    }
  }
  
  // Crea el código HTML y JavaScript para abrir las URL en nuevas pestañas
  var html = '<script>';
  for (var i = 0; i < urls.length; i++) {
    html += 'window.open("' + urls[i] + '");';
  }
  html += '</script>';
  
  // Crea y muestra una interfaz de usuario HTML
  var ui = HtmlService.createHtmlOutput(html)
      .setTitle('Abriendo URLs en nuevas pestañas')
      .setWidth(300)
      .setHeight(100);
  
  SpreadsheetApp.getUi().showModelessDialog(ui, 'Abriendo URLs');
}
```
Cambiando el rango A2:A25 según sea necesario
