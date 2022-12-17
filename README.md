# Useful bookmarklets

A collection of bookmarklets I use frequently.

To activate a bookmarklet by typing a keyword into the address bar:
*	Chrome
	1.	Right-click the address bar, 'Manage search engines and site search' to reach <tt>chrome://settings/searchEngines</tt>
	2.	'Add' a site search and enter a keyword into 'Shortcut'
*	Edge
	1.	Visit <tt>edge://settings/searchEngines</tt>
	2.	'Add' a site search and enter a keyword into 'Keyword'
*	Firefox
	1.	Add a bookmark and enter a keyword into 'Keyword'

Note that bookmarklets don't need to be URL-encoded. You can just paste a block of JavaScript starting with `javascript:` into the URL of the bookmark (Firefox) or search engine.

## Kill Sticky

<a href="https://github.com/t-mart/kill-sticky">Kill Sticky</a> is the best bookmarklet and it's in someone else's repository.

Suggested keyword: <kbd>ks</kbd> ("kill sticky")

## Replace all font-family with your preferred typefaces

```js
javascript:(function() {
	document.querySelectorAll('*').forEach(e => {
		if (!e.style.fontFamily.includes("monospace")) {
			e.style.fontFamily = 'system-ui, sans-serif';
		} else {
			e.style.fontFamily = 'monospace';
		}
	});
})();
```

Suggested keyword: <kbd>ff</kbd> ("font family")

## Invert all images

Sometimes useful with Dark Reader, which will generally not invert images.

```js
javascript:(function() {
	document.querySelectorAll('canvas, img').forEach(e => {
		e.style.filter = 'invert(1) hue-rotate(180deg)';
	});
})();
```

Suggested keyword: <kbd>inv</kbd> ("invert")

## Set a gray background-color for all images

Sometimes useful with Dark Reader when images with a transparent background are made unreadable (<a href="https://github.com/darkreader/darkreader/issues/356">#356</a>, <a href="https://github.com/darkreader/darkreader/issues/3753">#3753</a>). Both black and white will be readable on gray.

```js
javascript:(function() {
	document.querySelectorAll('canvas, img').forEach(e => {
		e.style.backgroundColor = '#888';
	});
})();
```

Suggested keyword: <kbd>fdr</kbd> ("fix dark reader")

## Override all colors

Useful for pages that are unreadable due to broken CSS.

```js
javascript:(function() {
	document.querySelectorAll('*').forEach(e => {
		e.style.background = '#E4DCD7 !important';
		e.style.color = '#000 !important';
		e.style.textShadow = 'none !important';
	});
	document.querySelectorAll(':link, :link *').forEach(e => {
		e.style.color = '#0000EE !important';
	});
	document.querySelectorAll(':visited, :visited *').forEach(e => {
		e.style.color = '#551A8B !important';
	});
})();
```
