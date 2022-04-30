# Portfolio

## Darstellen hierarchischer Beziehungen von Notizen im Wissensmanagementprogramm Obsidian

Das Problem: Obsidian basiert auf der Idee, dass Notizen nicht in Ordnern sortiert, sondern durch Verknüpfungen untereinander geordnet werden sollen. Das ist viel flexibler als feste hierarchische Strukturen, aber wenn man viele Notizen hat, ist es schwer den Überblick zu behalten da man immer nur Links der nächsten Ebene sehen kann. Man hat keinen Anhaltspunkt, in welchem Geflecht von Notizen man sich befindet oder wie genau es strukturiert ist.

Die Lösung:

Meine Erweiterung basiert auf der Idee, alle Notizen durch verschiedene, zunehmend abstraktere andere Notizen mit einem zentralen Index zu verknüpfen, also Kategorien und Unterkategorien realisiert als Notizen anstatt Ordner.

Mein Programm analysiert alle Notizen und findet die kürzesten Pfade zu jeder Notiz, also sozusagen die Überkategorien. Diese werden automatisch als Pfad dargestellt, außerdem werden die Notizen, die hierarchisch unter der geöffneten Notiz liegen, als Verwandtschaftsbaum dargestellt. Hier ist ein Beispiel der automatisch generierten Übersicht aus meiner eigenen Sammlung:

![]

Dies bringt eine große Erleichterung im Navigieren und Sortieren großer Sammlungen von Notizen.
Gleichzeitig bleibt die Flexibilität der verweisbasierten Organisation erhalten.


Technologien: Das Backend ist in TypeScript geschrieben, und die Frontend-Elemente sind als Svelte-Komponenten realisiert.

## Importieren von Englischvokabeln von Onlinewörterbüchern in den Vokabeltrainer Anki (privates Projekt, GitHub)

Das Problem: Um mein Englisch zu verbessern füge ich konstant englische Begriffe, die mir neu sind, in meinen Vokabeltrainer ein. Die Definitionen beziehe ich vom Onlinewörterbuch dict.cc und die Audiodateien der Aussprache vom Cambridge Dictionary. Ich brauchte ein Programm, das das Herunterladen dieser Informationen automatisiert.

Die Lösung:

Ich habe eine Erweiterung für Anki geschrieben, die die Liste neuer Wörter von dict.cc lädt und automatisch die Aussprache als ogg-Datei herunterlädt. Vor dem Import werden die neuen Wörter dem Nutzer präsentiert, damit dieser sie von Hand korrigieren und in eine einheitliche Form bringen kann. Gleiche Wörter mit verschiedenen Bedeutungen werden gruppiert, ganz links wird mit abwechselnden Symbolen die Gruppierung verdeutlicht:


![Edit words dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20words%20dialog.png)

Anschließend werden die englischen Wörter erneut präsentiert, um den idealen Suchausdruck für die Aussprache zu finden. Rechts wird die Lautschrift der gefundenen Aussprache angezeigt, daneben außerdem die Häufigkeit der Suchphrase im englischen Sprachgebrauch, welche von phrasefinder.io geladen wird. Anhand der Häufigkeit werden die Karten nachher in verschiedene Stapel sortiert.

![Edit scrubbing output dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20scrubbing%20output%20dialog.png)

Nach der Bestätigung werden die Karten automatisch erstellt, aber zuerst deaktiviert, da der Lerneffekt gering ist, wenn man die Karten direkt nach dem Erstellen wiederholt. Nach ein paar Tagen werden sie wieder aktiviert und können dann gelernt werden.

Technologien: Python für das Backend und PyQt5 für die GUI-Elemente


## Hintergrunddienste für verschiedene Aufgaben (privates Projekt, GitHub)

Das Problem: Ich brauchte eine Plattform, in der ich verschiedene Dienst erstellen konnte, die kleine Aufgaben übernehmen, zum Beispiel die Interaktion mit meinem Todomanagerdienst und meinen Smart-Home-Lampen.

Die Lösung:

Ich habe eine Elternklasse erstellt, welche Aufgaben übernimmt, die jeder Dienst braucht, zum Beispiel das Auslösen des Haupttasks in bestimmten Zeitabständen oder das in der Kommandozeile Ausgeben und in Logdateien Speichern von hilfreichen Hinweisen. Von dieser Klasse erben viele verschiedene Dienste, die nur noch das initialisieren müssen, was sie für ihre jeweilige Aufgabe brauchen. Aufgaben, die diese Dienste übernehmen sind zum Beispiel:

- Wenn ich meinen Kindle per USB verbinde, automatisch aus der SQlite-Datenbank des Gerätes neue Englischvokabeln, die ich mir zum Lernen markiert habe, zum Import in meinen Vokabeltrainer auslesen

- Über die API von meinem Todomanagerdienst bestimmte Projekte überwachen, und den Inhalt von neuen Einträgen in diesen Projekten in einer bestimmten Suchmaschine im Browser öffnen. Damit kann ich mir zum Beispiel unterwegs neue Englischvokabeln notieren und diese direkt recherchieren wenn ich wieder am Computer bin.

- Die Routinen meiner smarten Lampen täglich um ein paar Minuten verschieben, um eine Umstellung auf neue Schlafenszeiten sanft zu gestalten

Technologien: Python



## Herunterladen von YouTube-Videos für den Import in Obsidian (privates Projekt, GitHub)

Das Problem: Ich möchte oft Notizen zu einem YouTube-Video verfassen. Dabei ist es hilfreich, das Transkript des Videos zur Hand zu haben, und es empfiehlt sich, das Video herunterzuladen, falls es irgendwann gelöscht wird. Das alles von Hand zu machen ist viel Arbeit.

Die Lösung:

Mein Skript lädt das Video herunter und generiert eine Markdown-Datei, die alle wichtigen Informationen wie Beschreibung, Uploaddatum und den Link zu der Videodatei enthält. Diese Datei kann dann in Obsidian betrachtet und bearbeitet werden. Sie enthält außerdem, soweit vorhanden, das automatisch von YouTube generierte oder manuell erstellte Transkript, das zur besseren Übersichtlichkeit in 30-Sekunden-Blöcke aufgeteilt wird.

Technologien: Python. Für das Herunterladen der Videos und des Transkripts benutze ich verschiedene Python-Libraries. Nach dem Herunterladen werden die Audio- und Videospur noch mit ffmpeg kombiniert.



## Automatische Synchronisation für Anki (GitHub)

Das Problem:  Der Vokabeltrainer Anki kann mobil und auf dem Computer genutzt werden, und über die Cloud synchronisiert werden. Dass muss aber von Hand ausgelöst werden. Wenn man das Programm über den Tag verteilt viel benutzt, kann das anstrengend werden, und sogar zu einem unangenehmen Phänomen führen, bei dem man aus Gewohnheit immer wieder auf den Button klickt, selbst wenn es gar nichts zu synchronisieren gibt.

Die Lösung:

Ich habe eine Erweiterung geschrieben, die die Synchronisation automatisch auslöst, wenn der Nutzer das Programm ein paar Minuten nicht benutzt hat. Die Inaktivität wird festgestellt, indem Maus- und Tastaturevents überwacht werden. Außerdem wird sichergestellt, dass keine Karten wiederholt oder bearbeitet werden. All das kann in einem Einstellungsdialog geregelt werden:

![]

Technologien: Python für das Backend und PyQt5 für die GUI-Elemente
