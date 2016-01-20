# Compatibility #

DtCSS is under development. When new syntaxes are introduced they will be mentioned.

For example, if I say ([R8](https://code.google.com/p/dtcss/source/detail?r=8)) it means that syntax is available on DtCSS [R8](https://code.google.com/p/dtcss/source/detail?r=8) and later.

# Features Changelog #
  * ([R27](https://code.google.com/p/dtcss/source/detail?r=27)) 10. Property Grouping
  * ([R8](https://code.google.com/p/dtcss/source/detail?r=8)) 9. Expressions

# 1. Nested CSS ruleset #

You can put rulesets inside of a ruleset. This makes it easier to select elements in the same parents. Just make sure that you have put a semicolon (`;`) before the selector.

| **`__self__`** | Use `__self__` to prevent space from being added before it. Without `__self__` it would be `h1 a :link` which is not the right thing. |
|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|

## Input Example ##
```
h1 {
    color: red;
    a {
        font-size: 0.8em;
        __self__:link {
            color: blue;
        }
        __self__:visited {
            color: purple;
        }
    }
}
```

## Output ##
```
h1 {
    color: red;
}

h1 a {
    font-size: 0.8em;
}

h1 a:link {
    color: blue;
}

h1 a:visited {
    color: purple;
}
```

# 2. Macros #

You can create macros by using the `#define` directive. Macros are useful when you want to use common values for something, e.g., colors, fonts. Macros can then be used at almost anywhere in the CSS file. A reference to the macro must appear **after** the define directive.

| **`#define`** _`MACRO_NAME value`_ | Creates a macro with the name `MACRO_NAME` with the value `value` |
|:-----------------------------------|:------------------------------------------------------------------|

## Input Example ##
```
#define HEADING_COLOR red
#define HEADING_ELEMENTS h1, h2, h3, h4, h5, h6

HEADING_ELEMENTS {
	color: HEADING_COLOR;
}
```

## Output ##
```
h1, h2, h3, h4, h5, h6 {
    color: red;
}
```

# 3. Variables #

When a query string variable is added to the end of CSS file, it will become a macro. The macro's name is processed like this.

  1. The macro's name is changed to upper case.
  1. All non-alpha numeric characters are converted to underscores.

That means that by visiting this URL:

> `test.ipu.css?font-name=Verdana&font-size=20px`

Would create 2 macros like this:

| **Macro Name** | **Value** |
|:---------------|:----------|
| FONT\_NAME     | Verdana   |
| FONT\_SIZE     | 20px      |

The files will be cached uniquely for that set of parameters.

## Input Example ##
```
body, input, select, option, textarea {
	font: FONT_SIZE FONT_NAME;
}
```

## Output Example ##
```
body, input, select, option, textarea {
    font: 20px Verdana;
}
```

# 4. Include CSS code from other files #
You can include other files to use with DtCSS.

| **`#include`** _`"filename.css"`_ | This will include the file named `filename.css` to the current CSS file for preprocessing. Double quotes around the file name are **required**. |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------|

## Input Example ##
```
#include "layout.css"
#include "forms.css"
body {
    #include "body.css"
}
```

## Output ##
```
/* Contents of layout.css */
/* Contents of forms.css */
body {
    /* Contents of body.css */
}
```

# 5. Conditional directives #

You can use conditional directives to enable a line of CSS code when some condition is true.

| **`#ifdef`** _`MACRO_NAME`_ | Will process if a macro named `MACRO_NAME` exists. |
|:----------------------------|:---------------------------------------------------|
| **`#ifndef`** _`MACRO_NAME`_ | Will process if a macro named `MACRO_NAME` does not exists. |
| **`#ifexpr`** _`expression`_ | ([R8](https://code.google.com/p/dtcss/source/detail?r=8)) Will process if an [expression](ExpressionSyntax.md) evaluates to true. |
| **`#endif`**                | Ends the matching conditional directive.           |
| **`#else`**                 | ([R8](https://code.google.com/p/dtcss/source/detail?r=8)) Will process if the last matched conditional directive is true. |

  * [Examples of conditional directives.](ConditionalDirectivesExamples.md)

# 6. Single line comments #

Single line comments will be converted to multiline comments, automatically.

```
// hello
```

Will be converted to

```
/* hello */
```

# 7. Template ruleset #

When a set of declarations are used multiple times, you can specify an abstract ruleset for using with multiple selectors.

## Input example ##
```
%thickbox {
    border: 2px solid #000;
    margin: 0.2em 1em;
    padding: 0.8em;
}

.info {
	use: thickbox;
	border-color: green;
}

.error {
	use: thickbox;
	border-color: red;
}
```

## Output ##
```
.info {
    /* % thickbox */
    border: 2px solid #000;
    margin: 0.2em 1em;
    padding: 0.8em;
    /* thickbox % */
    border-color: green;
}

.error {
    /* % thickbox */
    border: 2px solid #000;
    margin: 0.2em 1em;
    padding: 0.8em;
    /* thickbox % */
    border-color: red;
}
```

# 8. Color Processing #

This is one of unique things in DtCSS. It comes with [Color Processing](ColorProcessing.md).

  * [Read about color processing](ColorProcessing.md)

# 9. Expressions ([R8](https://code.google.com/p/dtcss/source/detail?r=8)) #

DtCSS can also do some simple maths and strings calculation and manipulation.

One limitations is the processed file will not be cached if an expression is present in the CSS file.

## Using expressions in conditional expression. ##

You can use the `#ifexpr` to make it process the code only if the expression evaluates to true.

### Input Example ###
The following example makes heavy use of directives.

```
#ifexpr date('m-d') == '04-01'
	#define APRIL_FOOLS 1
#endif

#define BACKGROUND_COLOR white

#ifdef APRIL_FOOLS
	#define BACKGROUND_COLOR pink
#endif

body {
	background: BACKGROUND_COLOR;
}
```

From the above code, the background color will be pink on April 1st of any year.

## Using expressions in CSS values ##

The expression must be in between `<#` and `#>`.

### Input Example ###
```
#define WINDOW_WIDTH 500px

.floating {
	position: absolute;
	top: 50px;
	left: 50%;
	width: WINDOW_WIDTH;
	margin-left: <# intval(WINDOW_WIDTH) / -2 . "px" #>
}
```

### Output ###
```
.floating {
    position: absolute;
    top: 50px;
    left: 50%;
    width: 500px;
    margin-left: -250px;
}
```

## Syntax ##

The expression looks like in PHP, but it is a bit more simple.

In DtCSS, you can write expressions like in PHP, like this:

```
intval(WINDOW_WIDTH) / -2 . "px"
```

Or you may use a more simple syntax like this:

```
int WINDOW_WIDTH / -2 "px"
```

  * [Expression Syntax Guide](ExpressionSyntax.md)

# Property Grouping ([R27](https://code.google.com/p/dtcss/source/detail?r=27)) #

Commas can be used to group properties together. For example,

```
margin, padding: 0;
```

Left, top, right, bottom can be combined as one.

```
border-top-left-color: #f5f5f5;
```

## Input Example ##
```
#define BUTTON_COLOR red
.button {
	padding, margin: 1em;
	background: shade(1.8, BUTTON_COLOR);
	color: darker(darker(BUTTON_COLOR));
	border-top-left: 1px solid shade(1.95, BUTTON_COLOR);
	border-bottom-right: 1px solid shade(1.6, BUTTON_COLOR);
}
```

## Output ##
```
.button {
    padding: 1em;
    margin: 1em;
    background: #fecccc;
    color: #5d1f1f;
    border-top: 1px solid #fff2f2;
    border-left: 1px solid #fff2f2;
    border-bottom: 1px solid #ff9999;
    border-right: 1px solid #ff9999;
}
```














