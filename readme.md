# etc.js

etc.js is a small javascript library with plenty of handy functions. 

## Elements

**_e** function performs quering of DOM elements and wraps them in an array, 
provides methods to iterate over the array, access attributes and css classes of that elements.

### _e(element) -> []

Wraps a single DOM element into array to give access to methods of the library

### _e(selector [,context]) -> []

- **selector** is a string coontaining one or more CSS selectors separated by coma (more about CSS selectors [on MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors))
- **context** is a root element to search from; when omitted, the function returns all the elements in the document that are matched by any of the specified selectors

Returns an array of elements that are matched by any of the specified selectors that implements a set of functions to iterate over elements:

### .even() -> []

Returns even elements in the array

### .odd() -> []

Returns odd elements in the array

### .first() -> []

Returns a new array containing the first element of the array

### .last() -> [] 

Returns a new array containing the last element of the array

### .node -> Element

Unwraps and returns the fist DOM element in the array

### .val() -> String

Returns context-dependent value of the first element (`true`/`false` for checkbox `INPUT` elements, `.value` attribute value for `INPUT`, `SELECT`, `TEXTAREA` elements, `.innerHTML` for the rest elements)

### .val(newVlaue)

Sets the value of the first element (`.checked` attribute of checkbox `INPUT` elements, `.value` attribute of `INPUT`, `SELECT`, `TEXTAREA` elements, `.innerHTML` of the rest elements)

---

#### Attributes

### .attr() -> {}

Returns an object with attributes of the first element

---

#### Classes

### .css(classValue)

- **classValue** a string or an array of strings representing CSS classes

This function applies CSS classes in *classValue* to all elements in the array

### .css() -> []

Returns an array that implements methods to manipulate CSS classes of an element:

### .add(className) -> []

- **className** CSS class name

Adds class *className* to the first element's `.class` attribute

Returns the same array of CSS classes, allows chaining

### .remove(className) -> []

- **className** CSS class name

Removes class *className* from the first element's `.class` attribute

Returns the same array of CSS classes, allows chaining

### .toggle(className) -> []

- **className** CSS class name

Adds *className* if `.class` attribtute of the first element doesn't contain it, removes otherwise

Returns the same array of CSS classes, allows chaining

### .has(className) -> Bool

- **className** CSS class name

Returns `true` if `.class` atribute contains *className*, `false` otherwise

## Templates

**_t** function provides templating for the library

### _t(template, values) -> String

- **template** a string of text with placeholders, e.g., 'Hello {username}!'
- **values** an object containing values for the template, e.g., {username: 'Jerry'}

It's a simple function handy when you need to add values to a string, for example:

`_t('Hello {username}!', {username: 'Jerry'})` will return `Hello, Jerry!`

### _t(element, prefix, values)

A more complex approach to templating

- **element** a DOM element containing template elements
- **attribute** elements data-attribute for field names
- **values** an object containing field values

For example, HTML code for template: 

    <div id="form-fields">
      <input field-name="username" type="text" />
      prefers
      <select field-name="prefers">
        <option></option>
      </select>
    </div>

Can be filled in with values this way:

    _t(_e('form-fields').node, 'field-name', {username: '', prefers: ['tea', 'coffee']})


### _t(element, prefix) -> {}

- **element** a DOM element containing template elements
- **attribute** elements data-attribute for field names

Returns an object with form values, for the example above, after user enters his name and selects what he prefers:

    <div id="form-fields">
      <input field-name="username" type="text" value="John" />
      prefers
      <select field-name="prefers">
        <option>tea</option>
        <option selected>coffee</option>
      </select>
    </div>

The function returns:

    _t(_e('form-fields).node, 'field-name') -> {username: 'John', prefers: 'coffee'}

## Control

**_c** function provides general control over page behaviour

### .bindValue(object, propertyName, element, callback)

- **object** an object 
- **propertyName** a string, property name that will be added to the object
- **element** DOM element, value of the element will be binded to `propertyName`
- **callback** a callback function `callback(newValue)` 

Performs bidirectional binding between `object.propertyName` and `element` values; 
whenever `object.propertyName` changes, `element` value changes as well, 
when user changes the `element` value, `object.propertyName` value reflects the change. In both cases `callback(newValue)` is called.

### .merge(array1, array2, options) -> []

- **array1** first array
- **array2** second array to be merged with the first one
- **options** merge options:
  - undefined: append elements from array2 that are not in array1;
  - false: remove array2 elements from array1
  - true: toggle a2 elements in a1

The function merges two arrays, for example:

    _c.merge([1, 2], [2, 3]) -> [1, 2, 3]
    _c.merge([1, 2], [2, 3], false) -> [1]
    _c.merge([1, 2], [2, 3], true) -> [1, 3]

### .merge(object1, object2, options) -> {}

- **object1** first object
- **object2** second object to be merged with the first one
- **options** true to replace values in object1 with values from object2

The function merges two objects, for example:

    var o1 = {a: 1, b: 2},
        o2 = {b: 3, c: 4}
    _c.merge(o1, o2) -> {a: 1, b: 2, c: 4}
    _c.merge(o1, o2, true) -> {a: 1, b: 3, c: 4}

### .clone(array) -> []

Makes a deep copy of an array

### .clone(object) -> {}

Makes a deep copy of an object 

### .owns(object, propertyName) -> Bool

- **object** an object
- **propertyName** a string with property name

hasOwnProperty alias

### .keys(event, combination)

Compare keyboard event with a human readable keyboard combination, for example:

    window.keydown = function(e) {
        _c.keys(e, 'ctrl+s') // returns true when user presses Ctrl + S combination
    }

---

#### AJAX requests

### .get(link, params, callback)

- **link** request URL
- **params** an object containing request parameters
- **callback** callback function `callback(responseObject, responseString, requestObject)` where
  - **responseObject** contains an object if response can be parsed as a JSON object
  - **responseString** contains response text representation
  - **requestObject** requestObject value

Performs GET request

### .post(link, params, callback)

- **link** request URL
- **params** an object containing request parameters
- **callback** callback function `callback(responseObject, responseString, requestObject)` where
  - **responseObject** contains an object if response can be parsed as a JSON object
  - **responseString** contains response text representation
  - **requestObject** requestObject value

Performs POST request

### .put(link, params, callback)

- **link** request URL
- **params** an object containing request parameters
- **callback** callback function `callback(responseObject, responseString, requestObject)` where
  - **responseObject** contains an object if response can be parsed as a JSON object
  - **responseString** contains response text representation
  - **requestObject** requestObject value

Performs PUT request

### .del(link, params, callback)

- **link** request URL
- **params** an object containing request parameters
- **callback** callback function `callback(responseObject, responseString, requestObject)` where
  - **responseObject** contains an object if response can be parsed as a JSON object
  - **responseString** contains response text representation
  - **requestObject** requestObject value

Performs DELETE request

---

#### Helper functions

### .isObject(value) -> Bool

Returns `true` if `value` is an object

### .isArray(value) -> Bool

Returns `true` if `value` is an array

### .isString(value)

Returns `true` if `value` is a string

### .isNumber(value)

Returns `true` if `value` is a number


