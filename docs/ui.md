# Overview

## What is this?
It's a thing meant to add generic, extensible UI creation on top of [Isotope](http://isotope.metafizzy.co), which is another thing.  It primarily shines when used to create compound boolean filters.

## Features

* Behavior fully configurable from HTML
* 2-way data binds filter state between the UI and URL search string
* Supports boolean filtering

# Building a Filter UI
The structure and behavior of the filter UI is entirely configurable by classing HTML elements.

## Example

For example the following code will create a filter group that has multi-select enabled, with a default value that also clears all other selected filters within that group:

```html
<div class="b-filter-ui">
    <div class="b-filter-ui-container js-multi">
        <a class="b-filter js-default js-clear" data-filter="*">All</a>
        <a class="b-filter" data-filter="man">Man</a>
        <a class="b-filter" data-filter="dog">Dog</a>
        <a class="b-filter" data-filter="bear">Bear</a>
    </div>
</div>
```

The base filter UI is specified by `b-filter-ui`; all UI groups and components should be wrapped inside this element.

`b-filter-ui-container` defines a UI group, which can then have additional options specified. In the example `js-multi` is an option used to configure the group as a multi-select, which will enable multiple filters to be selected within the group and all added to the filter string. In contrast, the default behavior for a group is a single select, where only one filter may be selected at a time.

`b-filter` specifies an individual filter UI component.  These might be something like buttons (as in the above example), or a `<select>` element, etc.  The options `js-default` and `js-clear` cause that filter to be selected automatically at load time, and clear any selected filters within the multi-select group, respectively.

`data-filter` specifies the actual filter to add to the filter string.  Unadorned single words without any css selector syntax characters will be treated as single classes to filter on (e.g. `bear` becomes `.bear` in the filter string). More complex css selectors are also supported, and these will be left pristine as written. A value consisting of only one or more `*` characters will be ignored and not included in the filter string.

All of the classnames used to configure the UI behavior can be customized [in the js file](#further-configuration)

## UI Classes

Default Classname       | Behavior
-------------------     | --------------------------------------
`b-filter-ui`           | Defines the base UI
`b-filter-ui-container` | Defines a UI group which can have additional options specified
`b-filter`              | Defines a filter UI component
`b-isotope`             | Defines the results area
`b-result`              | Defines a specific result that isotope can filter
`js-and`                | Configures AND boolean filtering; used on `b-filter-ui` element.  This is the default behavior if nothing is specified.
`js-or`                 | Configures OR boolean filtering; used on `b-filter-ui` element
`js-multi`              | Configures a `b-filer-ui-container` as a multi-select group
`js-default`            | `b-filter` will be selected automatically when page is loaded
`js-clear`              | `b-filter` will clear other filters within group when selected
`js-clear-all`          | `b-filter` will clear other filters within entire UI when selected
`js-click`              | Binds updating filter string to `click` event on the `b-filter`. This is the default behavior.
`js-change`             | Binds updating filter string to `change` event on the `b-filter`
`js-pre-filter`         | Used on `b-isotope` to set a default state for the results to be filtered to instead of `*`. This will be overridden as soon as any UI interactions are performed.  This also bypasses any binding on page load from URL if a search string is present.

Default Attribute       | Behavior
-------------------     | --------------------------------------
`data-filter`           | Specifies filter parameter used by the element. Currently, these must all be unique in order for the data binding to work correctly.


## Full Usage Example

```html
<!DOCTYPE html>
<html lang="en">
<head><title>isotope-ui UI Example</title></head>
<body>
    <style type="text/css">
    .b-card {
        background-color: #efe;
        width: 100px;
        height: 100px;
        display: inline-block;
        margin: 5px;
    }
    .b-filter:not(.js-change).m-active {
        background-color: #faa;
    }
    .b-filter-ui {
        background-color: #fee;
    }
</style>

<div class="b-filter-ui js-and">
    <h2>Example Filter UI</h2>
    <select id="andor">
        <option value="and">AND</option>
        <option value="or">OR</option>
    </select>
    <h3>Multi select container</h3>
    <div class="b-filter-ui-container js-multi">
        <a class="b-button b-filter js-default js-clear-all" data-filter="*">all</a>
        <a class="b-button b-filter" data-filter="man">man</a>
        <a class="b-button b-filter" data-filter="dog">dog</a>
        <a class="b-button b-filter" data-filter="bear">bear</a>
    </div>
    <h3>Single select container</h3>
    <div class="b-filter-ui-container">
        <a class="b-button b-filter" data-filter="thing">thing</a>
        <a class="b-button b-filter" data-filter="potato">potato</a>
        <a class="b-button b-filter" data-filter="car">car</a>
    </div>
    <h3>No container</h3>
    <a class="b-button b-filter" data-filter="creature">creature</a>
    <a class="b-button b-filter" data-filter="alien">alien</a>
    <a class="b-button b-filter" data-filter="public-prosecutor">public prosecutor</a>
    <h3>On change</h3>
    <select class="b-filter js-change">
        <option data-filter="* *">* *</option>
        <option data-filter="yo">yo</option>
        <option data-filter="hey">hey</option>
        <option data-filter="pancake">pancake</option>
        <option data-filter="squid">squid</option>
    </select>
</div>
<div class="b-isotope" data-filter="man">
    <div class="b-card b-result yo man creature">yo man creature</div>
    <div class="b-card b-result yo dog">yo dog</div>
    <div class="b-card b-result hey man alien">hey man alien</div>
    <div class="b-card b-result hey dog">hey dog</div>
    <div class="b-card b-result hey dog potato">hey dog potato</div>
    <div class="b-card b-result hey potato thing">hey potato thing</div>
    <div class="b-card b-result hey man alien pancake">hey man alien pancake</div>
    <div class="b-card b-result hey dog squid">hey dog squid</div>
    <div class="b-card b-result yo pancake alien creature">yo pancake alien creature</div>
    <div class="b-card b-result yo alien public-prosecutor">yo alien public prosecutor</div>
    <div class="b-card b-result hey squid car creature">hey squid car creature</div>
    <div class="b-card b-result alien potato bear">alien potato bear</div>
</div>
<script src="//code.jquery.com/jquery-2.2.3.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/jquery.isotope/2.2.2/isotope.pkgd.min.js"></script>
<script src="isotope-ui.js"></script>
<script>
$(function() {
    $('#andor').on('change', function() {
        if ($(this).val() === 'and') {
            $(this).closest('.b-filter-ui').removeClass('js-or');
            $(this).closest('.b-filter-ui').addClass('js-and');
        } else {
            $(this).closest('.b-filter-ui').removeClass('js-and');
            $(this).closest('.b-filter-ui').addClass('js-or');
        }
    });
});
</script>
</body>
</html>
```

# Further Configuration

It is possible to further customize the configuration by editing options within `isotope-ui.js`. Available options are at the top of the js file as follows:

```js
var jsBindings = {
grid            : '.b-isotope',     // selector to bind the isotope grid to
uiBase          : '.b-filter-ui',   // selector to bind the parent element of all containers / ui elements on page
uiContainer     : '.b-filter-ui-container', // selector for ui container elements
ui              : '.b-filter, .b-filter option' // selector to bind the ui element(s) to
},

jsOptions = {

// for ui base
and             : 'js-and',         // sets boolean filtering mode to AND
or              : 'js-or',          // sets boolean filtering mode to OR

// for ui containers
multiSelect     : 'js-multi',       // multiple select option, otherwise defaults to single select

// for ui elements
click           : 'js-click',       // binds filter behavior to click event
change          : 'js-change',      // binds filter behavior to change event
clearFilters    : 'js-clear',       // clears other filters when this filter is selected (useful for multi select)
clearAllFilters : 'js-clear-all',   // clears all filters in uiBase when this filter is selected
defaultFilter   : 'js-default',     // default filter to select on page load
selectedClass   : 'm-active',       // class to toggle on ui control if selected (added automatically)

// for grid element
preFilter       : 'js-pre-filter',  // sets results to pre-filter on page load (ignores location.search)

// etc
filterOn        : 'data-filter'     // attribute name to be used for determining filter string(s)

},

isotopeDefaults = {
itemSelector    : '.b-result',
layoutMode      : 'fitRows',
sortBy          : 'category'
},

searchKey   = 'filter';     // location.search key name to be used for url-based filtering
```

The values of any of these options can be modified.  They are grouped into the following categories:

Name                | Purpose
------------------- | --------------------------------------
`jsBindings`        | Defines the css classes to bind the various UI components to
`jsOptions`         | Defines css class names to use to toggle the HTML configurable options on the UI components
`isotopeDefaults`   | Configuration options for Isotope. See isotope.metafizzy.co for documentation.
`searchKey`         | specifies the name of the key appearing after `?` in the browser search string that will be used for data binding.


# URL Data Binding

isotope-ui features 2-way data binding to the URL search string, which enables filter states to be preserved and linked.  Only the key specified by `searchKey` in the options is modified, while any other keys present in the URL search string will be left pristine and will not affect the data binding.

The UI can be set to either treat multiple selected values across UI groups as boolean AND or OR searches. In the case of AND, a filter string will be bound in a pattern such as `.filter1.filter2.filter3`, while OR will bind a string in the pattern `.filter1,.filter2,.filter3` which can then be filtered on directly by isotope. The default is AND.