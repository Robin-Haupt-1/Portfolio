# Portfolio

## Visualizing hierarchical relationships between notes in the knowledge management tool Obsidian.md

The challenge: Obsidian is built around the concept that notes shouldn't be sorted into folders but primarily linked among each other to establish order and context. This allows for more flexibility than a predefined, strictly hierarchical scaffolding, but it makes it harder to keep an overview of large collections of notes since all you can ever see is the very next layer of links, without much of an indication as to the larger context in which the note is relevant.

The solution: My plugin is built to support the approach of connecting all notes to a single central note through a decentralized network of notes that are increasingly abstract, in effect serving as hierarchical categories and subcategories realized as notes instead of folders.

It will analyze all the users notes and find the shortest path from the central note to every other note, which is assumed to best represent its greater context. This path will be displayed to the user. If there are several shortest paths, they will all be displayed, allowing a certain ambiguity that reflects the way information often doesn't strictly belongs only to a single context. The notes that are hierarchically below the currently open one will be displayed as a tree view.

This is what it looks like using my some notes from own collection as an example:

![MOC example view](https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20view%20with%20technology%20tree.png)


This added contextual information allows navigating and sorting large collections of thoughts and information with greater ease, while preserving all the flexibility of the link-first philosophy of organizing notes, because the hierarchical location of thousands of notes can be altered by changing just a single link.

The view allows users to click on notes' names to open them. It also has buttons to increase or decrease the depths of descendants shown in the tree, to reanalyze the notes' links after changes and to pin the view to the currently open note while browsing through others.

I have published this plugin as free and open source software. It has been downloaded 10.000 times[^mocdownloads] and has received almost 100 stars on GitHub. I have released 17 updates, fixing issues reported and adding features requested by the community [on GitHub](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content/issues?q=+is%3Aissue+-author%3ARobin-Haupt-1+) and in the [official Obsidian forum](https://forum.obsidian.md/t/map-of-content-plugin-release/25209/).


### Options dialog

The options dialog allows users to filter the notes to be analyzed and displayed in the tree by their name or path and to customize some other plugin behaviour.

![MOC settings](https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20settings.png)

### Technologies used

The backend is written in TypeScript, and the frontend has been created using web technologies (HTML, CSS, Svelte)

[GitHub Repo](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content)

## Importing english vocabulary terms from online dictionaries into the spaced repetition software Anki

The challenge: To improve my grasp of the english language, I enter terms I am not familiar with into my flashcard trainer whenever I encounter them. I obtain the german translations from the crowdsourced dictionary dict.cc and the pronunciation audio files from the online Cambridge Dictionary. Downloading all this data and creating the cards manually would be prohibitively work intensive.

The solution:

I have created an extension for Anki, the tool I use to review my flashcards, that automatically loads the terms that are to be turned into cards from dict.cc, downloads the pronunciation files and creates the cards for me. Prior to creating the cards, it has to show the new terms to me so I can make adjustments where necessary. Different german translations for the same english word will be combined into one card. On the left side, these future groupings are displayed using lines of alternating characters, immediately reacting to changes I make to the terms.

![Edit words dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20words%20dialog.png)

Then the english terms will be displayed again so I can try to find the ideal search phrase to get the pronunciation files. As I change the search phrases, the phonetic spelling of the pronunciation found will be displayed, if any, next to a number indicating how common the phrase is in everyday usage. Assuming that rarer terms will be harder to learn, the finished cards will be sorted into different decks with different review settings based on this number.

![Edit scrubbing output dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20scrubbing%20output%20dialog.png)

After this final step, the audio files will be downloaded and the cards created.

### Technologies used

I used Python, and created the GUI using PyQt5. For querying the online dictionary I use their REST-API or scrape their website using BeautifulSoup.

[GitHub Repo](https://github.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki)


## Automatische Synchronisation für Anki


The problem:
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

## Verwendete Technologien

Python. Die Verbindung zur MySQL-Datenbank wird über die Bibliothek `mysql-connector-python` hergestellt, und als Webserver nutze ich `Flask`.

[GitHub Repo](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis)


# Fußnoten / Links

[^mocdownloads]: [Offizielle Downloadstatistik der Obsidianerweiterung "Map of Content"](https://github.com/obsidianmd/obsidian-releases/blob/b1c88521a25f026308b1bf8fb50682d9e1a51a92/community-plugin-stats.json#L6199)

[^autosyncdownloadpage]: [Downloadseite der Ankierweiterung "Auto Sync"](http://ankiweb.net/shared/info/501542723)
