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

The challenge: To improve my understanding of the english language, I enter terms I am not familiar with into my flashcard trainer whenever I encounter them. I obtain the german translations from the crowdsourced dictionary dict.cc and audio files of the corrent pronunciation from the Cambridge Dictionary website. Downloading all this data and creating the cards manually would be prohibitively time intensive.

The solution:

I have created an extension for Anki that automatically loads the terms that are to be turned into cards from dict.cc, where I save them, then downloads the pronunciation files and generates the cards from them. First, it has to show me the new terms so I can adjust them where necessary. Different german translations for the same english word will be combined into one card. On the left, these future groupings are visualised using lines of alternating characters, immediately reaction to alterations I make to the terms.

![Edit words dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20words%20dialog.png)

Next, the english terms will be displayed again so I can find the ideal search phrase to obtain the pronunciation files. As I change the search phrases, the phonetic spelling of the pronunciation found will be displayed, if any, next to a number indicating how common the phrase is in everyday usage. Assuming that rarer terms will be harder to learn, the finished cards will be sorted into different decks with different review settings based on this rank.

![Edit scrubbing output dialog](https://raw.githubusercontent.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki/main/doc/2021%20edit%20scrubbing%20output%20dialog.png)

After this second editing, the audio files will be downloaded and the cards created.

### Technologies used

I used Python, and created the GUI using PyQt5. For querying the online dictionaries I use their REST-API or scrape their website using BeautifulSoup.

[GitHub Repo](https://github.com/Robin-Haupt-1/Dict.cc-and-Cambridge-Dictionary-to-Anki)


## Automatic synchronization for Anki

The problem: The flashcard trainer Anki can be used on mobile or desktop and can be synchronized via the cloud. This process must be initiated manually though, which is tiresome if you use it a lot throughout the day. One can even get into a habit of reflexively pressing the button to start the sync process, regardless of whether there actually is something to sync or not.

The solution:

I have created an extension to automatically initiate synchronization after the user has been idle for a few minutes, waiting for inactivity so as not to be disruptive. User activity is detected by monitoring keyboard and mouse events and the programs state. If cards are being reviewed or edited, synchronization will not be initiated even after a period of inactivity. This behaviour can be regulated in the options dialog:

![auto sync options](https://raw.githubusercontent.com/Robin-Haupt-1/Auto-Sync-Anki-Addon/main/doc/2022-04-25%20options%20dialog.png)

A log window allows monitoring and troubleshooting the tools activity:

![auto sync log window](https://raw.githubusercontent.com/Robin-Haupt-1/Auto-Sync-Anki-Addon/main/doc/2022-04-25%20log%20window.png)

I have made this extension publicly available as free and open source software. It has been downloaded 650 times and has received overwhelmingly positive reviews. [^autosyncdownloadpage]

### Technologies used

Python für das Backend und PyQt5 für die GUI-Elemente

[GitHub Repo](https://github.com/Robin-Haupt-1/Auto-Sync-Anki-Addon)


## Informationen über Lebensmittelkäufe über die Kommandozeile eingeben und in einer relationalen Datenbank speichern

The problem: I want to store information on the food items I purchase to better be able to judge the quality of my diet and track changes over time.
Manually entering this data into an Excel file is time consuming and it would be challenging to always enter it in a way thats optimized for statistical analysis.

The solution:

I have created a command line tool that allows me to quickly enter data on a new purchase by offering past stores and products to select from. This also prevents duplicate entries for the same item from coming into existence.

My tool differentiates between abstract product types and concrete product items to allow me to store as much detail as possible (brand, store, package size) of each item purchased without introducing confusion about which items are really the same commodity sold under a different label, so that they can later be analyzed as one. To this end, every concrete product item must be associated with a abstract (generic) product type.

After gathering information on the purchase, it sends it to a web servers REST-API endpoints, which then stores it in the database. This follows a microservice approach, in that the CLI and database read/write operations take place in seperate processes.

The web server does not know what kind of database it is accessing, since it works with a generic interface that the MySQL-based database implementation fulfills (adhering to the principle of Dependency Inversion). This will allow me have the tool use a different type of database in the future without having to change the server code.


### Screenshot: Using the application to store information on a new purchase

![FPT enter purchase](https://raw.githubusercontent.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/main/doc/cli-screenshot-m%C3%BCsli.png)

### Video: Inputting details of a new product item

https://user-images.githubusercontent.com/85873542/166096132-24622d5b-f95c-44ec-b41b-d95c5311ba8a.mp4

(Video won't play? [Click here](https://raw.githubusercontent.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/main/doc/2022-04-15-create-concrete-item.gif))

### MySQL tables

The details of items, stores and purchases are split up over different tables that refence each other's entries by ID:

#### Stores

![FPT stores table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-stores-table.png)

#### Abstract (generic) items

![FPT abstract items table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-abstract-items-table.png)

#### Concrete (specific) items

![FPT concrete items table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-15-concrete-items-table.png)

#### Purchase events

![FPT purchases table](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/blob/main/doc/2022-04-24-purchases_table.png)

## Verwendete Technologien

Python. I use the python library `mysql-connector-python` to connect to the MySQL database, and `Flask` as my web server.

[GitHub Repo](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis)


# Footnotes

[^mocdownloads]: [Official download statistics for Obsidian extension "Map of Content"](https://github.com/obsidianmd/obsidian-releases/blob/b1c88521a25f026308b1bf8fb50682d9e1a51a92/community-plugin-stats.json#L6199)

[^autosyncdownloadpage]: [Download page for Anki addon "Auto Sync"](http://ankiweb.net/shared/info/501542723) (Download statistics are not publicly visible)
