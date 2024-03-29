# Useful bookmarklets

A collection of <a href="https://en.wikipedia.org/wiki/Bookmarklet">bookmarklets</a> I use frequently.

How to set things up so that you can activate a bookmarklet by typing a keyword into the address bar:
* Chrome
  1. Right-click the address bar, 'Manage search engines and site search' to reach <tt>chrome://settings/searchEngines</tt>
  2. 'Add' a site search with a `javascript:` URL and enter a keyword into 'Shortcut'
* Edge
  1. Visit <tt>edge://settings/searchEngines</tt>
  2. 'Add' a site search with a `javascript:` URL and enter a keyword into 'Keyword'
* Firefox
  1. Add a bookmark with a `javascript:` URL and enter a keyword into 'Keyword'

Note that bookmarklets don't need to be URL-encoded. You can just paste a block of JavaScript starting with `javascript:` into the URL of the bookmark (Firefox) or search engine.

Yes, bookmarklets can be used on mobile as well. In iOS Safari, bookmark any page, save the bookmark, then edit the bookmark and replace the URL with some `javascript:`.



## Kill Sticky

<a href="https://github.com/t-mart/kill-sticky">Kill Sticky</a> is the best bookmarklet and it is in someone else's repository.

Kill Sticky removes all elements that have `position: fixed` or `position: sticky`, which 99% of the time is junk obscuring the text you want to read.

Just add <code>javascript:</code> before <a href="https://github.com/t-mart/kill-sticky/blob/master/src/kill-sticky.js">the source here</a>.

If you find the canonical version lacking because it fails to kill Wayback Machine's sticky header (it's inside its own `#shadow-root`), use

```js
javascript:(function() {
  document.querySelectorAll('div#wm-ipp-base[style]').forEach(function(node) {
    node.parentNode.removeChild(node);
  });

  document.querySelectorAll('body *').forEach(function(node) {
    if (['fixed', 'sticky'].includes(getComputedStyle(node).position)) {
      node.parentNode.removeChild(node);
    }
  });

  document.querySelectorAll('html *').forEach(function(node) {
    const s = getComputedStyle(node);
    if ('hidden' === s['overflow']) { node.style['overflow'] = 'visible'; }
    if ('hidden' === s['overflow-x']) { node.style['overflow-x'] = 'visible'; }
    if ('hidden' === s['overflow-y']) { node.style['overflow-y'] = 'visible'; }
  });

  const htmlNode = document.querySelector('html');
  htmlNode.style['overflow'] = 'visible';
  htmlNode.style['overflow-x'] = 'visible';
  htmlNode.style['overflow-y'] = 'visible';
})();
```

Suggested keyword: <kbd>ks</kbd> ("kill sticky")

Test page: https://www.quantamagazine.org/she-turns-fluids-into-black-holes-and-inflating-universes-20221212/



## Replace all `font-family` with your preferred typefaces

```js
javascript:(function() {
  document.querySelectorAll('*').forEach(e => {
    if (!e.style.fontFamily.includes('monospace')) {
      e.style.fontFamily = 'sans-serif';
    } else {
      e.style.fontFamily = 'monospace';
    }
  });
})();
```

Suggested keyword: <kbd>ff</kbd> ("font family")

Test page: https://www.gwern.net/The-Melancholy-of-Subculture-Society



## Set `max-width` on body and unjustify text

Useful for pages with an unconstrained width, leading to very long lines.

This bookmarklet takes one argument typed after the bookmark keyword, a `max-width` in `em`s.

```js
javascript:(function() {
  const max_width = %s || 40;

  document.head.insertAdjacentHTML('beforeend', `<style>
    body {
      margin: 0 auto !important;
      max-width: ${max_width}em;
    }
  </style>`);

  document.querySelectorAll('*').forEach(e => {
    if (window.getComputedStyle(e).getPropertyValue('text-align') == 'justify') {
      e.style.textAlign = 'left';
    }
  });
})();
```

Suggested keyword: <kbd>rw</kbd> ("reading width")

Test page: https://danluu.com/input-lag/



## Override all colors

Useful for pages that are unreadable due to broken CSS.

```js
javascript:(function() {
  document.head.insertAdjacentHTML('beforeend', `<style>
    * {
      background: #E4DCD7 !important;
      color: black !important;
      text-shadow: none !important;
    }
    :link, :link * {
      color: #0000EE !important;
    }
    :visited, :visited * {
      color: #551A8B !important;
    }
  </style>`);
})();
```

Suggested keyword: <kbd>gr</kbd> ("gray background")

Test page: https://meaningness.com/modes-chart after being modified by Dark Reader's Dynamic mode.



## Inject `width=device-width`

Useful on mobile when reading pages authored before smartphones, where the font size is too small due to the lack of `<meta name="viewport" content="width=device-width" />`.

```js
javascript:(function() {
  document.head.insertAdjacentHTML('beforeend', `
    <meta name="viewport" content="width=device-width" />
    <style>
      body {
        /* Without word-break: break-word, iOS Safari 16.1 lets
         * very long words e.g. URLs widen the page */
        word-break: break-word;

        /* Don't let iOS Safari enlarge the font size when the phone is in landscape mode.
         * https://kilianvalkhof.com/2022/css-html/your-css-reset-needs-text-size-adjust-probably/
         */
        -webkit-text-size-adjust: none;
        text-size-adjust: none;
      }
    </style>
  `);
})();
```

Suggested keyword: none, as this is useful only on mobile.

Test page: http://home.hiwaay.net/~emilyj/usenet/index.html



## Invert all images

Sometimes useful with Dark Reader, which will generally not invert images.

```js
javascript:(function() {
  document.head.insertAdjacentHTML('beforeend', `<style>
    canvas, img {
      filter: invert(1) hue-rotate(180deg) !important;
    }
  </style>`);
})();
```

Suggested keyword: <kbd>inv</kbd> ("invert")

Test page: https://www.tensorflow.org/tensorboard/graphs#op-level_graph



## Set a gray `background-color` for all images

Sometimes useful with Dark Reader when images with a transparent background are made unreadable (<a href="https://github.com/darkreader/darkreader/issues/356">#356</a>, <a href="https://github.com/darkreader/darkreader/issues/3753">#3753</a>). Both black and white will be readable on <code>#888</code> (a midpoint which is untouched by Dark Reader).

```js
javascript:(function() {
  document.head.insertAdjacentHTML('beforeend', `<style>
    canvas, img {
      background-color: #888 !important;
    }
  </style>`);
})();
```

Suggested keyword: <kbd>fdr</kbd> ("fix dark reader")

Test page: https://blog.reverberate.org/2021/04/21/musttail-efficient-interpreters.html



## Reduce brightness of text and images

This is useful when using Dark Reader (for light-on-dark text) on a hidpi screen to read a long article or book.

```js
javascript:(function() {
  document.head.insertAdjacentHTML("beforeend", `<style>
    body {
      color: rgb(153 153 153);
    }
    img {
      opacity: 0.65;
    }
  </style>`);
})();
```

Suggested keyword: <kbd>da</kbd> ("darker")

Test page: https://unbook.ludios.org/ (with Dark Reader enabled)
