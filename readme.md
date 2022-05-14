# The Everything Menu

Demo: https://bryanhadaway.com/testing/the-everything-menu.html

Donate: https://bryanhadaway.com/donate

## The Goal?

To support everything a menu needs to be in the modern world, as simply and semantically as possible.

* [W3C Valid](https://validator.w3.org/nu/?doc=https%3A%2F%2Fbryanhadaway.com%2Ftesting%2Fthe-everything-menu.html)\*
* [WebAIM Valid](https://wave.webaim.org/report#/https://bryanhadaway.com/testing/the-everything-menu.html)
* [Schema Valid](https://validator.schema.org/#url=https%3A%2F%2Fbryanhadaway.com%2Ftesting%2Fthe-everything-menu.html)

*\*There's a warning about `role="navigation"`, but it's my understanding that this is still recommended where accessibility is concerned for backward-compatability with some screen readers. Maybe it's now safe to remove this in 2022?*

## How?

This menu uses a checkbox to toggle the mobile menu open and close, but it does not rely on the popular `label + checkbox` hack. You're hovering on or tabbing to the *actual checkbox itself* and then clicking on it or hitting the <kbd>spacebar</kbd> key to toggle it.

Uses the trigram symbol for the hamburger icon: https://graphemica.com/%E2%98%B0.

### CSS

```css
#menu{}
#menu ul, #menu li{display:inline;position:relative;list-style:none;padding:0;margin:0}
#menu li.parent > a:after{font-family:serif;content:' ▾'}
#menu a{display:inline-block;font-size:18px;text-decoration:none;padding:15px}
#menu a:hover, #menu a:focus, #toggle:hover, #toggle:focus{color:#777;transition:all .5s ease}
#menu ul.sub-menu{display:block;position:absolute;top:100%;left:-9999px;margin-top:15px;background:#f6f6f6;z-index:9999}
#menu ul.sub-menu a{width:100%;font-size:16px}
#menu li.parent a:hover + ul.sub-menu,
#menu li.parent a:focus + ul.sub-menu,
#menu li.parent a + ul.sub-menu:hover,
#menu li.parent a + ul.sub-menu:focus-within{left:0}
#toggle:before{content:'\002630'}
#toggle{display:none;appearance:none;font-family:serif;font-size:40px;text-align:center;margin:0 auto;cursor:pointer}
.visually-hidden:not(:focus):not(:active){position:absolute !important;height:1px;width:1px;overflow:hidden;clip:rect(1px 1px 1px 1px);clip:rect(1px, 1px, 1px, 1px);white-space:nowrap}
@media(max-width:768px){
#menu{display:none}
#toggle,
#toggle:checked + #menu,
#toggle:checked + #menu a,
#toggle:checked + #menu ul.sub-menu{display:block;position:relative;left:0}
#toggle:checked + #menu a{padding:15px 0}
#toggle:checked + #menu ul.sub-menu{margin:0;background:none}
#toggle:checked + #menu ul.sub-menu a{margin-left:15px}
}
```

*(there are a few opinionated bits in there, so you can probably strip it down to be even more barebones)*

### HTML

```html
<label for="toggle"><span class="visually-hidden">Menu</span></label>
<input id="toggle" type="checkbox" />
<nav id="menu" role="navigation" itemscope itemtype="https://schema.org/SiteNavigationElement">
<ul>
<li><a href="#" itemprop="url"><span itemprop="name">Testing</span></a></li>
<li><a href="#" itemprop="url"><span itemprop="name">Accessibility</span></a></li>
<li class="parent" itemprop="url"><a href="#"><span itemprop="name">Can</span></a>
<ul class="sub-menu">
<li class="child" itemprop="url"><a href="#"><span itemprop="name">Be</span></a></li>
<li class="child" itemprop="url"><a href="#"><span itemprop="name">Very</span></a></li>
<li class="child" itemprop="url"><a href="#"><span itemprop="name">Difficult</span></a></li>
</ul>
</li>
<li><a href="#" itemprop="url"><span itemprop="name">Forgive</span></a></li>
<li><a href="#" itemprop="url"><span itemprop="name">Me</span></a></li>
</ul>
</nav>
```

### JavaScript/jQuery (optional)

```javascript
<script src="https://unpkg.com/jquery@latest/dist/jquery.min.js"></script>

<script>

jQuery(document).ready(function($) {
$("input:checkbox").keypress(function(e) {
if((e.keyCode ? e.keyCode : e.which) == 13) {
$(this).trigger("click");
}
});
});

</script>
```

*(I think a lot of users have come to expect that the <kbd>enter</kbd> key will also toggle mobile menus, so it's probably a good idea to add this as well)*
