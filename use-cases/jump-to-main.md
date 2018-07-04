# Jumping straight to main content

This is a common technic used in almost all websites that serve users with disabilities. It is known as *Skip Links*

Examples(Sorted by simplicity and cleanlness) include
* [Webaim](https://webaim.org)
* [Github](https://github.com)
* [Delta airline](https://www.delta.com)
* [Facebook](https://www.facebook.com/)
* [United airline](https://www.united.com/)
* [Gmail](https://www.facebook.com/)

## Use-case
[United website] How to skip the navigation menu and go straight to the booking widget.

## Good implementation

```html
<div id="app">

  <a href="#travel-tab" class="skip-link">Skip to main content</a>
  <header>
    <!-- .... -->
  </header>
  <main >
    <ul role="tablist">
      <li id="travel-tab" role="tab" aria-selected="false" tabindex="-1"></li>

      <!-- ... -->

    </ul>

      <!-- ... -->
    
  </main>

</div>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #BF1722;
  color: white;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```

## Bad Implementation

### Reordering html
```html
<div id="app">

  <header>
    <!-- .... -->
  </header>
  <main >
    <ul role="tablist">
      <li id="travel-tab" role="tab" aria-selected="false" tabindex="-1"></li>

      <!-- ... -->

    </ul>

      <!-- ... -->
    
  </main>
  <a href="#travel-tab" class="skip-link">Skip to main content</a>
</div>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #BF1722;
  color: white;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```
Reason: Tab order is based on the DOM order, so the "Skip to main content" message will be announced after all focusable elements above it and this defeats the **purpose** of "skip links", which is to take user to content of interest right when they visit your website.


### Ignore `.skip-link:focus` style
```html
<div id="app">

  <header>
    <!-- .... -->
  </header>
  <main >
    <ul role="tablist">
      <li id="travel-tab" role="tab" aria-selected="false" tabindex="-1"></li>

      <!-- ... -->

    </ul>

      <!-- ... -->
    
  </main>
  <a href="#travel-tab" class="skip-link">Skip to main content</a>
</div>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #BF1722;
  color: white;
  padding: 8px;
  z-index: 100;
}
```

Forgetting to **show the skip link element** when user tabs to it, will confuse those [keyboard only users](https://webaim.org/techniques/css/invisiblecontent/#skipnavlinks).


### Ignore `tabindex="-1"` attribute of the main content element 
```html
<div id="app">
  <a href="#travel-tab" class="skip-link">Skip to main content</a>
  <header>
    <!-- .... -->
  </header>
  <main >
    <ul role="tablist">
      <li id="travel-tab" role="tab" aria-selected="false"></li>

      <!-- ... -->

    </ul>

      <!-- ... -->
    
  </main>

</div>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #BF1722;
  color: white;
  padding: 8px;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```

Browsers will not set focus on the *travel-tab* element because `li` elements are not considered `focusable` elements by default. Adding `tabindex="-1"` attribute forces the browser to recognize this `li` element as a `focusable` element.

