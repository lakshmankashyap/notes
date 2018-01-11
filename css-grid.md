# CSS Grid

### First CSS Grid

```css
.container {
    display: grid;
    grid-template-columns: 100px auto 100px; // 3 columns 
    grid-template-rows: 50px 50px; // 2 rows
    grid-gap: 3px; // 3px between each rows and columns
}
```

### Fraction units and repeat

```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr); // = 1fr 1fr 1fr;
    grid-template-rows: repeat(2, 50px);
}

/* SHORTER VERSION */

.container {
    display: grid;
    grid-template: repeat(2, 50px) / repeat(3, 1fr);
}
```

### Positioning items

```css
.container {
    display: grid;
    grid-gap: 3px;
    grid-template-columns: repeat(12, 1fr); // 12 columns
    grid-template-rows: 40px 200px 40px;
}

.header {
    /*
    starts at second column and target the very last column
    */
    grid-column: 2 / -1;
}

.menu {
    /*
    First row expands from the beginning to the middle
    Second column separates the tw columns
    Third row expands from the middle to the end
    */
    grid-row: 1 / 3;
}

.content {
    grid-column: 2 / -1;
}

.footer {
    grid-column: 1 / -1;
}
```

### Template areas

```css
.container {
    /*
    combine "auto" in grid-template-rows
    and 100% height make the page fill up all the space
    */
    height: 100%;
    display: grid;
    grid-gap: 3px;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: 40px auto 40px;
    /*
    we name cells of our layout and build it
    */
    grid-template-areas: 
        ". h h h h h h h h h h h" 
        "m c c c c c c c c c c c"
        ". f f f f f f f f f f f"; // . means blank cell
}

.header {
    grid-area: h;
}

.menu {
    grid-area: m;
}

.content {
    grid-area: c;
}

.footer {
    grid-area: f;
}
```

### auto-fit and minmax

```css
.container {
    display: grid;
    grid-gap: 5px;
    /*
    minmax is a function accepting a minimum value and maximum value
    here our cells must be at least 100px and at maximum fill all the blank space

    auto-fit means we fit as many columns as we can into the grid
    */
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); // gives a responsive grid
    grid-template-rows: repeat(2, 100px);
}
```

The version above has a problem, if we shrink the screen, there'll be more rows than 2. Thus, the added rows won't have the given size.

This is how we correct that :

```css
.container {
    display: grid;
    grid-gap: 5px;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
    grid-auto-rows: 100px;
}
```

### Image grid

```css
.container {
    display: grid;
    grid-gap: 5px;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-auto-rows: 75px;
    /*
    controls the placement algorithm
    dense means it will fill up all the blank spaces
    */
}

.horizontal {
    grid-column: span 2; // fills automatically two columns
}

.vertical {
    grid-row: span 2; // fills automatically two rows
}

.big {
    grid-column: span 2;
    grid-row: span 2;
}
```

### Named lines

```css
.container {
    height: 100%; 
    display: grid;
    grid-gap: 3px;
    grid-template-columns: [main-start] 1fr [content-start] 5fr [content-end main-end];
    grid-template-rows: [main-start] 40px [content-start] auto [content-end] 40px [main-end]; 
}

.header {
    /*
    magic happens here because of the dashes
    behind the scenes, it is : grid-column: main-start / main-end
    */
    grid-column: main;
}

.menu {}

.content {
    /*
    we use grid-area instead of grid-column because we boxed the content with main-start and main-end
    */
    grid-area: content;
}

.footer {
    grid-column: main;
}
```

### justify-content and align-content

It works like the `flexbox` : 

```css
.container {
    border: 1px solid black;
    height: 100%;
    display: grid;
    grid-gap: 5px;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(2, 100px);
    justify-content: space-around;
    align-content: center;
}
```

### justify-items and align-items

```css
.container {
    height: 100%; 
    display: grid;
    grid-gap: 3px;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: 40px auto 40px;
    // justify-items: center;
    // align-items: center;
}

.header {
    grid-column: 1 / -1;
}

.menu {
    grid-column: 1 / 3;
}

.content {
    grid-column: 3 / -1;
    // justify-self: center;
    // align-self: end;
}

.footer {
    grid-column: 1 / -1;
}
```

### auto-fit vs auto-fill

```css
.container {
    border: 1px solid black;
    display: grid;
    grid-gap: 5px;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-template-rows: 100px 100px;
}
```

Let's say we have 4 columns and a wide screen, both `auto-fit` and `auto-fill` will create empty column tracks. However, `auto-fit` will collapse them. This results in empty spaces with `auto-fill`.