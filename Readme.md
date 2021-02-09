# TinyMCE 4/5 Spoiler Plugin

This is a spoiler plugin for [TinyMCE 4/5](https://www.tiny.cloud/) web content editor. It allows to insert spoiler
blocks into authored text, that is, blocks of text that are initially collapsed and can be expanded by mouse click.

## Installation

Copy `spoiler` folder to TinyMCE `plugins` folder and add the necessary options to TinyMCE configuration.
Example configuration:

```javascript
tinymce.init({
  selector: "textarea",  // change this value according to your HTML
  plugins: "spoiler",
  menubar: "format",
  toolbar: "spoiler-add spoiler-remove"
});
```

Alternatively, you can place the plugin outside the TinyMCE folder
and add the plugin using `external_plugins` option of TinyMCE:

```javascript
tinymce.init({
  selector: "textarea",  // change this value according to your HTML
  external_plugins: {
            spoiler: '../spoiler/plugin.js'
        }
  menubar: "format",
  toolbar: "spoiler-add spoiler-remove"
});
```
**Note**: The path to the plugin file is relative to the main TinyMCE JavaScript file.

The plugin accepts an optional `spoiler_caption` configuration parameter that allows you to define
a custom caption for spoiler blocks (default: "Spoiler!").
The parameter accepts both plain text and html code, for example `<strong>Spoiler!</strong>`.

## Usage

To add a spoiler block, select a text that you want to hide under a spoiler or place a cursor
where you want to add a spoiler block and click "Add spoiler".

The plugin adds the following html code to an authored web page:

```html
<div class="spoiler">
  <div class="spoiler-toggle">Spoiler caption</div>
  <div class="spoiler-text">
    Spoiler text
  </div>
</div>
```

To remove a spoiler, click on a spoiler block and then click "Remove spoiler". The spoiler block will be converted
to a plain paragraph.

**Note**: For spoiler blocks to work properly you need to add to your web pages CSS/JavaScript
code that defines look and behavior of the spoiler blocks.

````javascript
window.addEventListener("load", function (event) {
    let elements = document.getElementsByClassName('spoiler');

    let i;
    for (i = 0; i < elements.length; i++) {

        let element = elements[i];
        let button = element.childNodes[1];
        let spoiler_text = element.childNodes[3];
        
        button.addEventListener('click', function (event) {
            if (spoiler_text.style.display === 'block') {
                spoiler_text.style.opacity = 0;
                setTimeout(function () {
                    spoiler_text.style.display = 'none';
                }, 300);
            } else {
                spoiler_text.style.display = 'block';
                let op = 0;
                let intervalID = setInterval(function () {
                    if (op > 1)
                        window.clearInterval(intervalID);
                    spoiler_text.style.opacity = op;
                    op += 0.01;
                }, 2);
            }
        });
    }
});
````
## License

[LGPL](http://www.gnu.org/licenses/lgpl-3.0.en.html).
