# CSS Style Guide #

The CSS style guide is based off [Google’s coding style guide](https://google.github.io/styleguide/htmlcssguide.html#CSS) but with some modifications.

## CSS Style Rules ##

### ID and Class Naming ###
Use meaningful or generic ID and class names.

Instead of presentational or cryptic names, always use ID and class names that reflect the purpose of the element in question, or that are otherwise generic.

Names that are specific and reflect the purpose of the element should be preferred as these are most understandable and the least likely to change.

Generic names are simply a fallback for elements that have no particular or no meaning different from their siblings. They are typically needed as “helpers.”

Using functional or generic names reduces the probability of unnecessary document or template changes.

```
#!css

/* Not recommended: meaningless */
#yee-1901 {}

/* Not recommended: presentational */
.button-green {}
.clear {}

/* Recommended: specific */
#gallery {}
#login {}
.video {}

/* Recommended: generic */
.aux {}
.alt {}
```

**Needs to be revisited.**

### ID and Class Name Style ###
Use ID and class names that are as short as possible but as long as necessary.

Try to convey what an ID or class is about while being as brief as possible.

Using ID and class names this way contributes to acceptable levels of understandability and code efficiency.

```
#!css

/* Not recommended */
#navigation {}
.atr {}

/* Recommended */
#nav {}
.author {}
```

### Type Selectors ###
Avoid qualifying ID and class names with type selectors.

Unless necessary (for example with helper classes), do not use element names in conjunction with IDs or classes. Avoiding unnecessary ancestor selectors is useful for performance reasons.

```
#!css

/* Not recommended */
ul#example {}
div.error {}

/* Recommended */
#example {}
.error {}
```

### Shorthand Properties ###
CSS offers a variety of shorthand properties (like font) that should be used whenever possible, even in cases where only one value is explicitly set.

Using shorthand properties is useful for code efficiency and understandability.

```
#!css

/* Not recommended */
#example {
  border-top-style: none;
  font-family: palatino, georgia, serif;
  font-size: 100%;
  line-height: 1.6;
  padding-bottom: 2em;
  padding-left: 1em;
  padding-right: 1em;
  padding-top: 0;
}

/* Recommended */
#example {
  border-top: 0;
  font: 100%/1.6 palatino, georgia, serif;
  padding: 0 1em 2em;
}
```

### 0 and Units ###
Do not use units after 0 values unless they are required.

```
#!css

margin: 0;
padding: 0;
```

### Leading 0s ###
It's recommended to omit leading "0"s in values, although it’s completely acceptable to add the 0 for readability.

```
#!css

font-size: .8em;
padding: 0.8px;
```

### Hexadecimal Notation ###
Use 3 character hexadecimal notation where possible. For color values that permit it, 3 character hexadecimal notation is shorter and more succinct.

```
#!css

/* Not recommended */
color: #eebbcc;

/* Recommended */
color: #ebc;
```

### ID and Class Name Delimiters ###
Separate words in ID and class names by a hyphen.

Do not concatenate words and abbreviations in selectors by any characters (including none at all) other than hyphens, in order to improve understanding and scannability.

```
#!css

/* Not recommended: does not separate the words "demo" and "image" */
.demoimage {}

/* Not recommended: uses underscore instead of hyphen */
.error_status {}

/* Recommended */
#video-id {}
.ads-sample {}
```

**Needs to be revisited.**

### Hacks ###
Avoid user agent detection as well as CSS "hacks"—try a different approach first.

It’s tempting to address styling differences over user agent detection or special CSS filters, workarounds, and hacks. Both approaches should be considered last resort in order to achieve and maintain an efficient and manageable code base. Put another way, giving detection and hacks a free pass will hurt projects in the long run as projects tend to take the way of least resistance. That is, allowing and making it easy to use detection and hacks means using detection and hacks more frequently—and more frequently is too frequently.

Sometimes this will be unavoidable.

## CSS Formatting Rules ##
### Declaration Order ###
Put declarations in alphabetical order in order to achieve consistent code in a way that is easy to remember and maintain.

Ignore vendor-specific prefixes for sorting purposes. However, multiple vendor-specific prefixes for a certain CSS property should be kept sorted (e.g. -moz prefix comes before -webkit).

```
#!css

background: fuchsia;
border: 1px solid;
-moz-border-radius: 4px;
-webkit-border-radius: 4px;
border-radius: 4px;
color: black;
text-align: center;
text-indent: 2em;
```

** Needs to be revisited. **

### Declaration Stops ###
End every declaration with a semicolon for consistency and extensibility reasons. 

```
#!css

/* Not recommended */
.test {
  display: block;
  height: 100px
}
/* Recommended */
.test {
  display: block;
  height: 100px;
}
```

### Property Name Stops ###
Always use a single space between property and value (but no space between property and colon) for consistency reasons.

```
#!css

/* Not recommended */
h3 {
  font-weight:bold;
}

/* Recommended */
h3 {
  font-weight: bold;
}
```

### Declaration Block Separation ###
Always use a single space between the last selector and the opening brace that begins the declaration block.
The opening brace should be on the same line as the last selector in a given rule.

```
#!css

/* Not recommended: missing space */
#video{
  margin-top: 1em;
}

/* Not recommended: unnecessary line break */
#video
{
  margin-top: 1em;
}

/* Recommended */
#video {
  margin-top: 1em;
}
```

### Selector and Declaration Separation ###
Separate selectors and declarations by new lines.

```
#!css

/* Not recommended */
a:focus, a:active {
  position: relative; top: 1px;
}

/* Recommended */
h1,
h2,
h3 {
  font-weight: normal;
  line-height: 1.2;
}
```

### Rule Separation ###
Always put a blank line (two line breaks) between rules.

```
#!css

html {
  background: #fff;
}

body {
  margin: auto;
  width: 50%;
}
```

### CSS Quotation Marks ###
Use single or double quotation marks for attribute selectors and property values.

```
#!css

/* Recommended */
@import url("//www.google.com/css/maia.css");

html {
  font-family: 'Open Sans', arial, sans-serif;
}
```

### Multi-Line CSS ###
CSS should be written across multiple lines, except in very specific circumstances. There are a number of benefits to this:

- A reduced chance of merge conflicts, because each piece of functionality exists on its own line.
- More 'truthful' and reliable diffs, because one line only ever carries one change.

Exceptions to this rule should be fairly apparent, such as similar rulesets that only carry one declaration each, for example:

```
#!css

.icon {
  display: inline-block;
  width:  16px;
  height: 16px;
  background-image: url(/img/sprite.svg);
}

.icon--home     { background-position:   0     0  ; }
.icon--person   { background-position: -16px   0  ; }
.icon--files    { background-position:   0   -16px; }
.icon--settings { background-position: -16px -16px; }
```

These types of ruleset benefit from being single-lined because

- they still conform to the one-reason-to-change-per-line rule;
- they share enough similarities that they don’t need to be read as thoroughly as other rulesets—there is more benefit in being able to scan their selectors, which are of more interest to us in these cases.
