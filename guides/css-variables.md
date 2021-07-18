CSS variables
=========================

Overview
-----------------
CSS variables are indeed like regular css styles (color, background-color, etc.) except that their name start with `--` and they can have ANY name and ANY value:

```scss

.s-main-header {
    
    --items-gap-width: 12px; /* standard css value */
    
    --items-color: rgb(12,14,18); /* standard css value */
    
    --layout: row; /* custom string value "row"*/
    
    --anything-you-want: anything-you-want-basically; /* custom string value */
    
}

```

Then you can use these variables to assign values to the regular css styles using the keyword "var":

```scss

.s-main-header {
    
    --items-gap-width: 12px; /* standard css value */
    
    --items-color: rgb(12,14,18); /* standard css value */
    
    --layout: row; /* custom string value "row"*/
    
    --anything-you-want: anything-you-want-basically; /* custom string value */
    
    &--item {
      color: var(--items-color);
      padding: 0 var(--items-gap-width)      
    }    
}

```

It's a very straigforward concept. You can read more details at [this MDN page](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties). 

>> DO NOT CONFUSE CSS VARIABLES WITH SCSS VARIABLES!!!!! They are two different topics and each has its own application and place. They cannot replace each other. As you learn more about css variables you will be able to tell when to use css and when to use scss variables. In short: scss variables are constant values that remain the same cross all of your elements. But css variables can change from element to element, and that's what makes them a perfect fit for using with "modifiers" in BEM (see the next section ...)

With BEM
------------------
One of the main applications of css variables is to write modifiers for your components. As an example, assume you want to write a class called `s-main-header` to style our header component. To keep things simple, also assume this header has one type of "element" which are its menu items:

```html

<article class="s-main-header">
    
    <menu>
        <li><a>Link 1</a></li>
        <li><a>Link 2</a></li>
        <li><a>Link 3</a></li>
        <li><a>Link 4</a></li>
    </menu>
</article>

```

And here is a simple stylesheet to add colors to the links:

```scss

.s-main-header {
    
    padding:0 16px;
    background-color: white;
    
    menu > li > a {
        color: $primary-color;
    }
}

```

Noe suppose you also want to write a modifier `--dark` for your header which reverses the text and background color (i.e., background color becomes primary and text color becomes white). Without using css variables, you have to overwrite *EVERY* occurance of color or background-color in your css:

```scss

.s-main-header {
    
    padding:0 16px;
    background-color: white;
    
    menu > li > a {
        color: $primary-color;
    }
  
    &--dark {
      // overwriting 
      background-color: white;

      // overwriting
      menu > li > a {
        color: white;
      }
    }
}

```

The above example is not very long, you only have to overwrite two styles, but in real-world cases you may need to overwrite quite a lot of styles ... Now using css variables, no need to overwrite anything, just change the variables for the modifier:

```scss

.s-main-header {
  
    // css variables (all names are arbitrary and up to us)
    --text-color: #{$primary-color};
    --bg-color: white;
  
    // modifier(s)
    &--dark {
      // changing the variables for the dark modifier
      --text-color: white;
      --bg-color: #{$primary-color};
    }
    
    // now defining styles 
  
    padding:0 16px;
    background-color: var(--bg-color);
    
    menu > li > a {
        color: var(--text-color);
    }
}

```

A real-world example taken from our projects
--------------------------------------------------

Styles for a page header.

Html:
```html
<header class="s-main-header s-main-header--dark s-main-header--no-shadow s-main-header--narrow">
        <div class="container d-flex align-items-center">
            <!-- company logo -->
            <div class="s-main-header__logo">
                <a href="/home">
                    Admin Console
                </a>
            </div>

            <!-- right menu -->
            <nav class="s-main-header__right-nav">
                <menu class="">
                    <li>
                        <a>
                            Link 1
                        </a>
                    </li>
                    <li>
                        <button class="s-btn s-btn--round s-btn--light">
                            Logout
                        </button>
                    </li>
                </menu>
            </nav>
        </div>
    </header>
```

Style:
```scss

.s-main-header {

  // css variables to be used with modifiers
  --padding-top: 1rem;
  --padding-bottom: 1rem;
  --box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  --text-color: #{$primary-color};
  --bg-color: transparent;


  // Modifiers:

  &--dark {
    --text-color: white;
    --bg-color: #{$primary-color};
  }
  &--no-shadow {
    --box-shadow: unset;
  }
  &--narrow {
    --padding-top: 0.5rem;
    --padding-bottom: 0.5rem;
  }

  // Main body styles
  padding-top: var(--padding-top);
  padding-bottom: var(--padding-bottom);
  box-shadow: var(--box-shadow);
  background-color: var(--bg-color);

  // Elements styles:

  &__logo {
    img {
      width: unset;
      height: unset;
    }

    a, p {
      color: var(--text-color);
    }
  }

  &__left-nav {
    margin-left: 3rem;
  }

  &__right-nav {
    margin-left: auto;
  }

  menu > li > a {
    color: var(--text-color);
    //text-transform: uppercase;
    //font-size: 0.9rem;

    &:hover {
      font-size: $font-size-base;
    }

    &.active {
      border-bottom: unset;
      color: $body-color;
      &:hover {
        font-size: 0.9rem;
      }
    }
  }
}

```

Note how css variables facilitate defining multiple modifiers for a block.


The structure of a scss sheet
----------------------------------
Based on the previous section example, we can come up with the following 4 sections for an scss styling:

```scss

.class-name {

  // 1 - css variables to be used with modifiers
  --some-variable: initial-value;  
  // more variables ...
  
  // 2 - Modifiers (which change the css variables)
  &--modifier {
    --some-variable: some-value;
  }
  // more modifiers ...
  
  // 3 - Main body styles
  padding-top: 12px;
  // more styles ...

  // 4 - Elements styles:
  &__element {
    // some styles ...
  }
  
}
```

Notes
------------------------------

### 1
In order to assign a scss variable's value to a css value DO NOT FORGET to use the interpolation `#{}` operator:

```scss
// WILL NOT WORK - the scss variable name is treated as a STRING -> the css variable value will become the string "$primary-color"
--text-color: $primary-color


// Will work
--text-color: #{$primary-color}

```

The reason for this is, css variables can take any value, including any arbitrary string. So to tell scss compiler that this is a scss variable name we have to use `#{}`

### 2
The css variable is especially useful to write modifiers(ex: --no-shadow, --narrow and --dark).

### 3
When you change a CSS variable `--name` within an element, in all places within that element `var(--name)` will be changed too. This makes css variables useful to dynamic changes.

### 4
In addition to css, you can also define/update a css variable using javascript or media query. On the other hand, scss variables are replaced when you compile your scss files and generate the css. It menas, after compilation, scss variables no longer exist, and so they cannot be used in a dynamic way as css variables do.

