# Portfolio

## Extension for notekeeping app Obsidian.md

The extension helps users with ordering and linking their notes, keeping an overview of their entire collection and finding notes relevant to the one they're currently working on. It has been downloaded 9500 times[^mocdownloads] and has almost 100 stars on GitHub.
I have released 17 versions, fixing issues reported and adding features requested by the community [on GitHub](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content/issues?q=+is%3Aissue+-author%3ARobin-Haupt-1+) and in this [official Obsidian forum post announcing the plugins release.](https://forum.obsidian.md/t/map-of-content-plugin-release/25209/4)

### What does it do?

#### Main functionality

It analyses the links between notes and finds the shortest path to a selected note starting from a single central index note. Then it displays this path and a tree view of notes to be found downstream from it in an interactive view that allows users to click on notes' names to open them.
The view also has buttons to increase or decrease the depths of descendants shown in the tree, to reanalyze the notes' links after changes and to pin the view to the currently open note while browsing through others.

<img src="https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20view%20with%20technology%20tree.png" alt="MOC view"/>


#### Settings

![MOC settings](https://raw.githubusercontent.com/Robin-Haupt-1/Obsidian-Map-of-Content/main/doc/2022-04-24%20settings.png)


### Biggest challenges

- recursive pathfinding algorithm that analyses links between thousands of notes in a few hundred milliseconds
- integration with Obsidians plugin API for reading note information and controlling the application through the plugin
- creating an elegant but powerful and reactive web-based interface
- migrating settings between versions with added or changed configuration options


### Languages used

- TypeScript: 1158 Lines of code
- Svelte: 1374 Lines of code

### [GitHub Repo](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content)


## Food purchase tracker

I have created a command line tool to enter and store the details about what food items I buy in a way that's optimized for statistical analysis. The command line is designed to make it fast to enter a new purchase by offering past stores and products to select from. It then sends the complete purchase information to a Flask server that stores it in the database. This follows a microservice approach, in that the CLI and database read/write operations live in seperate processes.

The flask server does not know what kind of database it is accessing, since it works with a generic interface that the MySQL-based database implementation fulfills (implementing the principle of Information Hiding / Dependency Inversion).

In the future, I plan to write tools for outputting statistical analyses of the data, to get a better overview of my food purchasing habits and to help with making healthy and economical choices.

### Examples of command line interface usage

#### Entering data of a purchase

![FPT enter purchase](https://raw.githubusercontent.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis/main/doc/cli-screenshot-m%C3%BCsli.png)

### Entering data of a new item

https://user-images.githubusercontent.com/85873542/163574717-8024f1d0-0ad9-489e-98b9-9f32ae49e7af.mp4

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



### Technologies used

- Python: 682 lines
- Flask
- MySQL (through mysql-connector-python library)
- REST API

### Biggest challenges

- performing Object-Relational Mapping to store the data with as little redundancy as possible
- communicating the data between the database, Flask endpoints and the CLI through HTTP requests
- determining how to store as much detail as possible (brand, store, package size) of each item purchased without introducing confusion about which items are really the same commodity sold under a different label, so that they can later be analyzed as one item.

### [GitHub Repo](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis)


## Skills

## Footnotes

[^mocdownloads]: [Official download statistics for Obsidian extension "Map of Content"](https://github.com/obsidianmd/obsidian-releases/blob/b389b36debc012a93e52ae09a77be9cadbfcd050/community-plugin-stats.json#L6166)

[^autosyncreviews]:
