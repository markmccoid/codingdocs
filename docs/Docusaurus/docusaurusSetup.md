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

## Where Is My Stuff

You need to know where the files are that create all the different pages.  This section will focus on those files.

### Where Are My Docs

The markdown files that represent your documentation are located in the **docs** directory, which is at the same level as the **website** directory.

You can also create an **assets** or other named directory to place images an other information your markdown documents may need.  However, you cannot try to organize your markdown documents using directories.  Looks like docusaurus doesn't want the markdown files in directories.

### siteConfig.js

Located in the **website** directory, the siteConfig.js lets you define a number of defaults for you site.

It is here you will configure the following:

- Your websites title and tagline
- The website URL and BaseURL
- The header links
- The header and footer icons as well as the favicon
- Primary colors for the website
- Copyright information
- Other stuff...

### sidebars.json

This guy is also in the **website** directory.  the sidebars.json file defines the you sidebar structure and what docs appear in them.

The final output looks like:

![](https://dl.dropboxusercontent.com/s/lpuxhpa2bmkgqfi/DOCUSAURUS_sidebar1.png)

The json looks like this:

```json
{
  "docs": {
    "Overview": ["Overview","analytix-overview"],
    "Analytix Setup": ["analytixsetup", "analytixadministratorsguide"],
    "BI": ["bi-auditors"],
    "First Category": ["doc2"],
    "Second Category": ["doc3"]
  },
  "docs-other": {
    "Other Category": ["doc4", "doc5"]
  }
}

```

The ***docs*** and ***docs-other*** properties are not shown anywhere in the site, but are used to hold the different groups of documentation.

So in the screenshot above you are seeing all the items pertaining to the **"docs"** group.  If you were to link to any document listed in the **"docs-other"** group you would get a page that had the "docs-other" sidebar info on it.

The object properties under each of the groups makes up the sidebars headers (Overview, Analytix Setup, etc...) and the **array** under each of these properties are the docs that fall under that header.

Next, how do you access these docs from places other than the sidebar? You will have to access them via a link at some point to get to the page with the sidebar.  This is done in different ways depending on where you are.



**EXAMPLE**:  If you have a site with lots of documentation and it is only slightly related, you may want to create many groups.  Thinking of my Code Docs that I have setup.  I might have a structure as follows:

```json
{
  "Programming": {
    "Javascript": ["Overview","analytix-overview"],
    "React": ["analytixsetup", "analytixadministratorsguide"],
    "ReactNative": ["bi-auditors"],
    "Node": ["doc2"],
    "...": ["doc3"]
  },
  "Resources": {
    "JS Libraries": ["doc4", "doc5"],
    "Git": []
  },
  "My Projects": {
	"TMDB": ["API-docs", "DatabaseLayout", "..."]
  }
}

```

