# isotope-ui
HTML-configurable boolean filter UIs using [Isotope](http://isotope.metafizzy.co/)

Requires:
* JQuery (http://jquery.com/)
* Isotope (http://isotope.metafizzy.co/)

## Getting Started
Copy isotope-ui.js into your project and link it, after linking the above two libraries previously in the script load order.

Like so ~
```html
<script src="//code.jquery.com/jquery-2.2.3.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/jquery.isotope/2.2.2/isotope.pkgd.min.js"></script>
<script src="isotope-ui.js"></script> <!-- this is it, THIS. IS. IT. -->
```

## Example

A simple example using a single UI group;

```
<h3>Single select container</h3>
<div class="b-filter-ui-container">
    <a class="b-button b-filter" data-filter="thing">thing</a>
    <a class="b-button b-filter" data-filter="potato">potato</a>
    <a class="b-button b-filter" data-filter="car">car</a>
</div>
```

## Building Stuff

A detailed usage guide is available in the next section, [Using isotope-ui &gt;&gt;](ui/)