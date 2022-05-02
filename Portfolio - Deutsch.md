# Portfolio

## Darstellen hierarchischer Beziehungen von Notizen im Wissensmanagementprogramm Obsidian

Das Problem: Obsidian basiert auf der Idee, dass Notizen nicht in Ordnern sortiert, sondern durch Verknüpfungen untereinander geordnet werden sollen. Das ist viel flexibler als feste hierarchische Strukturen, aber wenn man viele Notizen hat, ist es schwer den Überblick zu behalten, da man immer nur Links der nächsten Ebene sehen kann. Man hat keinen Anhaltspunkt, in welchem Geflecht von Notizen man sich befindet oder wie genau es strukturiert ist.

Die Lösung:

Meine Erweiterung basiert auf der Idee, alle Notizen durch verschiedene, zunehmend abstraktere andere Notizen mit einem zentralen Index zu verknüpfen, also Kategorien und Unterkategorien realisiert als Notizen anstatt Ordner.

Mein Programm analysiert alle Notizen und findet die kürzesten Pfade von der zentralen Notiz zu jeder anderen Notiz, also sozusagen die Überkategorien. Diese werden automatisch als Pfad dargestellt. Wenn es mehr als einen Pfad mit dieser Länge gibt, werden alle diese kürzesten Pfade dargestellt, um ein Bild von den verschiedenen Umständen, unter denen die Notiz relevant ist zu vermitteln.
Außerdem werden die Notizen, die hierarchisch unter der geöffneten Notiz liegen, als Verwandtschaftsbaum dargestellt. Hier ist ein Beispiel der automatisch generierten Übersicht aus meiner persönlichen Sammlung:

![MOC example view](https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20view%20with%20technology%20tree.png)

Dies bringt eine große Erleichterung im Navigieren und Sortieren großer Sammlungen von Notizen. Gleichzeitig bleibt die Flexibilität der verweisbasierten Organisation erhalten, da tausende Notizen völlig neu in die Hierarchie einsortiert werden können durch das Abändern eines einzigen Links.

Die Ansicht ermöglicht es, auf die Namen der Notizen zu klicken, um sie im Editor zu öffnen. Es gibt Buttons um die Verknüpfungen auf mehr oder weniger Ebenen der Hierarchie anzuzeigen oder den Verwandtschaftsbaum einer Notiz offen zu halten während man durch andere Notizen navigiert.

Ich habe diese Erweiterung als Open Source veröffentlicht. Seitdem wurde sie 10.000 mal heruntergeladen [^mocdownloads] und hat 17 Updates erhalten, in denen ich Ideen und Bugfixes realisiert habe, die von den Nutzern [auf GitHub](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content/issues?q=+is%3Aissue+-author%3ARobin-Haupt-1+) und [im offiziellen Obsidianforum](https://forum.obsidian.md/t/map-of-content-plugin-release/25209/) eingebracht wurden. Sie hat auf GitHub schon fast 100 Sterne erhalten.


### Einstellungen

In dem Einstellungsdialog können die zu beachtenden Dateien anhand ihres Namens oder Pfades gefiltert und einige andere Verhaltensweisen personalisiert werden.

![MOC settings](https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20settings.png)

### Verwendete Technologien

Das Backend ist in TypeScript geschrieben, und die Frontend-Elemente wurden mit Webtechnologien (HTML, CSS, Svelte) erstellt.

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

Python für das Backend und PyQt5 für die GUI-Elemente. Für das Abfragen der Onlinewörterbücher nutze ich deren REST-API sofern vorhanden oder scrape das HTML mit BeautifulSoup.

[GitHub Repo](https://github.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki)


## Automatische Synchronisation für Anki

Das Problem:  Der Vokabeltrainer Anki kann mobil und auf dem Computer genutzt werden, und über die Cloud synchronisiert werden. Das muss aber von Hand ausgelöst werden. Wenn man das Programm über den Tag verteilt viel benutzt, kann das anstrengend werden, und sogar zu einem unangenehmen Phänomen führen, bei dem man aus Gewohnheit immer wieder auf den Button klickt, selbst wenn es gar nichts zu synchronisieren gibt.

Die Lösung:

Ich habe eine Erweiterung geschrieben, die die Synchronisation automatisch auslöst, wenn der Nutzer das Programm ein paar Minuten nicht benutzt hat. Die Inaktivität wird festgestellt, indem Maus- und Tastaturevents überwacht werden. Außerdem wird sichergestellt, dass keine Karten wiederholt oder bearbeitet werden. All das kann in einem Einstellungsdialog geregelt werden:

![auto sync options](https://raw.githubusercontent.com/Robin-Haupt-1/Auto-Sync-Anki-Addon/main/doc/2022-04-25%20options%20dialog.png)

Ein Logfenster ermöglicht es, die Aktivität der Erweiterung zu überwachen:

![auto sync log window](https://raw.githubusercontent.com/Robin-Haupt-1/Auto-Sync-Anki-Addon/main/doc/2022-04-25%20log%20window.png)

Ich habe die Erweiterung als Open Source veröffentlicht. Sie wurde bis jetzt 650 mal heruntergeladen und hat überwiegend positive Bewertungen erhalten. [^autosyncdownloadpage]

### Verwendete Technologien

Python für das Backend und PyQt5 für die GUI-Elemente

[GitHub Repo](https://github.com/Robin-Haupt-1/Auto-Sync-Anki-Addon)


## Informationen über Lebensmittelkäufe über die Kommandozeile eingeben und in einer relationalen Datenbank speichern

Das Problem: Ich möchte Informationen über das Essen, das ich kaufe, speichern, um besser beurteilen zu können wie ausgewogen meine Ernährung ist und um längerfristige Entwicklungen nachvollziehen zu können.

Die Informationen von Hand in eine Exceltabelle o.ä. zu übertragen würde viel Zeit kosten, und die Auflistung wäre in diesem Format statistisch nicht unbedingt gut auszuwerten.

Die Lösung:

Meine Kommandozeilenapplikation ließt die Informationen über vergangene Einkäufe aus einer Datenbank und schlägt sie bei der Eingabe der neuen Einkäufe vor. Dadurch wird der Zeitaufwand für das Übertragen der Kassenbons stark gesengt, und es enstehen keine doppelten Einträge für den gleichen Artikel, die später bei der statistischen Auswertung wieder vereint werden müssten.
Auch wird zwischen abstrakteren Produktarten und konkreten Artikeln unterschieden, wobei jeder konkrete Artikel einem abstrakten Produkttyp zugeordnet sein muss. So kann ein Warentyp im Gesamten analysiert werden, unabhängig davon, in welchem Geschäft er unter welcher Marke in welcher Packungsgröße erworben wurde.

Die in der Kommandozeile eingegebenen Informationen werden über REST-API-Endpunkte an einen Server übertragen, der sie in eine Datenbank schreibt. Diese Segregation der Aufgabenbereiche folgt einem Microserviceansatz.
In meiner Implementierung ist die MySQL-Datenbank beliebig mit einer anderen Art von Datenbank außtauschbar, da der Server unter Beachtung des Dependency-Inversion-Prinzips mit einem unspezifischen Interface arbeitet, das durch die MySQL-Schnittstelle oder eine andere Datenbankschnittstelle erfüllt werden kann.

### Screenshot vom Gebrauch der Applikation zum Speichern eines gekauften Artikels

![FPT enter purchase](https://raw.githubusercontent.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/main/doc/cli-screenshot-m%C3%BCsli.png)

### Video vom Einpflegen der Daten eines neuen Produkts

https://user-images.githubusercontent.com/85873542/166096132-24622d5b-f95c-44ec-b41b-d95c5311ba8a.mp4

(Video geht nicht? [Hier](https://raw.githubusercontent.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/main/doc/2022-04-15-create-concrete-item.gif) klicken)

### MySQL-Tabellen

Die Informationen über Geschäfte, Produkte und Einkaufshistorie werden in verschiedenen Tabellen gespeichert, die auf die Einträge anderer Tabellen mittels deren ID verweisen.

### Geschäfte

![FPT stores table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-stores-table.png)

### Abstrakte Produkttypen

![FPT abstract items table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-abstract-items-table.png)

### Konkrete Artikel

![FPT concrete items table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-concrete-items-table.png)

### Einkaufshistorie

![FPT purchases table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-24-purchases_table.png)

## Verwendete Technologien

Python. Die Verbindung zur MySQL-Datenbank wird über die Bibliothek `mysql-connector-python` hergestellt, und als Webserver nutze ich `Flask`.

[GitHub Repo](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis)


# Fußnoten / Links

[^mocdownloads]: [Offizielle Downloadstatistik der Obsidianerweiterung "Map of Content"](https://github.com/obsidianmd/obsidian-releases/blob/b1c88521a25f026308b1bf8fb50682d9e1a51a92/community-plugin-stats.json#L6199)

[^autosyncdownloadpage]: [Downloadseite der Ankierweiterung "Auto Sync"](http://ankiweb.net/shared/info/501542723) (Die Downloadstatistik ist nicht öffentlich einsehbar)
