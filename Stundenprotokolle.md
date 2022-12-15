# Stundenprotokolle

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