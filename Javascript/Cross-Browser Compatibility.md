## Modern tools can help

Now you know a few great reasons to make your web site more compatible. But how do you do it?

- If you’ve found a bug on *someone else’s site*, file it at [webcompat.com](http://webcompat.com/?utm_source=hacks&utm_medium=blog-post&utm_campaign=compat)! If you’re debugging your own site, read on.
- Try your site in different browsers and move through it as a user might. Watch the developer console in the browser’s developer tools for errors (most modern desktop browsers have incredibly capable developer tools built in to help you debug issues, even on mobile):
  - [Firefox dev tools](https://developer.mozilla.org/en-US/docs/Tools?utm_source=hacks&utm_medium=blog-post&utm_campaign=compat)
  - [Chrome dev tools](https://developers.google.com/web/tools/chrome-devtools/)
  - [IE/Edge dev tools](https://developer.microsoft.com/en-us/microsoft-edge/platform/documentation/f12-devtools-guide/)
  - [Safari dev tools](https://developer.apple.com/safari/tools/)
  - [Opera dev tools](http://www.opera.com/developer)
- If you find a bug that is not in your site, maybe it’s a bug in the browser! Open a bug report so your browser’s developers can fix it:
  - [Mozilla Bugzilla](https://bugzilla.mozilla.org/)
  - [Chrome issue tracker](https://bugs.chromium.org/p/chromium/issues/list)
  - [IE/Edge](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/)
  - [Safari/Webkit](https://webkit.org/reporting-bugs/)
  - [Opera](https://bugs.opera.com/wizard/desktop)
- Integrate a popular cross-browser-testing tool into your deploy process, to make cross-browser testing automatic:
  - [BrowserStack](https://www.browserstack.com/)
  - [Sauce Labs](https://saucelabs.com/)
  - [Browserling](https://www.browserling.com/)
- Understand which browsers have implemented web features before using them on your site:
  - [Caniuse](http://caniuse.com/)
  - [MDN](https://developer.mozilla.org/en-US/docs/Web?utm_source=hacks&utm_medium=blog-post&utm_campaign=compat)’s compatibility tables
  - [Kangax](http://kangax.github.io/compat-table)’s ECMAScript compatibility tables
- Explore coding tools that can improve cross-browser compatibility:
  - [Autoprefixer](https://github.com/postcss/autoprefixer), [CSSNext](http://cssnext.io/), [Oldie](https://github.com/jonathantneal/oldie) and other [PostCSS](https://github.com/postcss/postcss#readme) plugins make it possible to write pure, modern CSS that does not break in older browsers
  - [Modernizr](https://modernizr.com/) helps you identify and handle implementation differences between browsers (use this instead of UA sniffing)
  - [@supports](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports?utm_source=hacks&utm_medium=blog-post&utm_campaign=compat) helps you build [progressive enhancements](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement?utm_source=hacks&utm_medium=blog-post&utm_campaign=compat) into the web experience for more capable browsers
- Go deep. Learn about the web’s many features and quirks. The more you know about it the more you will love it:
  - [MDN](https://developer.mozilla.org/en-US/docs/Web?utm_source=hacks&utm_medium=blog-post&utm_campaign=compat)
  - [Google Developers](https://developers.google.com/web/)
  - [W3C Developers](https://www.w3.org/developers/)