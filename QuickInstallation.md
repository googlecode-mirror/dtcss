﻿#summary Quick installation guide for most users.
#labels Featured

# Quick Installation #

First, to install DtCSS, you need to [download](http://code.google.com/p/dtcss/downloads/list) it first. You need to have `mod_rewrite` or similar modules enabled for it to work.

Now go to document root, and make a folder called `dtcss`. Then extract the files there. Now you should have these in your document root:

  * /dtcss/DtCSScolorlist.txt
  * /dtcss/libdtcss.php
  * /dtcss/libdtexpression.php
  * /dtcss/rewrite\_css.php

Create another folder called `cache` inside the `dtcss` folder. The cache files will be stored here. Don't forget to chmod the cache folder to be writable by the web application. Now you should have this added.

  * /dtcss/cache/

Go back to your document root and edit your `.htaccess` file (or your web server configuration file), and add these lines:

```
RewriteEngine   On
RewriteBase     /
RewriteRule     ^(.*\.ipu\.css)$ /dtcss/rewrite_css.php?f=$1 [QSA,L]
```

The above `.htaccess` code will rewrite all files ending with `.ipu.css` files to be processed by DtCSS.

# Make sure that it works! #

Try creating a test file in your web directory, say, `test.ipu.css` and put in the following code.

```
#define TEXT_COLOR red
#define LINK_COLOR blue

body {
	color: TEXT_COLOR;
}
a {
	__self__:link,
	__self__:visited {
		color: LINK_COLOR;
	}
	__self__:hover {
		color: lighter(LINK_COLOR);
	}
}
```

Use your web browser to navigate to that file, you should see the code formatted like this:

```
body {
    color: red;
}

a {
}

a:link, a:visited {
    color: blue;
}

a:hover {
    color: #4c4cfe;
}
```

# See also #
  * [Syntax Guide](SyntaxGuide.md)