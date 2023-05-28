#documentation #Obsidian #github 

**Table of contents**
- [Summary](#Summary)
- [Best Practices](#Best%20Practices)
- [Common Categories](#Common%20Categories)
	- [Knowledge Base](#Knowledge%20Base)
	- [Planning](#Planning)
	- [Templates](#Templates)
- [Common Features](#Common%20Features)
	- [Document Status](#Document%20Status)
	- [Tags at the top](#Tags%20at%20the%20top)
	- [Table of Contents](#Table%20of%20Contents)
	- [Best Viewed in Obsidian](#Best%20Viewed%20in%20Obsidian)


# Summary
All the documentation, planning, guides and notes for the project are found here. The documentation may be viewed online via [Github](https://github.com/ScottGarryFoster/PROJECT-Snake/blob/main/SnakeGameDocumentation) the better method however is to install [Github desktop](https://desktop.github.com/), [Obsidian](https://obsidian.md/) and open this as a Vault see instructions here.

The documentation is split up into the phases of game production. The phases may occur in parallel to one another. All phases contain planning, knowledge bases and templates for ease of use. Other headings may be included which are subject specific.

The goal of documentation is to ensure decisions are made with reason and the reasoning is not lost or forgotten. The documentation contained here is in tandem with the [Github Planning features](https://github.com/features/issues) which for this project are found [here](https://github.com/ScottGarryFoster/PROJECT-Snake/projects?query=is%3Aopen).

---
# Best Practices
To keep the documentation organised and under control there are best practices which we all *should keep in mind* to ensure the documentation is kept clean, clear and maintainable.

1. When adding files, create a folder called **LinkedFiles** near to where the files are used. Prefix the files with the location and a dash between that and a descriptive name. For instance: *Documentation-PlantUMLImage* this ensures duplicates do not occur. References are global within Obsidian and it is easier to control with descriptive names.
2. Add a Summary to every page. This ensures there is an easy point at which everyone may figure out whether the page is correct or not for their query. Keep in mind not everyone will find a page via the same link, directory tree or search tag so it is key to describe what the page's purpose is.
3. Good organisation. Moving files around may break links and in general makes it harder to find information. Finding a suitable location straight away is much better than doing so later on.
4. Markdown links when linking between pages. Markdown links will not work for [Github](https://github.com/) and [Obsidian](https://obsidian.md/) therefore it is best to use the internal Markdown links. These display as the name of the page on [Github](https://github.com/) and a link on [Obsidian](https://obsidian.md/). An example is displayed below.
	* This is an internal link: [[1. Documentation/Overview]]. It is displayed as "Overview" on both platforms. It is created in this format:  `[[Overview]]`
5. Updating and reviewing pages. Documentation is only as useful as to how up to date it is. This is especially true for the knowledge base. If exceptions begin to arise within pages of the knowledge base it is worth considering a re-write. If there are new tools or software, it is worth writing new pages for these workflows. Capturing up to date information will help everyone.

---
# Common Categories
There are a few common categories which are repeated in almost every phase / department of production. This section is included to explain their role.

## Knowledge Base
Explanations of a given workflow, library or tutorial are found in here. Support entries for both internal and external tools may be found here.

## Planning
Design documents, forecasts and general planning documents.

## Templates
Notes which may be copied and have notes where generic information should be replaced for specific information. These should be obvious or marked with a markdown tag 'quote' > such as the below:
>Replace this with something else.

---
# Common Features
There are some common features of documentation to look out for.

## Document Status
At the top of most pages is a *status box* which contains the owner, state and last time the page was edited. This should be filled out and edited each time there is an edit on the page. Below is an example of the template and below that an example of this filled out.

**The date format must always be: Date Month Year DD(rd/th/st) MMM YYYY**

|Owner|State|Last_update|
|--|--|--|
|USERNAME|Draft, Development, Review, Signed off|Date|

|Owner|State|Last_update|
|--|--|--|
|@ScottGarryFoster|Review|27th May 2023|

## Tags at the top
[Obsidian](https://obsidian.md/) allows for searching via Tags. Documentation for this may be found [here](https://help.obsidian.md/Editing+and+formatting/Tags). All pages must be tagged and tags are found at the top of the page.
`#myTag`

## Table of Contents
Most pages will contain a table of contents. This is a [Markdown](https://www.markdownguide.org/cheat-sheet/) set of links to headings in the page. It is created using an extension within [Obsidian](https://obsidian.md/). Instructions for how to use / insert these may be found at [[Obsidian]]#Table of Contents. 

## Best Viewed in Obsidian
Some features are best viewed in [Obsidian](https://obsidian.md/), in particular [PlantUML](https://plantuml.com/) and social media links will display in more visual ways within the [Obsidian](https://obsidian.md/) editor. This allows one to view this information more freely than via [Github](https://github.com/) online.

Some examples:

|Online|Obsidian|
|---|---|
| ![[Documentation-PlantUMLText.png]] | ![[Documentation-PlantUMLImage.png]]|
|`![](https://www.youtube.com/watch?v=G-mkiWlkE3U)`| ![[Documentation-YoutubeLink.png]] |
