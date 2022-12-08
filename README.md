# Doc

### Allgemeine Hinweise
#### Syntax der Dokumentation
Klasse (<- Vererbte Klasse)

#### Godot
Das Projekt verwendet Godot 3, welches Standartfunktionen sowie UI-Elemente bereitstellt, auf denen die Software aufbaut. Da das Projekt langfristog auch 3D Modelle generieren soll, wurde hier bewusst auf eine Basis gesetzt, die auch als GameEngine genutzt werden kann, da so viele Funktionen auch im Bezug auf 3D Modelle schon vorhanden sind.<br>
Die Engine selbst nutzt eine Art Baumsystem, welches immer von unterschiedlichen Szenen ergänzt wird. Alle Kinder des Baumsystems erben dabei von der Klasse Node oder Klassen, die wiederrum von Node erben. So könnte eine Szene mit zwei Button und einem Label von der Struktur so aussehen:

```
Control (<- Control)
 | Label (<- Label)
 | Container (<- VBoxContainer)
    | Button1 (<- Button)
    | Button2 (<- Button)
```

Mit jeweiligen Kinder des Baumsystem können nun Scripte verknüpft werden. Standartmäßig wird hierfür GDScript genutzt, eine objektorientierte und für Godot optimierte Version von python. Jede Scriptdatei stellt hierbei zwingend eine eigene Klasse da, welche von der Klasse des verknüpften Nodes oder von einer übergeordneten Klasse erben muss. Angeben wird dies mit dem Schlüsselwort `extends`.

##### C++ (GDNative)
Es kann auch C++ Code verknüpft werden, hier stellt sich jedoch die Funktionsweise etwas anders. Hierfür wird der Code zunächst in eine Dynamic-Link-Library (z. B. dll) kompiliert. Nun wird eine `.gdnlib` Datei erstellt die auf die libs für die einzelnen Plattformen verweist. Nun wird für jede Anbindung an Node des Baumsystem eine `.gdns` Datei erstellt, die unteranderem den Klassennamen und die verknüpfte `.gdnlib` Datei angibt.

## Die wichtigsten Klassen des Programms
![Graph](https://raw.githubusercontent.com/UranosNetworkCreation/UranosNetworkCreaton/main/dev-base-graph.png)

### Excecuter (<- Node)
Die sehr kleine Klasse Excecuter stellt den Drehpunkt für die Ausführung der AI da. Sie ist im wesentlichen dafür zuständig, die events zur Ausführung auszulösen. Hierbei werden folgende Events (signals) definiert:

```GDScript
signal ExecuteSoftware
signal PrepareExecuting
signal ExecutingDone
```

Um die AI auszuführen, stellt die Klasse nun die Funktion `exeCurrentLoaded()` bereit.

### gNode (<- GraphNode)
Die Klasse gNode repräsentiert die Basisklasse alle Blöcke, die sich in die programmierte AI integrieren lassen. Dies betrifft unter anderem `img.gd`, `INT.gd` und `DATA1D_STR.gd`. Dabei kann die Klasse `gNode` als Vorschau und als aktives Node initalisiert (`init_as_preview(phantomID, previewInst, packedPth)`, `init_as_node(packedPth)`) werden.

#### Die Verbindungen auf dem Graph erfassen
Die Verbindungen eines gNodes (Klasse, die von gNode erbt) repräsentieren zwei Arrays, nähmlich `input_conns[]` und `output_conns[]`. Aktualisiert werden diese von der Funktion `updateConnections()`. Hierbei werden mittles einer for-Schleife alle verbindungen duchgegangen und anschließend geprüft, ob diese zudem jeweiligen Node gehören. Ist dies der Fall, werden die Arrays dementsprechend ergänzt:
```GDScript
for conn in GraphE.get_connection_list():
		if(conn.from == name):
			output_conns[conn.from_port] = [conn.to, conn.to_port]
		if(conn.to == name):
			input_conns[conn.to_port] = [conn.from, conn.from_port]
```

#### Die Daten einer Verbindung auslesen
Um die Daten bzw. das Ergebnis eines übergeordneten Nodes zu bekommen, stellt die Klasse `gNode` zudem die Funktion `getDataOfPinConn(slot : int, backprop : bool = false, no_conn = "<undefined>")`. Hierbei wird das verbundene Node je nach Fall in der Liste `input_conns` oder `output_conns` gesucht und dann die Funtion `getPinValue(var id : int)` ausgeführt, welche dann je nach Fall entweder einer der schon berechneten Werte zurückgint oder eine neue Berechnung startet:
```GDScript
if(!calculated):
    updateConnections()
    updateCalc()
return outputs[id]
```
Die Funktion updateCalc wird jenach 