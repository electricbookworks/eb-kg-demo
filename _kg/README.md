# Electric Book KG plugin

This plugin adds support for [KG interactive graphs](https://github.com/cmakler/kgjs/) to an Electric Book project.


## Installation

1. Copy the contents of this folder to one called `_kg` in the root directory of your Electric Book project.
2. The folder is a Jekyll collection. Register it with Jekyll by adding it to `collections` in `_config.yml`:

   ~~~ yaml
   collections:
     kg:
       output: true
   ~~~

3. Save the `kg` file to your project's `_includes` folder.


## Usage

Then, to include a graph in your web and app outputs:

1. Save each graph's JSON or yaml definition to `_kg/graphs`, e.g. as `myGraph.yml`.

    You can choose instead to store graph definitions in `_data/kg`. To change to `_data/kg`, change the `kg-graph-location` value in `_includes/kg` to just `_data` instead of `_kg`.

2. Include the graph in a markdown file with:

   ~~~ liquid
   {% include kg graph="myGraph.yml" %}
   ~~~

3. In PDF and epub[^1] outputs, the graph will not work. Instead, you can show a box with a link to an online version. Specify this link by adding a `link` attribute to the include tag:

   ~~~ liquid
   {% include kg graph="myGraph.yml" link="http://example.com" %}
   ~~~

4. To change or translate the text that appears around the link (for each language in your project), add a `kg` section to each language in `_data/locales.yml`, with values for `link-prefix` (the text before the link) and `link-suffix` (the text after the link).

   ~~~ yaml
   en:
     kg:
       link-prefix: "This interactive is online at "
       link-suffix: "."
   fr:
     kg:
       link-prefix: "Cette interactive est en ligne sur "
       link-suffix: "."
   ~~~

5. To change the appearance of the box in non-interactive formats, edit the styles in `kg-non-interactive-styles` in the `kg` include file.

[^1]: You could make these graphs work in EPUB by heavily modifying the way you load the KG script. Contact [Electric Book Works](https://electricbookworks.com) if you need help doing this. Since many ereaders don't support Javascript well, we assume this is usually not feasible anyway.
