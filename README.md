# Projektseite: UranosNetworkCreation
## Über das Projekt
Das Projekt UranosNetworkCreation (kurz UNC) hat das Ziel die Erstellung von KIs deutlich zu vereinfachen. Dies geschieht über eine visuelle Programmiersprache, mit der sich komplett visuell KIs erstellen lassen, einen Datensatzeditor und ein schnelles Pluginsystem. Bei der Entwicklung wurden der Editor VSCode zu der Bearbeitung von Code verwendet und ansonsten auf Godot gesetzt.

<details>
<summary><b>Projekttagebuch</b></summary>

## Allgemeines
Das akuelle Projekt entwickle ich ausschließlich seit den Herbsferien, da wie bereits besprochen, vorher eigentlich ein anderes Projekt in Teamarbeit angedacht war. Die Leistungen dieses Projekte, insbesondere die erste Auseinandersetzung mit KIs, dienten jedoch teilweise als Grundlage für dieses Projekt. Das Projekttagebuch für jene Informatikstunden können sie [hier](https://github.com/ComputerScienceDevs/infodevs) finden.

## 28. Okt. 2022
Heute habe ich mir erstmals grundlegende Gedanken über die Architektur und die spätere Funktionsweise des Projektes gemacht. Hierbei habe ich zwischen GDScript und C++ zunächst lange Abgewogen. GDscript hat hier nämlich den Vorteil, dass es deutlich scneller geht zu schreiben und auch nicht sehr stark typisiert ist (Der Standarttyp ist Variant), was für eine visuelle Programmiersprache von Vorteil ist. C++ hat vor allem den Vorteil im Bereich der Leistung, was natürlich vor allem für große Rechenaufgaben attrativ ist. Am Ende habe ich mich dann für eine Kombination aus C++ und GDScript entschieden, bei der der Kern des Netzwerks und ein teil des Kerns der Software mit C++ geschrieben ist während die gesamte Oberfläche mit GDScript gecoded ist.

## 1. Nov. 2022
Heute aber ich dann versucht das Repo für diese etwas aufwändige Kombination aus 2 Sprachen einzurichten. Dabei habe ich mich dann mit der Erstellung von submodules in Github auseinandergesetzt und hatte einige Probleme beim Verschieben, sodass ich am Ende die Config-datei manuell editieren musste.

## 2. und 3. Nov. 2022
Heute und gestern habe ich mir erste Gedanken darüber gemacht, wie ich die KI-Plugins (in C++) am besten Laden kann. Hierbei habe ich mich zunächst dafür entschieden dies in C++ zu tun. Bei den Bibliotheken habe ich allerdings nicht nur die Godot-internen libs verwendet, sondern auch teils Standartbibliotheken von C++ wie "vector" oder ähnliches.

## 9. Nov. 2022
Heute habe ich mich mit dem Implementieren einer Editorklasse auseinandergesetzt, die die Anbindung der Steuerelemente des Editors an den Code darstellen sollte. Hierbei hatte ich leider zwischendurch einige Probleme beim Kompilieren, was auch teils meinem sehr langsamen Laptop verschuldet war. Trotzdem hatte ich dann nochmal ein Problem mit fehlerhaften gdnlib Dateien, auf dass ich leider erst sehr spät gekommen bin.

## 10. Nov. 2022
Heute habe ich weiter versucht die Probleme des vorherigen tages zu lösen. Hierbei habe ich mich unter anderem dafür entschieden den Pluginloader und den Editor in die gleiche DynamicLinkLibrary zu packen, da es sonst sehr schwer gewesen wäre vom Editor direkt nach dem Programmstart auf die Funktionen des PluginLoaders zuzugreifen. Des Weiteren habe ich den PluginLoader weitergeschrieben, wobei ich leider sehr große Probleme am Instanzieren der GDNS-Scripte hatte. Infolgedessen habe ich dann auch eine [Frage](https://stackoverflow.com/questions/74387307/how-to-load-a-gdns-script-from-an-other-gdns-script-in-godot) auf stackowerflow gestellt, die leider unbeantwortet blieb.

## 12. Nov. 2022
Heute habe ich die gesamte Architektur grundsätzlich verändert, da ich große Probleme dabei hatte den in C++ geschriebenen PluginLoader fertigzustellen, da mir weder eine ausführliche Google-Suche noch stackoverflow weiterhalf. Dementsprechend habe ich mich dann dazu entschieden den PluginLoader in GDScript zu schreiben und nur die eigentlichen Netzwerkplugins in C++ zu coden. Dementsprechend habe ich dann viel im Repo umsortiert und auch den neuen PluginLoader geschrieben.

## 15. Nov. 2022
Heute habe ich den gestern neu angefangenen PluginLoader weiterentwickelt und auch das Fundament für die aktuelle Oberfläche des Editors gelegt. Hierbei habe ich auch den angefangen den Code für den Graphen zu schreiben, wobei mir folgende [Resource](https://gdscript.com/solutions/godot-graphnode-and-graphedit-tutorial/) sehr viel geholfen hat. Ansonsten war es eigentlich schon fast etwas langweilig, weil ich im wesentlichen im visuellem Editor der Godot-Engine die GUI designed habe.

## 16. und 17. Nov. 2022
Heute habe ich angefangen die grundsätzliche Funktionalität der GraphNodes (Blöcke) zu programmieren. Hierbei habe ich unter anderem eine Funktion namens `updateConnections` hinzufügt, die Verbindung der gesamten Arbeitsfläche einliest und den jeweiligen Nodes zuordnet.
```GDScript
func updateConnections():
	input_conns = []
	output_conns = []
	for _i in range(slotCount):
		input_conns.append(NO_CONN)
		output_conns.append(NO_CONN)
	for conn in GraphE.get_connection_list():
		if(conn.from == name):
			output_conns[conn.from_port] = [conn.to, conn.to_port]
		if(conn.to == name):
			input_conns[conn.to_port] = [conn.from, conn.from_port]
	print("IConnections of ", name, ": ", input_conns, ", ", output_conns)
```
Zudem habe ich die Klasse `Executer`geschrieben, die ich für die zentrale Lenkung der Ausführung des Codes angedacht hatte.

## 22. Nov. 2022
Heute habe ich die im Wesentlichen die Funktionalitaät für Blöcke auf weitere Blöcke erweitert, sodass diese nun auch nutzbar sind. Zudem habe ich die grundlegende Struktur geschrieben, damit Projekte geladen und gespeichert werden können. In Ergänzung dazu habe ich auch eine Statusbar in die GUI eingefügt, auf der eine Info beim Speichern wie auch bei normalen Programmen erscheinen soll.

## 23. Nov. 2022
Da das Projekt in seiner Entwicklung jetzt schon deutlich fortgeschritten war, habe ich einen Ordner für Beispiele zum Repo hinzugefügt und auch die README geupdated. Zudem habe ich eine kleine Zeichnung in excalidraw gestaltet, welche die grundlegene Funktionalität des Programms wiederspiegelt. Zudem musste ich ein bisschen OneDrive (Meinem Cloudprogramm) hinterherräumen, weil es irgendwie verschiedene Dateien falsch kopiert hatte.

## 24. Nov. 2022
Heute habe ich aufgrund des immer größer werdenen Projektes auch eine THIRDPARTY Datei angelegt, die eine gesamte Übersicht über die von dem Projekt verwendeten Resourcen/Dateien gibt. Bei spezifischen Erweiterungen (Themes, etc.) die in sich sehr abgeschlossen sin habe ich die Lizenzdatei immer direkt in den Basisordner gelegt. Zudem habe ich eine kleine Webseite auf Github pages angefangen ([https://uranosnetworkcreation.github.io](https://uranosnetworkcreation.github.io)) und die normale README auch dementsprechend angepasst.

## 30. Nov. 2022
Heute habe ich die Weseite weiter ausgebaut sowie am Dateisystem der Software weiterprogrammiert. Hierbei stellte sich schnell die Schwierigkeit heraus, dass einzelne Nodes oft doppelt gespeichert wurden oder beim Laden von Projekten die Nodes des vorherigen Projektes noch nicht richtig gelöscht waren, wodurch es interne Namenskonflikte bei den Blöcken gab. Hierdurch wurden dann natürlich die abgespeicherten Verbindungsinformationen ungültig.

## 1. Dez. 2022
Nachdem ich gestern schon ein großes Stück an der Webseite weitergemacht hatte, habe ich auch nun heute nochmal einiges ergänzt. So habe ich ein Menü hinzugefügt und dazu dann auch noch die Seite in verschiedene Unterseiten aufgeteilt. Beim Menü erwies sich das Design zudem als etwas schwierig, weil ich es in die Beschreibung der Weseite einfügen musste. Dies war leider dem von mir verwendetem Theme geschuldet, da dies eigentlich keine Menübar vorsah. Zudem habe ich am Laden und Speichern von Dateien weitergearbeitet und das gestrige Problem durch eine Funktion gelöst, die die Array mit den Verbindungen bei Namenskonflikten automatisch updated.
```GDScript
func updateConnectionNodeName(var old : String, var new : String, var conns : Array) -> Array:
	var result : Array = []
	for conn in conns:
		var nconn = conn
		if(nconn.from == old):
			nconn.from = new
		if(nconn.to == old):
			nconn.to = new
		result.append(nconn)
	return result

```

## 8. Dez 2022
Heute habe ich angefangen, an der Dokumentaion der Software und der einzelnen Klassen zu arbeiten. Hierbei habe ich unter anderem eine Dokumentation für den Executer und die grundfunktionsweise der Software geschrieben.

## 9. Dez 2022
Nachdem ich gestern schon die Grundlage für die Dokumentation gesetzt hatte, habe ich dies heute fortgeführt. Hierbei habe ich mir noch mehrmals Gedanken insbesondere ums layout gemacht und mich auch noch einmal in einem längerem Gespräch über die genauen Abgaberichtlinien informiert.

## 10. Dez. 2022
Heute habe ich mich stark damit beschäftigt, wie ich den AI-Kernel programmieren muss. Hierbei haben mir folgene Videos sehr geholfen:
- https://www.youtube.com/watch?v=oCPT87SvkPM
- https://www.youtube.com/watch?v=EAtQCut6Qno&t=0s

## 11. Dez. 2022
Heute habe ich einen riesen Meilenstein in der Entwicklung der Software gesetzt. Ich habe heute nämlich den AI-Kernel so weiter programmiert, dass diese benutzbar ist und sich Layer erstellen, trainieren und verwalten lassen. All dies funktioniert nun über das in C++ geschribene AI-Plugin über welches sich die Layer anhand von Indexes von GDScript aus verwalten lassen.

## 12. Dez. 2022
Heute habe ich zum Einstellungsdialog zuhause nur noch kurz schnell die Möglichkeit hinzugefügt, dass aktuelle theme auf ein anderes zu ändern und noch kurz eine neue Resource für eine Themensammlung implementiert:
```GDScript
extends Resource

export var paths : PoolStringArray
export var optimized : Array
```

## 14. Dez. 2022
Heute habe ich nun entgültig auch die Funktion der AI mit dem Blöcken gekoppelt, sodass die Sprache nun fast voll funktionsfähig ist. Zudem habe ich dass Repo noch etwas aufgeräumt.

## Was jetzt noch kommt/fehlt
Ein großes Programm, was die meine visuelle Sprache noch hat, ist das Error-Handling. Oft ist es nähmlich aktuell so, dass wenn etwas schief läuft das Programm einfach abstürtzt oder garnichts passiert. Zwar lässt sich in der Konsole (Macro `UNC_EXTENDED_DEBUG`), Debugger oft ein Fehler finden, aber langfristig möchte ich hier auf jeden Fall noch eine bessere Lösung implementieren.
</details>

## Mit der Software arbeiten
Hier will ich nur kurz vorstellen, wie sich ein simples Testprojekt erstellen lässt, bei dem eine KI darauf Trainiert wird Zahlenfolgen zu erkennen.<br><br>
**Achtung**: *Die Software ist bis jetzt wenig auf falsche Blockkombinationen, leere Verbindungen, etc. programmiert weshalb diese in solch einem Fall meist abstürtzen wird. Dieses Problem werde ich vor allem im 2. Halbjahr angehen.*<br><br>

<details>
<summary><b>Ein simples Projekt erstellen</b></summary>

### Schritt 1: Das Grundgerüst in der visuellen Programmiersprache bauen
Ein mögliches grundgerüst könnte wie folgt aussehen:
![base graph](https://github.com/UranosNetworkCreation/Doc/blob/main/img/base_graph.png?raw=true)
Die gebaute KI auf dem Bild besitzt zwei Layer, wobei das letzte automatisch als Output Layer fungiert (Wird software-intern geregelt). Ansonsten stellt das Data1D Node die Input Daten bereit. Zum Schluss wird das generierte Array zudem noch zu einem String konvertiert, da mit es in der Seitenbar angezeigt werden kann. Beim bauen des Codes sollte zudem schon automatisch ein Ausgabefeld in der Seitenleiste erstellt werden.

### Schritt 2: Den Code ausführen
Wenn Sie ihre KI fertig gebaut haben und alle Daten eingetragen haben, kann diese nun über das Playzeichen in der Ecke oben Rechts ausgeführt werden. das Ergebnis sollte hierbei in der Seitenleiste sichtbar werden und etwa so aussehen:<br>
![string_output.png](https://github.com/UranosNetworkCreation/Doc/blob/main/img/string_output.png?raw=true)

### Schritt 3: Einen Trainingsbereich erstellen
![AddtrainPoint.gif](https://github.com/UranosNetworkCreation/Doc/blob/main/img/AddTrainPoint.gif?raw=true)
Um einen Trainingsbereich zu erstellen wird unter der Registerkarte Training mit dem Plussymbol eine neue Kachel hinzugefügt. Nun kann, in dem der Pfeil nebem dem Start und Entpunkt auf den Block gezogen wird, ein Bereich festgelegt werden. Die Kachel sollte nun circa so aussehen:<br>
![training_area.png](https://github.com/UranosNetworkCreation/Doc/blob/main/img/training_area.png?raw=true)

### Schritt 4: Den Datensatz des Trainingsbereiches bearbeiten
Um den Datensatz zu erstellen, kann auf den Button "edit dataset ..." geklickt. So gelang man in den Datensatzeditor. Hier lassen sich durch Ziehen input und output Blöcke hinzufügen. So lassen sich die jeweils gewünschten Input und Outputdaten miteinander kombinieren. Ein sehr simpler Dantensatz für die obige KI könnte so aussehen:<br>
![simple_dataset.png](https://github.com/UranosNetworkCreation/Doc/blob/main/img/simple_dataset.png?raw=true)

### Schritt 5: Die AI-Trainieren
**Hinweis:** *Je nach PC und eingestellten Durchläufen kann hier das Programm hurz einfrieren*<br>
**Hinweis:** *Der Button "Run All" funktioniert noch nicht*<br><br>
Um die KI nun zu trainieren kann nun die  Anzahl der Durchläufe eingestellt werden und nun der Button "Start Training" gedrückt werden. Wird die KI nun über den Play-Button nochmal ausgeführt sollten sich die Ergebnisse im Output-Tab dementsprechend anpassen (Auch ohne das drücken des Play button sieht man schon eine Verbesserung, da beim Training die aktuellen Werte auch immer im Outputtab angezeigt werden).<br>
![train_button.png](https://github.com/UranosNetworkCreation/Doc/blob/main/img/train_button.png?raw=true)
</details>

## Wie die Software funktioniert
### Hinweise
#### Syntax der Dokumentation
Klasse (<- Vererbte Klasse)

<details>
<summary>
<b>Allgemeines</b>
</summary>

#### Godot
Das Projekt verwendet Godot 3, welches Standartfunktionen sowie UI-Elemente bereitstellt, auf denen die Software aufbaut. Da das Projekt langfristig auch 3D Modelle generieren soll, wurde hier bewusst auf eine Basis gesetzt, die auch als GameEngine genutzt werden kann, da so viele Funktionen auch im Bezug auf 3D Modelle schon vorhanden sind.<br>
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
</details>
<details>
<summary><b>Die wichtigsten Klassen des Programms (GDScript)</b></summary>

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
Um die Daten bzw. das Ergebnis eines übergeordneten Nodes zu bekommen, stellt die Klasse `gNode` zudem die Funktion `getDataOfPinConn(slot : int, backprop : bool = false, no_conn = "<undefined>")`. Hierbei wird das verbundene Node je nach Fall in der Liste `input_conns` oder `output_conns` gesucht und dann die Funtion `getPinValue(var id : int)` ausgeführt, welche dann je nach Fall entweder einer der schon berechneten Werte zurückgibt oder eine neue Berechnung startet:
```GDScript
if(!calculated):
    updateConnections()
    updateCalc()
return outputs[id]
```
Die Funktion updateCalc wird von Klassen, die von gNode erben, überschrieben, sodass für jeden Block die Werte individuell geupdated werden können.<br>
Ein ähnliches Prinzip wird auch beim "Rückwärtsrechen"(Auflösen des Graphes in die andere Richtung) angewand. Hier wird die Funktion `backCalc` überschrieben.

#### Daten speichern und laden
Um Daten (Einstellungen eines Nodes) aus einer Resource laden und speichern zu können wird eine eigne Resource namens `NodeData` verwendet. Diese besitzt folgende Eigenschaften:
```GDSCript
export var type : String
export var offset : Vector2
export var data : Array
export var name : String
```
Diese werden nun beispielsweise beim speichern des Nodes 
 in der Funktion `getNodeData()` gesetzt:
```GDScript
data.data = DataSync.collectData()
data.offset = self.offset
data.type = self.packedPath
data.name = self.name
```
Der DataSync stellt hierbei eine Instanz der Klasee ResDataSync da, welche ich auch noch einmal erläutere.

### Graph (<- GraphEdit)
Die Klasse `Graph` ist im wesentlichem dafür zuständig, die Events bei der bearbeitung des visuellem Programm zu bearbeiten. Allerdings stellt SIe auch funktionen zum Laden und generieren von Resourcen bereit.
#### Verbindungsanfragen
Verbindungsanfragen werden in der Funktion `_on_GraphEdit_connection_request(from:String, from_slot:int, to:String, to_slot:int)` wie folgt gehandelt:
```GDSCript
# Check if nodes are already connected
for con in get_connection_list():
    if con.to == to and con.to_port == to_slot:
        print("[Graph] Exit from connection request process. Warning: You can't double connect nodes")
        return

# Connect nodes
print("[Graph] Connect nodes(from: ", from, ", from_slot: ", from_slot, ", to: ", to, ", to_slot: ", to_slot, "): ", connect_node(from, from_slot, to, to_slot))
```

Hierbei wird zunächst überprüft ob die beiden Blöcke schon miteinander verbunden sind. Ist dies nicht der Fall, wird eine neue Verbindung zwischen den beiden Blöcken erstellt.
#### Nodes löschen
Auch für das löschen von gibt es eine extra Handlefunktion. Hier werden die nodes realtiv simple nacheinander gelöscht:
```GDScript
for node in nodes:
    print("[GraphEdit] Del node " + node)
    # remove connections
    remove_connections_to_node(node)
    # delete node
    get_node(node).queue_free()
```
#### Laden und Speichern von Daten
Den Rahmen für das Laden und Speichern bildet hier die Resource GraphData, die nur zwei Arrays speichert:
```GDScript
export var nodes : Array = []
export var conns : Array = []
```
Hierbei stellt nun das Array `nodes` relativ einfach eine Sammlung aus allen NodeData Resourcen der jeweiligen Blöcke da
und dass Array `conns` eine aus allen Verbindungen. Diese werden nun in der Funktion `loadData(var data : GraphData)` wie folgt geladen:
```GDScript
# Load nodes
for node in data.nodes:
    print("[Editor][OpenFile] Creating node with type: " + node.type)
    # Create node inst
    var nodeInst = load(node.type).instance()
    nodeInst.offset = node.offset
    nodeInst.name = node.name

    # Add node inst to Graph
    add_child(nodeInst)
    # Init inst and load data
    nodeInst.init_as_node(node.type)
    nodeInst.DataSync.loadData(node.data)
    # Update connections array
    data.conns = updateConnectionNodeName(node.name, nodeInst.name, data.conns)

# Load connections
for conn in data.conns:
    # connect nodes
    print("[Editor][OpenFile] Conn nodes: ", connect_node(conn.from, conn.from_port, conn.to, conn.to_port))
```

Das speichern stellt sich hier etwas simpler da, dies geschieht in der Funktion `getData()`:

```GDScript
# Create new GraphData container
var gData : GraphData = GraphData.new()

# For each block ...
for child in get_children():
    if child is GraphNode:
        print("[Editor][Save] Collect node data ...")
        # Append node specific data to container
        gData.nodes.append(child.getNodeData())

# Add connection to data
gData.conns = get_connection_list()
return gData
```

### ResDataSync (<- Object)
Der `ResDataSync` stellt vor allem beim Laden und Speichern eine wichtige Rolle. Zusammengefasst besitzt Sie die Funktion alle Kinder eines Nodes durch zu gehen, zu gucken, ob dies Eingabefelder (oder ähnliches) sind, und dann eine Array aus den eingebenen Werten zu erstellen. Sie kann zudem auch eine solche Array auf mehere Eingabefelder laden. Die Funktion zum erstellen der Array (`collectData()`) ist wir folgt aufgebaut:
```GDScript
# Create empty array for the result
var result : Array = []
for child in bNode.get_children():
    # Add the child's data to the result if it is a dataField
    if(isDataField(child)):
        result.append(getData(child))
return result
```
Auch zum Laden wird in der Funktion `loadData()`ein ähnlicher Mechanismus verwendet:
```GDSCript
# Loads a data array
func loadData(var data : Array):
    # counter
    var data_idx : int = 0
    for child in bNode.get_children():
        if(isDataField(child)):
            # Apply data
            setData(child, data[data_idx])
            # increment counter
            data_idx += 1
```

### Editor (<- Control)
Einen zentralen Schlüsselpunkt stellt natürlich die Editorklasse da. Eine wichtige Funktion ist hier beim Laden und Speichern das zusammen führen aller Resourcen. Dies geschieht beispielsweise beim Speichern in der Funktion `save_current(path)` wie folgt:
```GDScript
# Create a new file container
var file : UNCFile = UNCFile.new()
# Assign the GraphData
file.GData = GEdit.getData()
# Save the file
return ResourceSaver.save(path, file)
```
Auch das Laden von Dateien ist in der Funktion `open_file(path)` implementiert. Hierbei muss natürlich beachtet werden, dass vorher noch der Editor aufgeräumt werden muss:
```GDScript
# Load file
var file = ResourceLoader.load(path)
# Check if the file is a UNCFile
if(!(file is UNCFile)):
    print("Cannot load file because it's not an UNCFile")
    return -1

# clear
resetEditor()

# Load data
print("[Editor] Load gEdit data ...")
GEdit.loadData(file.GData)
return 0
```
Sowohl beim Laden als auch bei Speichern wird auf die UNCFile-Resource zurückgegriffen. Theoretisch könnte man beim aktuellem Entwicklungsstand auch die GraphData Resource direkt speichern. Allerdings wäre dies dann später, wenn man das Programm erweitern würde, unpraktisch. Aufgrund dessen gibt es die UNCFile Resource, welche allerdings nur eine Eigenschaft besitzt:
```GDScript
export var GData : Resource
```

### PluginLoader (<- Node)
Der PluginLoader stellt die Schnittstelle zwischen dem Teil des Programms in GDScript und dem in GDNative (C++) da. Er lädt nämlich die einzelnen GDN-Scripte (d.h. die Libs, siehe [offizielle Dokumentation](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/gdnative_cpp_example.html)) und verwaltet diese. Grundsätzlich werden alle Scripte aus dem Ordner `res://plugins/gdns` geladen, welche mit keinem Punkt beginnen. Das Laden der eines Plugins ist dann wie folgt umgesetzt:
```GDScript
# Load plugin
var plugin = load("res://plugins/gdns/" + file)
print("[PluginLoader] plugin: ", plugin)

# Instance plugin
var pluginRef = plugin.new()

# Check if pluginInst is valid
if(!pluginRef.has_method("getInfo")):
    print("[PluginLoader] Fatal error: plugin is invalid.")

# Add to plugin array
plugins.append(pluginRef)
print("[PluginLoader] plugin ", pluginRef.getInfo(), " loaded.")
```
</details>

<details>
<summary>
<b>Die wichtigsten Klassen des Programms (C++)</b>
</summary>

### NetworkKernelPlugin (<- godot::Object)
Die Klasse `NetworkKernelPlugin` stellt die Basisklasse für alle KI-Plugins da. Sie gibt die Rahmen an und legt die Funktionen fest, die definiert sein müssen. Dazu gehören unter anderem:
```C++
virtual NetworkKernelPluginInfo getInfo();
virtual int buildLayer(int size, int parent_size, Array weights);
virtual PoolRealArray BackpropLayer(
    int id,
    PoolRealArray underGrad,
    Array underWeights,
    int activationFunc,
    bool outputLayer = false,
    real_t learning_rate = 0.2
);
virtual PoolRealArray SimpleLayerCall(
    int id,
    PoolRealArray input,
    int activationFunc
);
virtual Array getLayerWeights(
    int id
);
```
Des Weiteren übernimmt Sie Teile der Registrierung von Funktionen bei GDN-API in der überschriebenen Funktion `_register_methods()`:
```C++
register_method("getInfo", &NetworkKernelPlugin::getInfoArray);
register_method("simple_layer_call", &NetworkKernelPlugin::SimpleLayerCall);
register_method("build_layer", &NetworkKernelPlugin::buildLayer);
register_method("backprop_layer", &NetworkKernelPlugin::BackpropLayer);
register_method("get_layer_weights", &NetworkKernelPlugin::getLayerWeights);
```
Insgesamt arbeiten die Layer spezifischen Funktionen auf Basis von Indexen, d.h. jedes Layer bekommt einen Index mit welchem dann auch weitere Funktionen aufgerufen werden müssen.

### UranosKernel (<- NetworkKernelPlugin)
Die Klasse `UranosKernel` stellt aktuell das einzige KI-Plugin da. Die KI-Engine ist komplett von mir programmiert. Hier nur ein Einblick in die wichtigsten Teile der Klasse.

#### Die Verwaltung von Layern
Alle aktuellen Layer werden in einer Array des Types `std::vector<Layer>` namens `Layers` gespeichert. Die wesentlichen Funktionen des Layers sind in der `Layer` definiert. Infolge Dessen stellt die Basisklasse `UranosKernel` im wesentlichen eine Reihe von Wrapperfunktionen bereit, die dann mit den benötigten Daten die eigentlichen Funktionen der jeweiligen Instanz der Klasse Layer aufrufen. Hier nur ein Einspiel anhand des Feedforward Prozesses in der Funktion `SimpleLayerCall(int id, PoolRealArray input, int activationFunc`:
```C++
NeuronActivationFunc aFunc;

// Set the activation func
switch (activationFunc)
{
case ACTIVATION_FUNC_SIGMOID:
    aFunc = &UranosKernel::AFuncs::sigmoid;
    break;

default:
    Godot::print("[UranosKernel] No Support for selected activation func. Use sigmoid as default ...");
    aFunc = &UranosKernel::AFuncs::sigmoid;
    break;
}

// Call the Layer and return the result
return Layers[id].getResult(aFunc, input);
}
```
Hierbei wird zunächst Anhand der übergebenen ID der Aktivierungsfunktion der Pointer auf diese bestimmt und dann die eigentliche Funktion der Klasse Layer aufgerufen.

#### Die Klasse Layer
Die Klasse Layer stellt ein Layer da. Dabeuí besitzt die Klasse folgende wichtige Variablen:
```C++
Array weights;
size_t size;
size_t parentSz;
PoolRealArray lastResult;
PoolRealArray lastInput;
```

Hierbei werden in der Variable `weights` die Gewichte des Layers gespeichert, `size` stellt die Größe des Layers da, `parentSz` stellt die Größe der übergebenen Daten des übergeordneten Blockes (Im Graph der Block am linkem Data1D slot) da und lastResult sowie lastInput jeweils die letzten Ergebnisse bzw. Inputs.

##### Das Bauen des Layers
Zum Bauen des Layers stellt die Klasse zwei wichtige Funktionen bereit, nämlich `buildFromWeights(Array n_weights, size_t n_size, size_t parent_size)` und `build(size_t n_size, size_t parent_size, int64_t seed = -1)`. Die Funktion `buildFromWeights` kopiert hierbei vor allem die übergebenen Paramter auf die Eigenschaften des Layersm während die Funktion `build` noch die gesamten Gewichte euf zufällige Werte setzt. Dies geschieht wie folgt:
```C++
Ref<RandomNumberGenerator> rnd = RandomNumberGenerator::_new();

// Create empty weights array
Array Nweights = Array();

// Init rng
if(seed == -1) {
    Godot::print("   | prepare pseudo-rnd: randomize seed ...");
    rnd->randomize();
}
else {
    Godot::print("   | prepare pseudo-rnd: set seed ...");
    rnd->set_seed(seed);
}

// Randomize weights
for(size_t i = 0; i < size;i++) {
    PoolRealArray randomizedArr;
    std::stringstream msg;
    // ...
    for(size_t ci = 0; ci < parent_size; ci++) {
        randomizedArr.push_back(rnd->randf());
    }
    // ...
    Nweights.push_back(randomizedArr);
}
```

Hierbei wird das `weights` Array wie folgt gebaut:
```
[(Neuron 0),    (Neuron 1),    ...]
    |               |           |
   w0, w1, ...     w0, w1, ...  w0, w1, ...
```

##### FeedForward!
Um das Ergebnis des Layers zu berechnen stellt die Klasse die Funktion `getResult(NeuronActivationFunc activation_func, PoolRealArray input)` bereit. Diese berechnet das Ergebnis jedes Neurons so: Ergebnis = Aktivierungsfunktion(w0 * input0, w1 * input1, ...). Dies ist nun mit mehreren Schleifen umgesetzt:
```C++
// Update lastInput
lastInput = input;

// Create result array
PoolRealArray result;

// Do activation_func(wi * conn, ...) for each neuron
for(int i=0;i<size;i++) {
    float neuronInputValue = 0;
    // ...
    for(int ci=0;ci<parentSz;ci++) {
        real_t value = ((PoolRealArray)(weights[i]))[ci] * input[ci];
        // ...
        neuronInputValue += value;
    }
    // ...
    result.append(activation_func(neuronInputValue));
}

// Update lastresult
lastResult = result;

return result;
```

##### Back propagation
Für die Back propagation stellt die Klasse Layer die Funktion `backprop(PoolRealArray under_grad, Array under_weights NeuronActivationFunc derivative, bool outputLayer, real_t learning_rate)`. In der Funktion wird nun zunächst der Gradient für jedes Neuron des Layers ausgerechnet. Dies ist, wie zu erwarten, für HiddenLayer anders umgesetzt als für outputlayer. Für die OutputLayer ist die Implementierung nämlich noch relativ einfach:
```C++
for (int idx = 0; idx < size; idx++) {
        // push the calculated result back
        gradient.push_back((lastResult[idx] - under_grad[idx]) * derivative(lastResult[idx]));
    }
```
Und auch für die HiddenLayer ist es eigentlich machbar (hierbei hat mir [folgendes](https://www.youtube.com/watch?v=EAtQCut6Qno&t=0s) Video sehr geholfen):
```C++
for (int idx = 0; idx < size; idx++) {
    real_t sum = 0;
    // ci = connection index
    for(int ci = 0; ci < under_weights.size(); ci++) {
        // Add to sum
        sum += ((PoolRealArray)under_weights[ci])[idx] * under_grad[ci];                                                                             
    }

    // push back calculated result
    gradient.push_back(sum * derivative(lastResult[idx]));
}
```
Nachdem man nun den Gradienten des Layers erfolgreich bestimmt hat, muss man nur noch die Gewichte mit diesem updaten. Dies lässt sich wie folgt tun:
```C++
Array nWeights;
for (int idx = 0; idx < size; idx++) {
    PoolRealArray connWeights;
    for(int i = 0; i < parentSz; i++) {
        // ...
        // Get last input
        real_t lastInputV = lastInput[i];
        // ...
        // Get gradient
        real_t gradientV = gradient[idx];
        // ...
        // Calculate nValue
        real_t nValue = ((PoolRealArray)weights[idx])[i] - (learning_rate * lastInputV * gradientV);
        // ...
        // push back to weights array
        connWeights.push_back(nValue);
    }

    // push back to new weights
    nWeights.push_back(connWeights);
}
// ...
// Update weights
weights = nWeights;
```

Nun wird schlussendlich nurnoch der Gradient für die Berechnung des nächsten Gradienten zurückgegeben:
```C++
return gradient;
```
</details>