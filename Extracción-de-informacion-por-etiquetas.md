A continuación, una función que extrae información, del sitio web indicado por urls en la columna A. La extracción de información, será de una etiqueta en específico, y corresponde al XPATH dado en la formula.

```
function addMetaFormulas() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var startRow = 2;
  var col = 2;
  for (var i = startRow; i <= sheet.getLastRow(); i++) {
    var cell = sheet.getRange(i, col);
    cell.setFormula('=IMPORTXML(A' + i + ',"/html/head/meta[5]")');
    while (cell.isBlank()) {
      SpreadsheetApp.flush();
      Utilities.sleep(1000); 
    }
  }
}
```
Esto agregará la fórmula =IMPORTXML() a cada celda en la columna B, empezando desde la fila 2 (starRow). Utiliza un bucle for para iterar por cada fila.
Dentro del bucle, espera mientras la celda está en blanco, haciendo un flush de SpreadsheetApp y esperando 1 segundo cada vez. Esto dará tiempo para que la fórmula se calcule antes de pasar a la siguiente fila.

Para usarlo, simplemente se debe ejecutar la función addFormulas(). Debes tener los valores de urls en la columna A para que funcione.

Una extensión de la función, es para ir extrayendo título, meta-descripción de cada url.
En este caso, el resultado será en la columna F y G respectivamente.
```
function extractData() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var startRow = 2;
  var colF = 6; // Columna F
  var colG = 7; // Columna G

  for (var i = startRow; i <= sheet.getLastRow(); i++) {
    // Extraer títulos en columna F
    var cellF = sheet.getRange(i, colF);
    cellF.setFormula('=IMPORTXML(A' + i + ',"/html/head/title")');
    
    // Esperar hasta que la celda de la columna F no esté en blanco
    while (cellF.isBlank()) {
      SpreadsheetApp.flush();
      Utilities.sleep(1000);
    }

    // Extraer meta-descripciones en columna G
    var cellG = sheet.getRange(i, colG);
    cellG.setFormula('=IMPORTXML(A' + i + ',"/html/head/meta[2]")');
    
    // Esperar hasta que la celda de la columna G no esté en blanco
    while (cellG.isBlank()) {
      SpreadsheetApp.flush();
      Utilities.sleep(1000);
    }
  }
}
```
