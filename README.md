# Projecthead
 
markdown-it
Build Status NPM version Coverage Status Gitter

Markdown parser done right. Fast and easy to extend.

Live demo

Follows the CommonMark spec + adds syntax extensions & sugar (URL autolinking, typographer).
Configurable syntax! You can add new rules and even replace existing ones.
High speed.
Safe by default.
Community-written plugins and other packages on npm.
Table of content

markdown-it
Install
Usage examples
Simple
Init with presets and options
Plugins load
Syntax highlighting
Linkify
API
Syntax extensions
Manage rules
Benchmark
Support markdown-it
Authors
References / Thanks
Install
node.js & bower:

npm install markdown-it --save
bower install markdown-it --save
browser (CDN):

jsDeliver CDN
cdnjs.com CDN
Usage examples
See also:

API documentation - for more info and examples.
Development info - for plugins writers.
Simple
// node.js, "classic" way:
var MarkdownIt = require('markdown-it'),
    md = new MarkdownIt();
var result = md.render('# markdown-it rulezz!');

// node.js, the same, but with sugar:
var md = require('markdown-it')();
var result = md.render('# markdown-it rulezz!');

// browser without AMD, added to "window" on script load
// Note, there is no dash in "markdownit".
var md = window.markdownit();
var result = md.render('# markdown-it rulezz!');
Single line rendering, without paragraph wrap:

var md = require('markdown-it')();
var result = md.renderInline('__markdown-it__ rulezz!');
Init with presets and options
(*) presets define combinations of active rules and options. Can be "commonmark", "zero" or "default" (if skipped). See API docs for more details.

// commonmark mode
var md = require('markdown-it')('commonmark');

// default mode
var md = require('markdown-it')();

// enable everything
var md = require('markdown-it')({
  html: true,
  linkify: true,
  typographer: true
});

// full options list (defaults)
var md = require('markdown-it')({
  html:         false,        // Enable HTML tags in source
  xhtmlOut:     false,        // Use '/' to close single tags (<br />).
                              // This is only for full CommonMark compatibility.
  breaks:       false,        // Convert '\n' in paragraphs into <br>
  langPrefix:   'language-',  // CSS language prefix for fenced blocks. Can be
                              // useful for external highlighters.
  linkify:      false,        // Autoconvert URL-like text to links

  // Enable some language-neutral replacement + quotes beautification
  typographer:  false,

  // Double + single quotes replacement pairs, when typographer enabled,
  // and smartquotes on. Could be either a String or an Array.
  //
  // For example, you can use '«»„“' for Russian, '„“‚‘' for German,
  // and ['«\xA0', '\xA0»', '‹\xA0', '\xA0›'] for French (including nbsp).
  quotes: '“”‘’',

  // Highlighter function. Should return escaped HTML,
  // or '' if the source string is not changed and should be escaped externally.
  // If result starts with <pre... internal wrapper is skipped.
  highlight: function (/*str, lang*/) { return ''; }
});
Plugins load
var md = require('markdown-it')()
            .use(plugin1)
            .use(plugin2, opts, ...)
            .use(plugin3);
Syntax highlighting
Apply syntax highlighting to fenced code blocks with the highlight option:

var hljs = require('highlight.js'); // https://highlightjs.org/

// Actual default values
var md = require('markdown-it')({
  highlight: function (str, lang) {
    if (lang && hljs.getLanguage(lang)) {
      try {
        return hljs.highlight(lang, str).value;
      } catch (__) {}
    }

    return ''; // use external default escaping
  }
});
Or with full wrapper override (if you need assign class to <pre>):

var hljs = require('highlight.js'); // https://highlightjs.org/

// Actual default values
var md = require('markdown-it')({
  highlight: function (str, lang) {
    if (lang && hljs.getLanguage(lang)) {
      try {
        return '<pre class="hljs"><code>' +
               hljs.highlight(lang, str, true).value +
               '</code></pre>';
      } catch (__) {}
    }

    return '<pre class="hljs"><code>' + md.utils.escapeHtml(str) + '</code></pre>';
  }
});
Linkify
linkify: true uses linkify-it. To configure linkify-it, access the linkify instance through md.linkify:

md.linkify.tlds('.py', false);  // disables .py as top level domain
API
API documentation

If you are going to write plugins - take a look at Development info.

Syntax extensions
Embedded (enabled by default):

Tables (GFM)
Strikethrough (GFM)
Via plugins:

subscript
superscript
footnote
definition list
abbreviation
emoji
custom container
insert
mark
... and others
Manage rules
By default all rules are enabled, but can be restricted by options. On plugin load all its rules are enabled automatically.

// Activate/deactivate rules, with curring
var md = require('markdown-it')()
            .disable([ 'link', 'image' ])
            .enable([ 'link' ])
            .enable('image');

// Enable everything
md = require('markdown-it')({
  html: true,
  linkify: true,
  typographer: true,
});
You can find all rules in sources: parser_core.js, parser_block, parser_inline.

Benchmark
Here is the result of readme parse at MB Pro Retina 2013 (2.4 GHz):

make benchmark-deps
benchmark/benchmark.js readme

Selected samples: (1 of 28)
 > README

Sample: README.md (7774 bytes)
 > commonmark-reference x 1,222 ops/sec ±0.96% (97 runs sampled)
 > current x 743 ops/sec ±0.84% (97 runs sampled)
 > current-commonmark x 1,568 ops/sec ±0.84% (98 runs sampled)
 > marked x 1,587 ops/sec ±4.31% (93 runs sampled)
Note. CommonMark version runs with simplified link normalizers for more "honest" compare. Difference is ~ 1.5x.

As you can see, markdown-it doesn't pay with speed for it's flexibility. Slowdown of "full" version caused by additional features not available in other implementations.

Support markdown-it
You can support this project via Tidelift subscription.

Authors
Alex Kocharin github/rlidwka
Vitaly Puzrin github/puzrin
markdown-it is the result of the decision of the authors who contributed to 99% of the Remarkable code to move to a project with the same authorship but new leadership (Vitaly and Alex). It's not a fork.

References / Thanks
Big thanks to John MacFarlane for his work on the CommonMark spec and reference implementations. His work saved us a lot of time during this project's development.

Related Links:

https://github.com/jgm/CommonMark - reference CommonMark implementations in C & JS, also contains latest spec & online demo.
http://talk.commonmark.org - CommonMark forum, good place to collaborate developers' efforts.
Ports

motion-markdown-it - Ruby/RubyMotion