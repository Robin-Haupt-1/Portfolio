# Portfolio

## Darstellen hierarchischer Beziehungen von Notizen im Wissensmanagementprogramm Obsidian

Das Problem: Obsidian basiert auf der Idee, dass Notizen nicht in Ordnern sortiert, sondern durch Verknüpfungen untereinander geordnet werden sollen. Das ist viel flexibler als feste hierarchische Strukturen, aber wenn man viele Notizen hat, ist es schwer den Überblick zu behalten, da man immer nur Links der nächsten Ebene sehen kann. Man hat keinen Anhaltspunkt, in welchem Geflecht von Notizen man sich befindet oder wie genau es strukturiert ist.

Die Lösung:

Meine Erweiterung basiert auf der Idee, alle Notizen durch verschiedene, zunehmend abstraktere andere Notizen mit einem zentralen Index zu verknüpfen, also Kategorien und Unterkategorien realisiert als Notizen anstatt Ordner.

Mein Programm analysiert alle Notizen und findet die kürzesten Pfade von der zentralen Notiz zu jeder anderen Notiz, also sozusagen die Überkategorien. Diese werden automatisch als Pfad dargestellt, außerdem werden die Notizen, die hierarchisch unter der geöffneten Notiz liegen, als Verwandtschaftsbaum dargestellt. Hier ist ein Beispiel der automatisch generierten Übersicht aus meiner eigenen Sammlung:

![MOC example view](https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20view%20with%20technology%20tree.png)

Dies bringt eine große Erleichterung im Navigieren und Sortieren großer Sammlungen von Notizen. Gleichzeitig bleibt die Flexibilität der verweisbasierten Organisation erhalten, da tausende Notizen völlig neu in die Hierarchie einsortiert werden können durch das Abändern eines einzigen Links.

Die Ansicht ermöglicht es, auf die Namen der Notizen zu klicken, um sie im Editor zu öffnen. Es gibt Buttons um die Verknüpfungen auf mehr oder weniger Ebenen der Hierarchie anzuzeigen oder den Verwandtschaftsbaum einer Notiz offen zu halten während man durch andere Notizen navigiert.

Ich habe diese Erweiterung als Open Source veröffentlicht. Seitdem wurde sie 10.000 mal heruntergeladen und hat 17 Updates erhalten, in denen ich Ideen und Bugfixes realisiert habe, die von den Nutzern [auf GitHub](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content/issues?q=+is%3Aissue+-author%3ARobin-Haupt-1+) und [im offiziellen Obsidianforum](https://forum.obsidian.md/t/map-of-content-plugin-release/25209/) eingebracht wurden. Sie hat auf GitHub schon fast 100 Sterne erhalten.


### Einstellungen

In dem Einstellungsdialog können die zu beachtenden Dateien anhand ihres Namens oder Pfades gefiltert und einige andere Verhaltensweisen personalisiert werden.

![MOC settings](https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20settings.png)

### Verwendete Technologien

Das Backend ist in TypeScript geschrieben, und die Frontend-Elemente sind als Svelte-Komponenten realisiert.

[GitHub Repo](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content)

## Importieren von Englischvokabeln von Onlinewörterbüchern in den Vokabeltrainer Anki

Das Problem: Um mein Englisch zu verbessern füge ich konstant englische Begriffe, die mir neu sind, in meinen Vokabeltrainer ein. Die Definitionen beziehe ich vom Onlinewörterbuch dict.cc und die Audiodateien der Aussprache vom Cambridge Dictionary. Ich brauchte ein Programm, das das Herunterladen dieser Informationen automatisiert.

Die Lösung:

Ich habe eine Erweiterung für Anki geschrieben, die die Liste neuer Wörter von dict.cc lädt und automatisch die Aussprache als ogg-Datei herunterlädt. Vor dem Import werden die neuen Wörter dem Nutzer präsentiert, damit dieser sie von Hand korrigieren und in eine einheitliche Form bringen kann. Gleiche Wörter mit verschiedenen Bedeutungen werden gruppiert, ganz links wird mit abwechselnden Symbolen die Gruppierung verdeutlicht:

![Edit words dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20words%20dialog.png)

Anschließend werden die englischen Wörter erneut präsentiert, um den idealen Suchausdruck für die Aussprache zu finden. Rechts wird die Lautschrift der gefundenen Aussprache angezeigt, daneben außerdem die Häufigkeit der Suchphrase im englischen Sprachgebrauch, welche von phrasefinder.io geladen wird. Anhand der Häufigkeit werden die Karten nachher in verschiedene Stapel sortiert.

![Edit scrubbing output dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20scrubbing%20output%20dialog.png)

Nach der Bestätigung werden die Mediendateien auf dem Computer gespeichert und die Karten erstellt.

### Verwendete Technologien

Python für das Backend und PyQt5 für die GUI-Elemente. Für das Abfragen der Onlinewörterbücher nutze ich deren REST-API sofern vorhanden oder scrape das HTML mit BeautifulSoup. Bei dem Cambridge Dictionary muss man auch einen korrekten User-Agent mitsenden, um die Inhalte abrufen zu können.

[GitHub Repo](https://github.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki)


## Automatische Synchronisation für Anki

Das Problem:  Der Vokabeltrainer Anki kann mobil und auf dem Computer genutzt werden, und über die Cloud synchronisiert werden. Das muss aber von Hand ausgelöst werden. Wenn man das Programm über den Tag verteilt viel benutzt, kann das anstrengend werden, und sogar zu einem unangenehmen Phänomen führen, bei dem man aus Gewohnheit immer wieder auf den Button klickt, selbst wenn es gar nichts zu synchronisieren gibt.

Die Lösung:

Ich habe eine Erweiterung geschrieben, die die Synchronisation automatisch auslöst, wenn der Nutzer das Programm ein paar Minuten nicht benutzt hat. Die Inaktivität wird festgestellt, indem Maus- und Tastaturevents überwacht werden. Außerdem wird sichergestellt, dass keine Karten wiederholt oder bearbeitet werden. All das kann in einem Einstellungsdialog geregelt werden:

![auto sync options](https://raw.githubusercontent.com/Robin-Haupt-1/Auto-Sync-Anki-Addon/main/doc/2022-04-25%20options%20dialog.png)

Ein Logfenster ermöglicht es, die Aktivität der Erweiterung zu überwachen:

![auto sync log window](https://raw.githubusercontent.com/Robin-Haupt-1/Auto-Sync-Anki-Addon/main/doc/2022-04-25%20log%20window.png)

Ich habe die Erweiterung veröffentlicht. Sie wurde bis jetzt 650 mal heruntergeladen und hat überwiegend positive Bewertungen erhalten. [^autosyncdownloadpage]

### Verwendete Technologien

Python für das Backend und PyQt5 für die GUI-Elemente

[GitHub Repo](https://github.com/Robin-Haupt-1/Auto-Sync-Anki-Addon)


## Informationen über Nahrungsmittelkäufe über die Kommandozeile eingeben und in einer relationalen Datenbank speichern

Das Problem: Ich möchte wissen, was ich an Essen kaufe, um besser beurteilen zu können wie ausgewogen meine Ernährung ist und zu wissen wie viel ich für welche Lebensmittel ausgebe.
Bestehende Möglichkeiten Kassenzettel automatisch digitalisieren zu lassen sind unbefriedigend oder teuer, außerdem sind die Informationen auf den Zetteln oft nicht vollständig oder aussagekräftig genug, sodass man auf jeden Fall manuell dazuarbeiten muss. Die Informationen von Hand in eine Exceldatei o.ä. zu übertragen würde zu viel Zeit kosten, und die Auflistung wäre in diesem Format statistisch nicht unbedingt gut auszuwerten und würde schnell unübersichtlich werden und mühsam zu handhaben sein.

Die Lösung:

Ich habe eine Kommandozeilenapplikation geschrieben, die die Informationen über vergangene Einkäufe aus einer Datenbank ließt und sie bei der Eingabe der neuen Einkäufe vorschlägt. Dadurch wird der Zeitaufwand für das Übertragen der Kassenbons stark gesengt, und es enstehen keine doppelten Einträge für den gleichen Artikel, die später bei der statistischen Auswertung wieder einander zugeordnet werden müssten. Auch wird zwischen abstrakteren Produktarten und konkreten Artikeln unterschieden, wobei jeder konkrete Artikel einem abstrakten Produkttyp zugeordnet sein muss. So kann ein Warentyp im gesamten analysiert werden, egal wo er unter welchen Marke in welcher Packungsgröße erworben wurde.

Die in der Kommandozeile eingegebenen Informationen werden über REST-API-Endpunkte an einen Server übertragen, der sie in die Datenbank schreibt. Diese Segregation der Aufgabenbereiche folgt einem Microserviceansatz. In meiner Implementation ist die MySQL-Datenbank beliebig mit einer anderen Art von Datenbank außtauschbar, da der Server unter Beachtung des Dependency-Inversion-Prinzips mit einem unspezifischen Interface arbeitet, das durch die MySQL-Schnittstelle oder eine andere Datenbankschnittstelle erfüllt werden kann.

### Screenshot vom Gebrauch der Applikation zum Speichern eines gekauften Artikels

![FPT enter purchase](https://raw.githubusercontent.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/main/doc/cli-screenshot-m%C3%BCsli.png)

### Video vom Einpflegen der Daten eines neuen Produkts

https://user-images.githubusercontent.com/85873542/166096132-24622d5b-f95c-44ec-b41b-d95c5311ba8a.mp4

(Video geht nicht? [Hier](https://raw.githubusercontent.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/main/doc/2022-04-15-create-concrete-item.gif) klicken)

### MySQL-Tabellen

Die Informationen über Geschäfte, Produkte und Einkaufshistorie werden in verschiedenen Tabellen gespeichert, die die Einträge anderer Tabellen über ihre ID referenzieren.

### Geschäfte

![FPT stores table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-stores-table.png)

### Abstrakte Produkttypen

![FPT abstract items table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-abstract-items-table.png)

### Konkrete Artikel

![FPT concrete items table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-concrete-items-table.png)

### Einkaufshistorie

![FPT purchases table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-24-purchases_table.png)


[GitHub Repo](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis)

## Hintergrunddienste für verschiedene Aufgaben (privates Projekt)

Das Problem: Ich brauchte eine Plattform, in der ich verschiedene Dienst erstellen konnte, die kleine Aufgaben übernehmen, zum Beispiel die Interaktion mit meinem Todomanagerdienst und meinen Smart-Home-Lampen.

Die Lösung:

Ich habe eine Elternklasse erstellt, welche Aufgaben übernimmt, die jeder Dienst braucht, zum Beispiel das Auslösen des Haupttasks in bestimmten Zeitabständen oder das in der Kommandozeile Ausgeben und in Logdateien Speichern von hilfreichen Hinweisen. Von dieser Klasse erben viele verschiedene Dienste, die nur noch das initialisieren müssen, was sie für ihre jeweilige Aufgabe brauchen. Aufgaben, die diese Dienste übernehmen sind zum Beispiel:

- Wenn ich meinen Kindle per USB verbinde, automatisch aus der SQlite-Datenbank des Gerätes neue Englischvokabeln, die ich mir zum Lernen markiert habe, zum Import in meinen Vokabeltrainer auslesen.

- Über die API von meinem Todomanagerdienst bestimmte Projekte überwachen, und den Inhalt von neuen Einträgen in diesen Projekten in einer bestimmten Suchmaschine im Browser öffnen. Damit kann ich mir zum Beispiel unterwegs neue Englischvokabeln notieren und diese direkt nachschlagen wenn ich wieder am Computer bin.

- Meine Firewallregeln überwachen und mich mit einem Alarmton benachrichtigen, falls sie nicht mehr korrekt eingestellt sein sollten.

- Die Routinen meiner smarten Lampen täglich um ein paar Minuten verschieben, um eine langsame Umstellung auf neue Schlafenszeiten zu ermöglichen.

- Jeden Tag eine PDF-Datei ausdrucken, damit die Düsen meines Druckers nicht austrocknen.

### Verwendete Technologien

Python und verschiedene Python-libraries

[GitHub Repo](https://github.com/Robin-Haupt-1/Daemons-for-various-jobs)

## Herunterladen von YouTube-Videos für den Import in Obsidian (privates Projekt)

Das Problem: Ich möchte oft Notizen zu einem YouTube-Video verfassen. Dabei ist es hilfreich, das Transkript des Videos zur Hand zu haben, und es empfiehlt sich, das Video herunterzuladen, falls es irgendwann gelöscht wird. Das alles von Hand zu machen ist viel Arbeit.

Die Lösung:

Mein Skript lädt das Video herunter und generiert eine Markdown-Datei, die alle wichtigen Informationen wie Beschreibung, Uploaddatum und den Link zu der Videodatei enthält. Diese Datei kann dann in Obsidian betrachtet und bearbeitet werden. Sie enthält außerdem, soweit vorhanden, das automatisch von YouTube generierte oder manuell erstellte Transkript, das zur besseren Übersichtlichkeit in 30-Sekunden-Blöcke aufgeteilt wird.


### Verwendete Technologien
Python. Für das Herunterladen der Videos und des Transkripts benutze ich die Python-Libraries `pytube` und `youtube_transcript_api`. Nach dem Herunterladen werden die Audio- und Videospur noch mit `ffmpeg` kombiniert.


[GitHub Repo](https://github.com/Robin-Haupt-1/Download-YouTube-Videos-into-Obsidian)



# Fußnoten / Links

[^mocdownloads]: [Offizielle Downloadstatistik der Obsidianerweiterung "Map of Content"](https://github.com/obsidianmd/obsidian-releases/blob/b1c88521a25f026308b1bf8fb50682d9e1a51a92/community-plugin-stats.json#L6199)

[^autosyncdownloadpage]: [Downloadseite der Ankierweiterung "Auto Sync"](http://ankiweb.net/shared/info/501542723)
