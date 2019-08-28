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

1. Save each graph's JSON definition to `_kg/graphs`, e.g. as `myGraph.json`.

    You can choose instead to store graph definitions in `_data/kg` as either JSON or YAML files. To change to `_data/kg`, change the `kg-graph-location` value in `_includes/kg` to just `_data` instead of `_kg`.

2. Include the graph in a markdown file with:

   ~~~ liquid
   {% include kg graph="myGraph.json" %}
   ~~~

3. In PDF and epub[^1] outputs, the graph will not work. Instead, you can show a box with a link to an online version. Specify this link by adding a `link` attribute to the include tag:

   ~~~ liquid
   {% include kg graph="myGraph.json" link="http://example.com" %}
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

5. To change the appearance of this box in non-interactive formats, edit the styles in `kg-non-interactive-styles` in the `kg` include file.

[^1]: You could make these graphs work in EPUB by heavily modifying the way you load the KG script. Contact [Electric Book Works](https://electricbookworks.com) if you need help doing this. Since many ereaders don't support Javascript well, we assume this is usually not feasible anyway.


## Developer notes

1. On where to store KG graph definitions. I think saving graphs in `_kg/graphs` is more intuitive than in `_data/kg`, because that keeps all things KG together. But then users must use JSON, because KG can't read YAML directly at this point, and Jekyll can't jsonify data that isn't in `_data`. If being able to use YAML is more important than keeping everything KG together in `_kg`, then there is an option in the include for that, as mentioned above.
2. Currently, the KG script does not add width and height attributes to its SVGs in Firefox and Edge. As a result, the SVG's height can default to a common 150px-high browser default, and the graph (which extends beyond the SVG viewBox) then disappears behind any content that follows it. As a workaround, our `kg.scss` styles add a default amount of `padding-bottom` to the SVG. As soon as the KG script can have width and height attributes in its SVGs, we should remove this `padding-bottom` workaround.
