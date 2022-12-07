# Doc

## Die wichtigsten Klassen des Programms
![Graph](https://raw.githubusercontent.com/UranosNetworkCreation/UranosNetworkCreaton/main/dev-base-graph.png)

### Allgemeine Hinweise
Klasse (<- Vererbte Klasse)

### Excecuter (<- Node)
Die sehr kleine Klasse Excecuter stellt den Drehpunkt für die Ausführung der AI da. Sie ist im wesentlichen dafür zuständig, die events zur Ausführung auszulösen. Hierbei werden folgende Events (signals) definiert:

```GDScript
signal ExecuteSoftware
signal PrepareExecuting
signal ExecutingDone
```

Um die AI auszuführen, stellt die Klasse nun die Funktion `exeCurrentLoaded()` bereit.

### gNode (<- GraphNode)
Die Klasse gNode repräsentiert die Basisklasse alle Blöcke, die sich in die programmierte AI integrieren lassen. Dies betrifft unter anderem "img.gd", "INT.gd" und "DATA1D_STR.gd".

#### Die Verbindungen auf dem Graph erfassen
