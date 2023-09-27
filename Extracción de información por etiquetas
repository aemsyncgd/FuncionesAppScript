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
