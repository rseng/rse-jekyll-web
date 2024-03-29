# Your Research Software Database

Congrats on deploying a custom research software database! Here are some basic instructions
for interaction.

## Config

Edit the `_config.yaml` to update metadata, and site settings.
This includes everything from the logo to social media handles and some colors. If there is an additional
style not exposed, please [open an issue](https://github.com/rseng/rse/issues).

## Index Page

The index.html in the root can be edited to include a custom welcome message to your site!
The logo defined in your config will also appear here.

## Posts

A fully populated [_posts](_posts) folder is included with an example of how to organize your posts! These will be included in the "news" portion of your site, and as you see, you can use this for news, tutorials, software review, or other blog content.

## Software Table

The software table (`_layouts/software.html`) has basic fields from the markdown files in [_software](_software) (that you will generate) hard coded in. If you look at your generated files, however, you'll notice that this set is extended to include all the data (and custom data) from the RSEPedia! So you are free to select anything from there! Right now you need to edit the JavaScript to instantiate the data table, but we could think of possibly easier ways to do this.

## Workflows

To see workflows that you might use 
