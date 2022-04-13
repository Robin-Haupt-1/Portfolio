# Portfolio

# Extension for notekeeping app Obsidian.md

The extension helps users with ordering and linking their notes, keeping an overview of their entire collection and finding notes relevant to the one they're currently working on. It has been downloaded 8800 times[^mocdownloads] and has almost 100 stars on GitHub.
I have released 17 versions, fixing issues reported and adding features requested by the community [on GitHub](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content/issues?q=+is%3Aissue+-author%3ARobin-Haupt-1+) and in the [Obsidian forum post announcing the plugins release.](https://forum.obsidian.md/t/map-of-content-plugin-release/25209/4)

## What does it do?

### Main functionality

It analyses the links between notes and finds the shortest path to a selected note starting from a single central index note. Then it displays this path and a tree view of notes to be found downstream from it in an interactive view that allows users to click on notes' names to open them.
The view also has buttons to increase or decrease the depths of descendants shown in the tree, to reanalyze the notes' links after changes and to pin the view to the currently open note while browsing through others.

![MOC view](https://github.com/Robin-Haupt-1/Portfolio/blob/main/images/moc.png)

### Settings



## Biggest challenges

- recursive pathfinding algorithm that analyses links between thousands of notes in a few hundred milliseconds
- integration with Obsidians plugin API for reading note information and controlling the application through the plugin
- creating an elegant but powerful and flexible web-based interface
- migrating settings between versions with added or changed configuration options


## Technologies used

# Food purchase tracker

I have created a command line tool to enter and store the details about what food items i buy in a way thats optimized for statistical analysis. The command line is designed to make it fast to enter a new purchase by offering past stores and products to select from. It then sends the complete purchase information to a Flask server that stores it in the database. This follows a microservice approach, in that CLI and database read/write operations are seperate processes.

The flask server does not know what kind of database it is accessing, since it works with a generic interface that the MySQL-based database implementation fulfills (Information hiding / Dependency Inversion).

In the future, i plan to write routines for outputting statistical analyses of the data, to get a better overview of my food purchasing habits and to help with making healthy and economical choices.

# Technologies used

REST API
Flask
MySQL
mysql-connector-python
Python: 682

# Approaches utilised

Object-Relational Mapping
Dependency Inversion


## [GitHub Repo](https://github.com/Robin-Haupt-1/Food-purchases-tracking-and-analysis)


# Skills

- Linux

I use Linux as my main OS and am comfortable using the command line. I have experience with Manjaro and Ubuntu.


# [GitHub Repo](https://github.com/Robin-Haupt-1/Obsidian-Map-of-Content)

# Footnotes

[^mocdownload]: [Official download statistics](https://github.com/obsidianmd/obsidian-releases/blob/6605c60a77a160e2fa0f1abe307cec308b839902/community-plugin-stats.json#L6110)
