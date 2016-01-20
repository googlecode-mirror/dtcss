## Input example ##
```
#define DARK_STYLE 1

body {
    #ifdef DARK_STYLE
        background: #000;
        #define TEXT_COLOR #eee
    #endif
    #ifndef DARK_STYLE
        background: #fff;
        #define TEXT_COLOR #444
    #endif
    color: TEXT_COLOR;
}
```

### ([R6](https://code.google.com/p/dtcss/source/detail?r=6)) ###
```
#define DARK_STYLE 1

body {
    #ifdef DARK_STYLE
        background: #000;
        #define TEXT_COLOR #eee
    #else
        background: #fff;
        #define TEXT_COLOR #444
    #endif
    color: TEXT_COLOR;
}
```

## Output ##
```
body {
    background: #000;
    color: #eee;
}
```

By deleting the line that says...

```
#define DARK_STYLE 1
```

...is deleted or commented, it will output this instead:

```
body {
    background: #fff;
    color: #444;
}
```