# Creating Docs for Docusaurus.io 

Setup the initial site as recommended in the [documentation](https://docusaurus.io/docs/en/installation.html).



## Directory Structure

After the initial install, you will have an example Directory structure.  You will want to rename a few of the directories to look like this:

BaseDir/
├── docs/
│   ├── assets/ (you need to add this directory.)
│   ├── doc1.md
│   └── doc2.md
└── website/
    ├── blog
    ├── build
    ├── ...
    └── static



## Markdown Docs Header

The header of the markdown docs in the /docs/ directory is YAML front matter.  Start with three hyphens followed by id, title, and sidebar_label

```yaml
---
id: filenameworks
title: Title to See
sidebar_label: Link in Sidebar
---
```

