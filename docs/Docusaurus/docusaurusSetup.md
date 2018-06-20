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



## Markdown

You will use standard markdown to write your docs, but there are few things you can do that will make the workflow a bit smoother.

### Markdown Docs Header

The header of the markdown docs in the /docs/ directory is YAML front matter.  Start with three hyphens followed by id, title, and sidebar_label

```yaml
---
id: filename-works
title: Title to See
sidebar_label: Link in Sidebar
---
```

The `id`must be unique across all documents within a folder.

`title` will be made the "h1" title for the document.

`sidebar_label` is what will show on the sidebar for the document.

### Comments

Since the `title`replaces the "h1" title I would normally use, I insert a comments indicating the header title.  This is probably not needed, but what the hell.

```markdown
---

[Optional Header]: # "Comment"
```

Notice, that I use the `---` to put an underline under the title that docusaurus generates, then a new line, then the comment.

### Images

Gotta have images in our docs.  

I have created an **/docs/assets** folder in the docs folder.  This is where all the images are.  You could create subdirectories in here to match the directory structure for your markdown, but I have opted to just use the image name to differentiate.

### Downloadable Files

There are times when you want a file to be downloaded, either a PDF, Excel, MP3, etc.  You can achieve this in two ways.

The first uses standard markdown formatting:

```markdown
[BI Schema Spreadsheet](../assets/downloads/AdBase_Schema_v2016-1(273).xlsx)
```

The downside with the above, is that for certain types, like PDF or mp3's, it will simply try and open the file.  The user can right click and choose "File Save as...", but if you want to force a download, you can inject standard HTML into your markdown doc:

```html
<a href="../assets/downloads/AdBase_Schema_v2016-1(273).xlsx" download>Click to Download</a> 
```

The other question you should be asking is "where do my download live?"

From what I can see in docusaurus, you will need to keep any type of asset in the assets folder that is in (i.e. you create in) the docs folder.

---

## Where Is My Stuff

You need to know where the files are that create all the different pages.  This section will focus on those files.

### Where Are My Docs

The markdown files that represent your documentation are located in the **docs** directory, which is at the same level as the **website** directory.

You can also create an **assets** or other named directory to place images an other information your markdown documents may need.  ~~However, you cannot try to organize your markdown documents using directories.  Looks like docusaurus doesn't want the markdown files in directories.~~

In docusaurus v1.2.1, you can have sub-directories within the docs folders for documents!  

Referencing the documents in the subdirectories is as easy as adding the directory.

For example if your structure is:

docs/
├── sub-dir1/
│   ├── file1.md
│   └── file2.md
├── readme.md
└── doc2.md

You would access the files in your `index.js` as follows:

```javascript
docUrl('readme.md'); //-> docs/readme.md
docUrl('sub-dir1/file1.md') //-> docs/sub-dir1/file1.md
```

Your `sidebars.json` will access the files similarly.

It is a good idea to come up with a standard for naming your docs also.  I like hyphens between words.

### Images

All images are in the `/assets` folder under the `/docs` directory.  I believe the folder needs to be called **assets**, but not sure.

I believe you could also have multiple directories in this folder to organize your assets.

---

## siteConfig.js

Located in the **website** directory, the siteConfig.js lets you define a number of defaults for you site.

It is here you will configure the following:

- Your websites title and tagline
- The website URL and BaseURL
- The header links
- The header and footer icons as well as the favicon
- Primary colors for the website
- Copyright information
- Other stuff...

A new feature can be enabled by adding the following to the siteConfig `onPageNav: 'separate', ` this will enable a right side menu with navigation for your document.

---

## sidebars.json

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
    "First Category": ["subdir/doc2"],
    "Second Category": ["subdir/doc3"]
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
    "Javascript": ["asyn-await","promises", "es6", "..."],
    "React": ["react-basics", "react-components"],
    "ReactNative": ["reactnative-basics"],
    "Node": ["modebasics"],
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

---

## Site Setup

### Main Page - Index.js

So where do you set up the main page of the site?

This is in the **index.js** file located in */webiste/pages/en*.

This is a react file with a lot of defaults.  If you scroll to the end of the file, you will see the exported component:

```javascript
class Index extends React.Component {
  render() {
    let language = this.props.language || '';

    return (
      <div>
        <HomeSplash language={language} />
        <div className="mainContainer">
        {/*
            <Features />
            <FeatureCallout />
          <LearnHow />
          <TryOut />
          <Description />*/
          }
          <Resources />
        </div>
      </div>
    );
  }
}
```

This "Index" component uses a number of other components built within the **index.js** page.

The **HomeSplash** component can be customized to show your basic links.

### Active Link CSS

You can put custom CSS to style the active document sidebar link.

This will be added to the `customer.css` file located in `/website/static/css/`

```css
.navItemActive {
    background: lightyellow;
    border: 1px dashed lightblue !important;
    font-weight: bold;
    padding-left: 10px !important;
}
```

