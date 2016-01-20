# The guide to expression writing in DtCSS #

Starting from [R8](https://code.google.com/p/dtcss/source/detail?r=8), CSS values in DtCSS can be in expressions. See the [syntax guide](SyntaxGuide.md) on how to use it. This page will focus on writing expessions.

The expression system is eval-based. The code is filtered in 3 pass to make sure that the code will be safe. The syntax is modified to be more simple. The use of functions are limited to the function in the allowed list.

# Macros #

Macros may be used anywhere is the expressions. Only limitation is that a macro must be using alphanumeric characters and underscore and they can't start with a number!

For example, I have set this macro:

```
#define COLUMN_WIDTH 120
```

I want to make a column container that will contain 2 columns. I can use this code:

```
.columns-container {
	width: <# (COLUMN_WIDTH * 2) #>px;
}
```

Or:

```
.columns-container {
	width: <# (COLUMN_WIDTH * 2) . "px" #>;
}
```

# Expressions #

## Styles of writing ##

You can make expressions in PHP style. But it is limited to be very simple. It is modified to be even more simple. It only support:

  * Strings (single, double quotes) - Same as PHP, but variables are not expanded.
  * Numbers (integers and floats are acceptable)
  * Some Operators (assignment operators are disabled)
  * Function Calls (in allowed list only)

From the above example, the expression is

```
(COLUMN_WIDTH * 2) . "px"
```

Now I will show you more ways of doing this. First. If you know about the priority of operators, you will know that the parentheses can be removed like this.

```
COLUMN_WIDTH * 2 . "px"
```

When 2 numbers or group of scalar are put together, they are multiplied automatically. So no need for the times sign:

```
2(COLUMN_WIDTH) . "px"
```

Or:

```
2 COLUMN_WIDTH . "px"
```

Or even:

```
2COLUMN_WIDTH . "px"
```

If a string is present, instead of multiplication, it will be concatenation instead, so the most simple code would be.

```
2COLUMN_WIDTH "px"
```

## Function calls ##

Take another code. This one is a bit more advanced.

```
.indicator {
	position: absolute;
	left: <# 50 * sin(1.5) #>px;
	top:  <# 50 * cos(1.5) #>px;
}
```

When you want to pass one scalar to a function call then parentheses can be omitted. And because of multiplication, a star can also be removed.

```
	left: <# 50 sin 1.5 #>px;
	top:  <# 50 cos 1.5 #>px;
```

## Parentheses in Function Calls ##

When you want to pass something to a function call, if it is only one token then a parens can be optional.

  * `5` -> There is only one token.
  * `5 + 1` -> Now there are 3 tokens.

This expression:

```
rand 5, 20 + 20
```

Actually means this

```
rand(5, 20) + 20
```

**In case** you want this instead:

```
rand(5, 20 + 20)
```

You need to put parens like in PHP.

So it is up to you whether to use parens or not. If you are familiar you can omit the parentheses, stars and dots. Just be careful. If you are not, you are recommended to use the PHP style.

# Allowed functions #

Allowed functions are listed in `libdtexpression.php`