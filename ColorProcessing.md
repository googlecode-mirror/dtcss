# Color Processing #
DtCSS comes with color processing which makes mixing colors a lot more easier. Just wrap the color values by these functions and it's processed. Some of functions are adapted from gtkrc's color mixing functions. Functions can be nested. If you want even more lighter blue you can just do `lighter(lighter(blue))`

# Accepted Color Types #
| `#ff0000` | Hex Color Code |
|:----------|:---------------|
| `#f00`    | Short Color Code |
| `rgb(255, 0, 0)` | RGB color code |
| `red`     | Color names    |

Color names comes from the color list. It can be found at `DtCSS__colorlist.txt`. Of course you can edit it.

# List of available color mixing functions #
| **Function** | **Description** | **Example Input** | **Output** |
|:-------------|:----------------|:------------------|:-----------|
| `mix(factor, color1, color2)` | Mixes the `color1` and `color2` together. The factor is a number between 0 and 1. If factor is 1 you will get `color1`. If factor is 0 you will get `color2`. | `mix(0.3, red, blue)` | `#4c00b2`  |
| `shade(factor, color)` | Computes a shade of `color` based on `factor`. Factor is a number that is not less than 0. If factor is 1 you will get the `color`. If factor is more than one you will get the lighter variant of that color. If factor is less than 1 you will get a darker variant of that color. | `shade(0.5, yellow)` | `#5f5f1f`  |
| `lighter(color)` | Computes a lighter variant of `color`. It is the same as the `shade` function with a factor of 1.3 | `lighter(green)`  | `#00a600`  |
| `darker(color)` | Computes a darker variant of `color`. It is the same as the `shade` function with a factor of 1.3 | `darker(green)`   | `#0d4c0d`  |
| `variant(hue, saturation, lightness)` | Computes a variant of color based on hue, saturation and lightness. `hue` is a number between -360 and 360. `saturation` is a number between -100 and 100. `lightness` is a number between -100 and 100. | `variant(-40, -50, 20, red)` | `#cc65aa`  |

# Example #
For example we want to make unvisited links blue, visited links purple, and make them lighter if you hover on it.

## Input ##
```
a:link {
	color: blue;
	__self__:hover {
		color: lighter(blue);
	}
}
a:visited {
	color: purple;
	__self__:hover {
		color: lighter(purple);
	}
}
```

## Output ##
```
a:link {
    color: blue;
}

a:link:hover {
    color: #4c4cfe;
}

a:visited {
    color: purple;
}

a:visited:hover {
    color: #a600a6;
}
```