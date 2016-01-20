# About DtCSS #

DtCSS is a PHP script that preprocesses your CSS file. It speeds up CSS coding by extending the features to CSS. Such as nested selectors, color mixing and more.
DtCSS reads the CSS file with special syntax written for DtCSS, and outputs the standard CSS. It also comes with a smart caching system.

You can download DtCSS from the [downloads](http://code.google.com/p/dtcss/downloads/list) page, or checkout from SVN. A [quick installation](QuickInstallation.md) guide is available.

# Example of DtCSS #

For detailed information of syntaxes in DtCSS, read the [Syntax Guide](SyntaxGuide.md).

DtCSS processes this CSS file:

```
#define HEADER_COLOR red
h1, h2, h3 {
	color: HEADER_COLOR;
	a:link {
		color: blue;
	}
	a:visited {
		color: darker(blue);
	}
}
```

And outputs:

```
h1, h2, h3 {
    color: red;
}

h1 a:link, h2 a:link, h3 a:link {
    color: blue;
}

h1 a:visited, h2 a:visited, h3 a:visited {
    color: #1a1a97;
}
```

# Another Example #

Some advanced features in the latest version of DtCSS

```
#define mp margin, padding
#define bg background
#define fg color

#define FONT Verdana, sans-serif

html, body {
	mp: 0;
	bg: #000;
	fg: #eee;
}
body {
	font: small FONT;
}
input, textarea {
	font: 1em FONT;
}

.fancy {
	border-top-left: 2px solid #00f;
	border-bottom-right: 4px dashed #f00;
	b {
		color: yellow;
	}
}
```

Will produce this CSS:

```
html, body {
    /* margin, padding */
    margin: 0;
    padding: 0;
    background: #000;
    color: #eee;
}

body {
    font: small Verdana, sans-serif;
}

input, textarea {
    font: 1em Verdana, sans-serif;
}

.fancy {
    /* border-top-left */
    border-top: 2px solid #00f;
    border-left: 2px solid #00f;
    /* border-bottom-right */
    border-bottom: 4px dashed #f00;
    border-right: 4px dashed #f00;
}

.fancy b {
    color: yellow;
}
```