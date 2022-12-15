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
Heute habe ich den gestern neu angefangenen PluginLoader weiterentwickelt und auch das Fundament für die aktuelle Oberfläche des Editors gelegt. Hierbei habe ich auch den angefangen den Code für den Graphen zu schreiben, wobei mir folgende [Resource](https://gdscript.com/solutions/godot-graphnode-and-graphedit-tutorial/) sehr viel geholfen hat.