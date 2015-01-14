## See rendered HTML immediately.

While researching for [react.rocks](http://react.rocks), I often find projects without a online demo. If a project has an /examples dir, the page might work served through the mighty [rawgit](http://rawgit.com).

Navigating to rawgit.com to build and test URL's got tedious, [bookmarklet](http://subsimple.com/bookmarklets/rules.php) seemed a better approach. Goals:
* Work from a directory or index.html
* Production-friendly URL. Beware the caveats: _"It's best to use a specific tag or commit hash in the URL (not a branch). Files are cached permanently after the first request"_

Here is the JS pre-minification. Splitting the pathname was a nice clean way to do string manipulation.

```
var sections=location.pathname.split('/');
sections[0] = 'https://cdn.rawgit.com';
if (sections[3]==='blob' || sections[3]==='tree') { sections.splice(3,1)}
if (sections[sections.length-1] !== 'index.html') { sections.push('index.html')}
var url=sections.join('/');
location.href=url
```

and here is the source for the JS bookmarklet:
```
javascript:(function(){ var sections=location.pathname.split("/");sections[0]="https://cdn.rawgit.com";if(sections[3]==="blob"||sections[3]==="tree"){sections.splice(3,1)}if(sections[sections.length-1]!=="index.html"){sections.push("index.html")}var url=sections.join("/");location.href=url})();
```

#### Installation
Github, very sensibly, won't allow javascript: urls. Sorry, but if you're reading this, you probably know what to do.

#### Known Issues:
* blindly appends /index.html, without checking for presence.
* Github Pages is a better choice without bandwidth / caching restrictions.
