# Features

* Behavior fully configurable from HTML
* 2-way data binds filter state between the UI and URL search string
* Supports boolean filtering

# Building a Filter UI
The structure and behavior of the filter UI is entirely configurable by classing HTML elements.

For example the following code will create a filter group that has multi-select enabled, with a default value that also clears all other selected filters within that group:

```html
<div class="b-filter-ui-container js-multi">
    <a class="b-button b-filter js-default js-clear" data-filter="*">All</a>
    <a class="b-button b-filter" data-filter="man">Man</a>
    <a class="b-button b-filter" data-filter="dog">Dog</a>
    <a class="b-button b-filter" data-filter="bear">Bear</a>
</div>
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

    jsOptions = {                           // names for template-configurable options. e.g. class="js-some-option"
        // for ui base
        and             : 'js-and',
        or              : 'js-or',

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

    isotopeDefaults = {                     // default options for isotope. these can be overwritten with corresponding jsOptions
        itemSelector    : '.b-result-item',
        layoutMode      : 'fitRows',
        sortBy          : 'category'
    },

        searchKey   = 'filter';             // location.search key name to be used for url-based filtering

```

The values of any of these options can be modified.  They are grouped into the following categories:

------------------- | --------------------------------------
`jsBindings`        | Defines the css classes to bind the various UI components to
`jsOptions`         | Defines css class names to use to toggle the HTML configurable options on the UI components
`isotopeDefaults`   | Configuration options for Isotope. See isotope.metafizzy.co for documentation.

`searchKey` specifies the name of the key appearing after `?` in the browser search string that will be used for data binding.


# URL Data Binding

isotope-ui features 2-way data binding to the URL search string, which enables filter states to be preserved and linked.  Only the key specified by `searchKey` in the options is modified, while any other keys present in the URL search string will be left pristine and will not affect the data binding.

The UI can be set to either treat multiple selected values across UI groups as boolean AND or OR searches. In the case of AND, a filter string will be bound in a pattern such as `.filter1.filter2.filter3`, while OR will bind a string in the pattern `.filter1,.filter2,.filter3` which can then be filtered on directly by isotope. The default is AND.