# Research Software Encyclopedia Jekyll Web

This is an example of a database generated via a [Google Sheet Import](https://rseng.github.io/rse/getting-started/commands/#import)
using the Research Software Encyclopedia, exporting to a Jekyll Web interface in [docs](docs),
and then including workflows that will automatically update both. Instructions are included
here for doing the same thing on your own.

## Setup

There are many ways to create a database that you can read about in the [documentation](https://rseng.github.io/rse/getting-started/commands)
but for this example we will focus on the Import -> Google Sheet method. We wil be using this [example template sheet](https://docs.google.com/spreadsheets/d/1ZW2kOsBOfSpRSH_9Efvz-ytn7dQ2m1DmYDBdIVNGY4c/edit?usp=sharing) that we have exported as csv to [this path](https://docs.google.com/spreadsheets/d/e/2PACX-1vTsPmEWUg8Tr1ZoYTcQ0kTdsCrVskQveSuwfdEHaktHtQG693O4DHQrZotoFd5dXCLAciykAYNf-RSz/pub?gid=0&single=true&output=csv).  To do this for any Google Sheet you can do:

```
Share -> Publish to Web -> Form Responses 1 (or the sheet name in first dropdown) -> Comma-separated value (csv) (second dropdown)
```
And the Research Software Encyclopedia requires these three fields (capitalization is not important):

 - Title: A human-friendly title to describe the software. If the Url doesn't have a version control address, this will be parsed for a unique identifier.
 - Url: A link to GitHub, GitLab, or another online resource (this will be parsed looking for a unique identifier)
 - Description: The description of the software project
 
All other fields will be treated as custom data fields to add to the data. Also note that the first row needs to be the header
with field names.

### Create the database
 
We can use `rse init` to create an empty filesystem database (and call the directory whatever you like): 

```bash
$ mkdir rse-jekyll-web
$ cd rse-jekyll-web
$ rse init .
INFO:rse.main.config:Generating configuration file rse.ini
```

And then running the import comes down to this command:

```bash
$ rse import --type google-sheet "https://docs.google.com/spreadsheets/d/e/2PACX-1vTsPmEWUg8Tr1ZoYTcQ0kTdsCrVskQveSuwfdEHaktHtQG693O4DHQrZotoFd5dXCLAciykAYNf-RSz/pub?gid=0&single=true&output=csv"
```

**Important** The sheet URL is provided in quotes.

This will create the database structure with a combination of custom and version control (e.g., GitHub) identifiers. The identifier is derived from the title, so add any organizational structure that you might desire there. You an also export `RSE_CUSTOM_DATABASE_DIR` via the environment (e.g., `export RSE_CUSTOM_DATABASE_DIR=research`) to save that prefix. The resulting structure will look like the following:

```bash
database/
├── custom
│   ├── adobe-audition
│   │   └── metadata.json
│   ├── anabat-insight
│   │   └── metadata.json
│   ├── animal-sound-identifier
│   │   └── metadata.json
│   ├── artwarp
│   │   └── metadata.json
│   └── audacity
│       └── metadata.json
└── github
    ├── ChristianBergler
    │   └── ANIMAL-SPOT
    │       └── metadata.json
    ├── nwolek
    │   └── audiomoth-scripts
    │       └── metadata.json
    └── patriceguyot
        └── Acoustic_Indices
            └── metadata.json
```

And if you just want to do a dry run, add `--dry-run` to the above. At this point, we have our database! Note that the import will not update records - this is a separate functionality that looks like this:

```bash
$ rse import --type google-sheet --update "https://docs.google.com/spreadsheets/d/e/2PACX-1vTsPmEWUg8Tr1ZoYTcQ0kTdsCrVskQveSuwfdEHaktHtQG693O4DHQrZotoFd5dXCLAciykAYNf-RSz/pub?gid=0&single=true&output=csv"
```

### Create the Web

The first time you create the web UI, it will populate the docs folder completely.

```bash
$ rse export --type jekyll-web docs/
```

This will create the [docs](docs) folder and you'll find your software in [docs/_software](docs/_software).
This means you'll want to:

1. Edit the _config.yml with your repository URL and other metadata
2. Since we want to use our custom "tags" column from the Google Sheet, we can edit [docs/pages/data.json](docs/pages/data.json) to customize the API data to use "tags" instead of "topics." Note that if you customize the table you'll need to edit [_layouts/software.html](_layouts/software.html).
3. When you push to GitHub, enable GitHub pages to render from "docs"
4. You should check the README.md in the docs folder to see details for customization.

### Automation

We include an example [GitHub workflow](.github/workflows/update.yaml) that will retrieve data from
a Google Sheet (as we did above) and then update the database and site. We install the RSEpedia from the master
branch to allow for testing new features. You can use and edit this workflow to your liking, and please open an issue
if you'd like to ask for help.

Do you have any questions? please [open an issue](https://github.com/rseng/rse-jekyll-web/issues) to let us know!
