# listaritensdodrive
//Um trecho de código que traz todas os links de uma pasta do drive em uma planilha de Excel e com ultima alteração, nome do Autor, nome do documento e link. Ideal para organizar pastas do drive.
//digitar na barra de busca (logado no google), "sheets.new" abrirá uma nova aba do Google planilhas.
// Ir em ferramentas > <> Editor de Script e inserir esse código abaixo dar play e dar as devidas permissões e Pronto \0/

```javascript
function onOpen() {  
  // Cria uma opção no menu
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var searchMenuEntries = [ {name: "Executar", functionName: "listFiles"}];
  ss.addMenu("Listar arquivos", searchMenuEntries);
}
```

```javascript
function listFiles() {
  // Recupera a planilha e a aba ativas
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var ssid = ss.getId();
  
  // Procura dentro da mesma pasta da planilha atual
  var ssparents = DriveApp.getFileById(ssid).getParents();
  var sheet = ss.getActiveSheet();
 
  // Configura um título para apresentar os resultados
  var headers = [["Atualizado em", "Proprietário", "Nome do arquivo", "URL do arquivo"]];
  sheet.getRange("A1:D").clear();
  sheet.getRange("A1:D1").setValues(headers);
  
  // Percorre todos os arquivos
  var folder = ssparents.next();
  var files = folder.getFiles();
  var i=1;
  while(files.hasNext()) {
    var file = files.next();
    if(ss.getId() == file.getId()){ 
      continue; 
    }
    sheet.getRange(i+1, 1, 1, 4).setValues([[file.getLastUpdated(),file.getOwner().getName(),file.getName(), file.getUrl()]]);
    i++;
  }
}
```

