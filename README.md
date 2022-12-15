# Doc
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

## Wie es funktioniert
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
</details>