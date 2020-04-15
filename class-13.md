# Read: 13 - Local Storage
* a lot of storage space
* on the client
* that persists beyond a page refresh
* and isn’t transmitted to the server
* `function supports_html5_storage() {
  try {
    return 'localStorage' in window && window['localStorage'] !== null;
  } catch (e) {
    return false;
  }
}`
* Instead of writing this function yourself, you can use Modernizr to detect support for HTML5 Storage.

`if (Modernizr.localstorage) {
  // window.localStorage is available!
} else {
  // no native support for HTML5 storage :(
  // maybe try dojox.storage or a third-party solution
}`
* `var foo = localStorage.getItem("bar");
// ...
localStorage.setItem("bar", foo);`
…could be rewritten to use square bracket syntax instead:

`var foo = localStorage["bar"];
// ...
localStorage["bar"] = foo;`
* There are also methods for removing the value for a given named key, and clearing the entire storage area (that is, deleting all the keys and values at once).

`interface Storage {
  deleter void removeItem(in DOMString key);
  void clear();
};`