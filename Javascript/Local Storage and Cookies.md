# Local Storage and Cookies

## What is local storage?

Local storage is a part of web storage, which itself is part of the HTML5 specification. There are two options for storing data in the specification:

The two mechanisms within Web Storage are as follows:

- **sessionStorage **maintains a separate storage area for each given origin that's available for the duration of the page session (as long as the browser is open, including page reloads and restores).
- **localStorage **does the same thing, but persists even when the browser is closed and reopened.


- Local Storage: stores data with no expiration date and this is the one we will be using because we want our to-do’s to stay on the page for as long as possible.
- Session Storage: only saves the data for one session so if the user closes the tab and reopens it all their data will be gone.



The read-only `localStorage` property allows you to access a [`Storage`](https://developer.mozilla.org/en-US/docs/Web/API/Storage) object for the [`Document`](https://developer.mozilla.org/en-US/docs/Web/API/Document)'s origin; the stored data is saved across browser sessions. `localStorage` is similar to `sessionStorage`, except that while data stored in `localStorage` has no expiration time, data stored in `sessionStorage` gets cleared when the page session ends — that is, when the page is closed.

It should be noted that data stored in either `localStorage` or `sessionStorage` **is specific to the protocol of the page**.

Exceptions :- SecurityError



## Syntax

```
localStorage.setItem('myCat', 'Tom');
var cat = localStorage.getItem("myCat");
localStorage.removeItem("myCat");
```

**Note**: It's recommended to use the Web Storage API (`setItem`, `getItem`, `removeItem`, `key`, `length`) to prevent the [pitfalls](http://www.2ality.com/2012/01/objects-as-maps.html) associated with using plain objects as key-value stores.



## Feature-detecting localStorage

```
function storageAvailable(type) {
    try {
        var storage = window[type],
            x = '__storage_test__';
        storage.setItem(x, x);
        storage.removeItem(x);
        return true;
    }
    catch(e) {
        return e instanceof DOMException && (
            // everything except Firefox
            e.code === 22 ||
            // Firefox
            e.code === 1014 ||
            // test name field too, because code might not be present
            // everything except Firefox
            e.name === 'QuotaExceededError' ||
            // Firefox
            e.name === 'NS_ERROR_DOM_QUOTA_REACHED') &&
            // acknowledge QuotaExceededError only if there's something already stored
            storage.length !== 0;
    }
}
```



## StorageEvent

The [`StorageEvent`](https://developer.mozilla.org/en-US/docs/Web/API/StorageEvent) is fired whenever a change is made to the [`Storage`](https://developer.mozilla.org/en-US/docs/Web/API/Storage) object (note that this event is not fired for sessionStorage changes). This won't work on the same page that is making the changes — it is really a way for other pages on the domain using the storage to sync any changes that are made. Pages on other domains can't access the same storage objects.

```
window.addEventListener('storage', function(e) {  
  document.querySelector('.my-key').textContent = e.key;
  document.querySelector('.my-old').textContent = e.oldValue;
  document.querySelector('.my-new').textContent = e.newValue;
  document.querySelector('.my-url').textContent = e.url;
  document.querySelector('.my-storage').textContent = JSON.stringify(e.storageArea);
});
```



# Cookies

document.cookie  :- is a string containing a semicolon-separated list of all cookies (i.e. `*key*=*value*` pairs)

document.cookie= newCookie; :- Note that you can only set/update a single cookie at a time using this method.



- The cookie value string can use [`encodeURIComponent()`](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/encodeURIComponent) to ensure that the string does not contain any commas, semicolons, or whitespace (which are disallowed in cookie values).

- ```
  document.cookie = "doSomethingOnlyOnce=; expires=Thu, 01 Jan 1970 00:00:00 GMT";
  ```

## Security

It is important to note that the path attribute does **not** protect against unauthorized reading of the cookie from a different path. It can be easily bypassed using the DOM, for example by creating a hidden [iframe](https://developer.mozilla.org/en-US/docs/HTML/Element/iframe) element with the path of the cookie, then accessing this iframe's `contentDocument.cookie` property. The only way to protect the cookie is by using a different domain or subdomain, due to the [same origin policy](https://developer.mozilla.org/en-US/docs/Same_origin_policy_for_JavaScript).

Cookies are often used in web application to identify a user and their authenticated session. So stealing cookie from a web application, will lead to hijacking the authenticated user's session. Common ways to steal cookies include using Social Engineering or by exploiting an XSS vulnerability in the application -

- You can delete a cookie by simply updating its expiration time to zero.
- Keep in mind that the more cookies you have, the more data will be transferred between the server and the client for each request. This will make each request slower. It is highly recommended for you to use [WHATWG DOM Storage](https://developer.mozilla.org/en-US/docs/DOM/Storage) if you are going to keep "client-only" data.

The [`path`](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie#new-cookie_path) parameter of a new cookie can accept only *absolute* paths. If you want to use *relative* paths, you'll need to convert them. The following function can translate *relative*paths to *absolute* paths. It is a general-purpose function, but can be of course successfully used for the [`path`](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie#new-cookie_path) parameter of a new cookie, as well.