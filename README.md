# tracking modifications to: [HAR Viewer](https://github.com/janodvarko/harviewer)

* Author: [Jan Odvarko](http://www.softwareishard.com/blog/har-viewer/)

## Unmodified/Original Source

  * [commit SHA-1 hash: 22c7b7b21c1db0f80ebcf8955ceea56b316ddde3, authored on Feb 26 2014](https://github.com/janodvarko/harviewer/tree/22c7b7b21c1db0f80ebcf8955ceea56b316ddde3)

## Change/Commit Log

  * [375fc21](https://github.com/warren-bank/moz-harviewer/commit/375fc2138823cfdb2eaebaf9e36fe8ec63a5b7f8):
    * mirror of the following directories taken from the _Unmodified/Original Source_:
      * [webapp/css](https://github.com/janodvarko/harviewer/tree/22c7b7b21c1db0f80ebcf8955ceea56b316ddde3/webapp/css)
      * [webapp/scripts](https://github.com/janodvarko/harviewer/tree/22c7b7b21c1db0f80ebcf8955ceea56b316ddde3/webapp/scripts)

    * SHA-1 hash value comparison:
        * _Unmodified/Original Source_:
          * [webapp](https://api.github.com/repos/janodvarko/harviewer/contents/webapp?ref=22c7b7b21c1db0f80ebcf8955ceea56b316ddde3) tree object:

            path | sha
            ---- | ---
            [webapp/css](https://api.github.com/repos/janodvarko/harviewer/contents/webapp/css?ref=22c7b7b21c1db0f80ebcf8955ceea56b316ddde3) | "d9a9e17e5b6092adb2c8dde849b6948720973eda"
            [webapp/scripts](https://api.github.com/repos/janodvarko/harviewer/contents/webapp/scripts?ref=22c7b7b21c1db0f80ebcf8955ceea56b316ddde3) | "c1934ce91cae5e15eef29e08b57de557c57db7ef"

        * _libs/harviewer_:
          * [_root_](https://api.github.com/repos/warren-bank/moz-harviewer/contents?ref=375fc2138823cfdb2eaebaf9e36fe8ec63a5b7f8) tree object <sub>(ie: `375fc21^{tree}`)</sub>:

            path | sha
            ---- | ---
            [css](https://api.github.com/repos/warren-bank/moz-harviewer/contents/css?ref=375fc2138823cfdb2eaebaf9e36fe8ec63a5b7f8) | "d9a9e17e5b6092adb2c8dde849b6948720973eda"
            [scripts](https://api.github.com/repos/warren-bank/moz-harviewer/contents/scripts?ref=375fc2138823cfdb2eaebaf9e36fe8ec63a5b7f8) | "c1934ce91cae5e15eef29e08b57de557c57db7ef"

        * result: confirmation that these files are all identical

  > this version will serve as the baseline, which future (modified) commits should be diff'ed against

  * [718c544](https://github.com/warren-bank/moz-harviewer/compare/375fc2138823cfdb2eaebaf9e36fe8ec63a5b7f8...718c5444a1504462f2a28bca1e4f0082d2831b36):
    * This commit differs from the production code contained in the [tagged release: v1.0.0](https://github.com/warren-bank/moz-harviewer/releases/tag/v1.0.0).
      A subsequent commit to this branch (labeled: v1.0.0) will be made that will directly correspond to this production code.
      However, it may be of interest to developers to see where things (both script and css) break when the doctype is removed,
      and the page is forced to render in quirks mode.
    * the diff for this commit will reveal the particulars, but high level:
      * scripts that aren't needed are removed
      * usage of `scripts/core/cookies.js` to store user preferences is replaced by add-on preferences
      * `scripts/downloadify/*` (a Flash library used to trigger a save-as dialog for the user to extract/save the HAR data to disk)
        is removed, and replaced by a pure javascript alternative (in `scripts/tabs/previewTab.js`)
      * `scripts/app.build.js`, the [RequireJS Optimizer](http://requirejs.org/docs/optimization.html) build config file, is rewritten to combine all of the modules into a single file
      * `scripts/i18n.js` is replaced with the [latest version](https://github.com/requirejs/i18n/raw/87ce4b30c75fece0aedb5ca0ba1be4194147259d/i18n.js)
      * `scripts/require.js` is replaced by [Almond](https://github.com/jrburke/almond/raw/36b4b28163c31b699235534686e32d9585d9b826/almond.js),
        which is a minimal/barebones [AMD module](http://requirejs.org/docs/whyamd.html) loader.

        > issue:

        > * to make this loader compatible with the AMD modules in this project, one additional line of code was necessary: `requirejs.def = define;`

    * adds a `demo` directory, which contains:
      * `demo/scripts/jquery.min.js`:
        > minified copy of [jQuery 1.5.1](http://ajax.googleapis.com/ajax/libs/jquery/1.5.1/jquery.min.js)

        > (same version as the non-minified copy in the _unmodified/original source_)

      * `demo/scripts/harViewer.min.js`:
        > the output from the [RequireJS Optimizer](https://github.com/jrburke/r.js):

        >> `cd scripts`

        >> `node /path/to/r.js -o app.build.js optimize=none out="../demo/scripts/harViewer.min.js"`

        > the production version (used in the add-on) is optimized with `uglify`, which minifies the output.

      * `demo/scripts/demo.js`:
        > contains a static snapshot of the code that is dynamically generated by the add-on:

        > * the source of the HAR data used in this particular example is:
           [httparchive.org](http://httparchive.webpagetest.org/export.php?test=140801_0_8JH&run=1&cached=0&pretty=0)

        >   this is the same HAR data as is shown in the
            [README screenshot](https://raw.githubusercontent.com/warren-bank/moz-harviewer/screenshots/01.png)

        > * the add-on embeds this code directly within the .html document.

        >   the demo uses an external script file simply for efficiency sake;
            a single git blob will be shared across all variations of the demo .html files.

      * `demo/index.standards_mode.html`:
        > a working demo to confirm proper functionality.

        > contains an HTML5 doctype

      * `demo/index.quirks_mode.html`:
        > identical to `demo/index.standards_mode.html`, with one exception:

        > omits any doctype declaration

  * [v1.0.0] [998bea6](https://github.com/warren-bank/moz-harviewer/compare/375fc2138823cfdb2eaebaf9e36fe8ec63a5b7f8...998bea6740e91749f579c5c0b0163cc9594a7964):
    * This commit corresponds to the [tagged release: v1.0.0](https://github.com/warren-bank/moz-harviewer/releases/tag/v1.0.0)
    * updates:
      * `scripts/domplate/infoTip.js` can now properly determine the height of the document when the page is rendered in quirks mode
      * the `demo` directory contains an additional file:
        * `demo/css/quirks.css`:

          > css rules to correct for styling issues when the page is rendered in quirks mode

  * [__HEAD__](https://github.com/warren-bank/moz-harviewer/compare/375fc2138823cfdb2eaebaf9e36fe8ec63a5b7f8...libs/harviewer):
    * All earlier modifications in this branch (to the _unmodified/original source_) have been done
      with the intention of retaining the desired existing functionality while reducing the codebase.
    * This is the first modification intended to add new/enhanced functionality.
    * A secondary download button has been added to the toolbar.
      It differs from the original, in that its behavior is to export/save a __sanitized__ copy of the HAR data.
      Exactly what data is filtered/removed from the original can be configured in _user preferences_.
    * In this initial version, the only filtering options are:
      * remove cookies:
        - [ ] whole header
        - [x] value only

        &nbsp;

      > cookie data is stored in:
        * log.entries[].request.cookies
        * log.entries[].request.headers
        * log.entries[].response.cookies
        * log.entries[].response.headers

      > the option to remove the _whole header_ removes whole data objects from these arrays. Not only are the cookie values removed (which are the cause for security concerns in passing around _unsanitized_ HAR data files), but all trace of their existence is lost as well.

      > the option to remove the _value only_ retains information about cookies that were present during the logged session:
        * their names
        * their transmission to/from the server

      > however, within this retained data, the value of the cookies are wiped clean.
        In all instances, the real value is replaced by an empty string.
