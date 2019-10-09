# HTML Style Guide #
Our HTML coding standards stem from [Google's HTML standards](https://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml), albeit with a few significant changes. 

## General Formatting ##

### Indentation ###
Indent by 2 spaces at a time.
Don't use tabs or mix tabs and spaces for indentation.

```
#!html

<ul>
  <li>Fantastic</li>
  <li>Great</li>
</ul>
```

### Capitalization ###

All code has to be lowercase: This applies to HTML element names, attributes, attribute values (unless text/CDATA), CSS selectors, properties, and property values (with the exception of strings).

```
#!html

<!-- Not recommended -->
<A HREF="/">Home</A>
	
<!-- Recommended -->
<img src="google.png" alt="Google">

```

```
#!css

/* Not recommended */
#color {
  color: #E5E5E5;
}

/* Recommended */
#color {
  color: #e5e5e5;
}
```

### Trailing Whitespace ###

Remove trailing white spaces. Trailing white spaces are unnecessary and can complicate diffs.

```
#!html

<!-- Not recommended -->
<p>What?</p>_

<!-- Recommended -->
<p>Yes please.</p>

```

## General Meta Rules ##

### Encoding ###

- Use UTF-8 (no BOM). 
- Make sure your editor uses UTF-8 as character encoding, without a byte order mark.
- Specify the encoding in HTML templates and documents via `<meta charset="utf-8">`. Do not specify the encoding of style sheets as these assume UTF-8.

### Comments ###

Explain code as needed. Leave comments when additional information is required to understand the code. Comments aren't needed as extensively with HTML since it's a markup language. 

```
#!html

<!— This is a standard HTML comment. -->
<p>Lorem ipsum</p>

```

### Action Items ###

Try to avoid marking `todos` to the best of your capability. Code that is on live should be completed projects. However, if there is a situation where the programmer needs to mark a `todo`, and it's impossible to avoid, it can still be marked.

- Highlight `todos` by using the keyword `TODO` only, not other common formats like `@@`.
- Append a contact (username or mailing list) in parentheses as with the format TODO (contact).
- Append action items after a colon as in TODO: action item.

```
#!html

<!-- TODO (cw_programmer): remove optional tags -->
<ul>
  <li>Apples</li>
  <li>Oranges</li>
</ul>

```

## HTML Style Rules ##

### Document Type ###

Use HTML5. HTML5 (HTML syntax) is preferred for all HTML documents: `<!DOCTYPE html>`

(It's recommended to use HTML, as `text/html`. Do not use XHTML. XHTML, as application/xhtml+xml, lacks both browser and infrastructure support and offers less room for optimization than HTML.)

### Semantics ###

Use elements (sometimes incorrectly called "tags") for what they have been created for. For example, use heading elements for headings, `<p>` elements for paragraphs, `<a>` elements for anchors, etc.

Using HTML according to its purpose is important for accessibility, reuse, and code efficiency reasons.

```
#!html

<!-- Not recommended -->
<div onclick="goToRecommendations();">All recommendations</div>
	
<!-- Recommended -->
<a href="recommendations/">All recommendations</a>

```

Reference the [semantic standards guide](https://html.spec.whatwg.org/multipage/semantics.html) to see a full list of when to use the correct elements.

### Multimedia Fallback ###

For multimedia, such as images, videos, animated objects via canvas, make sure to offer alternative access. For images that means use of meaningful alternative text (alt) and for video and audio transcripts and captions, if available.

Providing alternative contents is important for accessibility reasons: A blind user has few cues to tell what an image is about without `@alt`, and other users may have no way of understanding what video or audio contents are about either.

(For images whose alt attributes would introduce redundancy, and for images whose purpose is purely decorative which you cannot immediately use CSS for, use no alternative text, as in `alt=""`.)

```
#!html

<!-- Not recommended -->
<img src="spreadsheet.png">

<!-- Recommended -->
<img src="spreadsheet.png" alt="Spreadsheet screenshot.">

```

### Separation of Concerns ###

Strictly keep structure (markup), presentation (styling), and behavior (scripting) apart, and try to keep the interaction between the three to an absolute minimum. That is, make sure documents and templates contain only HTML and HTML that is solely serving structural purposes. Move everything presentational into style sheets, and everything behavioral into scripts.

In addition, keep the contact area as small as possible by linking as few style sheets and scripts as possible from documents and templates.

Separating structure from presentation from behavior is important for maintenance reasons. It is always more expensive to change HTML documents and templates than it is to update style sheets and scripts.

```
#!html

<!-- Not recommended -->
<h1 style="font-size: 1em;">Lorem ipsum</h1>
<p>I’ve read about this on a few sites but now I’m sure:</p>
<u>HTML is silly</u>
<center>I can’t believe there’s no way to control the styling of my website without doing everything all over again.</center>

<!-- Recommended -->
<h1>My first CSS-only redesign</h1>
<p>I’ve read about this on a few sites but today I’m actually doing it: separating concerns and avoiding anything in the HTML of my website that is presentational.</p>
<p>It’s awesome!</p>

```

### Do Not Use Unnecessary Elements and Tags ###
There are some HTML elements that should be avoided as often as possible, unless it's completely unavoidable. Most often just write external CSS to achieve the specific styling. Some elements to avoid using include:

- `&nbsp;` - Use margin, padding, or a combination of other CSS. The only acceptable time is when words should be kept together.
- `<small>` - Use font-size
- `<u>` - Use text-decoration.

### Use `<br>` Tags for Content ###
`<br>` elements must be used only for line breaks that are actually [part of the content](http://www.w3.org/html/wg/drafts/html/master/semantics.html#the-br-element), as in poems or addresses.

```
#!html

<!-- The following example is the correct usage of the br element: -->
<p>P. Sherman<br>
42 Wallaby Way<br>
Sydney</p>

```

`<br>` elements must not be used for separating thematic groups in a paragraph.

```
#!html

<!-- The following examples are non-conforming, as they abuse the br element: -->
<p><a ...>34 comments.</a><br>
<a ...>Add a comment.</a></p>

<p><label>Name: <input name="name"></label><br>
<label>Address: <input name="address"></label></p>

<!-- Here are alternatives to the above, which are correct: -->
<p><a ...>34 comments.</a></p>
<p><a ...>Add a comment.</a></p>

<p><label>Name: <input name="name"></label></p>
<p><label>Address: <input name="address"></label></p>
```

### Use Tags for Semantics, Not Presentational: `<b>` vs. `<strong>` and `<em>` vs. `<i>` ###

Often programmers want to quickly make presentational changes to make text bold or italicized. If that's the desired outcome, add a CSS class to the HTML element and give it the suitable property. For a more in-depth explanation, take a look at these articles from [W3C](http://www.w3.org/International/questions/qa-b-and-i-tags) and [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/strong).

All of these tags are used for semantic purposes rather than presentational purposes. Some semantic purposes include localization, optimizing for screen readers, and SEO. If the programmer wishes to make presentational changes, just use CSS.

### Use `<hr>` Element for Paragraph Breaks ###
As per [W3C](http://www.w3.org/wiki/HTML/Elements/hr): The `<hr>` element represents a paragraph-level thematic break. Some examples include a scene that changes in a story, or transitioning to another topic within a section of a reference book. Do not use the `<hr>` element stylistically, use CSS instead.

```
#!html

<!-- Correct -->
<p>Radios use the electromagnetic spectrum in the meter range and longer.</p>
<hr>
<p>Signal flares use the electromagnetic spectrum in the nanometer range.</p>

<!-- Incorrect -->
<h1>A Page Heading</h1>
<hr>
```

### Void Elements ###

As per [W3C](https://dev.w3.org/html5/html-author/#void):

>The term void elements is used to designate elements that must be empty. These requirements only apply to the HTML syntax. In XHTML, all such elements are treated as normal elements, but must be marked up as empty elements.
>
>These elements are forbidden from containing any content at all. In HTML, these elements have a start tag only. The self-closing tag syntax may be used. The end tag must be omitted because the element is automatically closed by the parser.
>
>Such elements include, among others, `br`, `hr`, `link` and `meta`.

It's recommended to not include the slashes for void elements as it’s automatically closed by the parser, but as closing void elements is still supported in the specs, it's acceptable to close void elements too.

```
#!html

<!-- Recommended -->
<hr>

<!-- Not Recommended But Acceptable -->
<hr/>
<hr />

```

### Entity Reference ###
**Google recommends not using entity references, we should revisit this. E.g. `&mdash;`**

## HTML Formatting Rules ##

### General Formatting ###
Use a new line for every block, list, or table element, and indent every such child element.

Independent of the styling of an element (as CSS allows elements to assume a different role per display property), put every block, list, or table element on a new line.

Also, indent them if they are child elements of a block, list, or table element.

(If you run into issues around whitespace between list items it's acceptable to put all li elements in one line. A linter is encouraged to throw a warning instead of an error.)

```
#!html

<p><em>Space</em>, the final frontier.</p>

<ul>
  <li>Moe</li>
  <li>Larry</li>
  <li>Curly</li>
</ul>

<table>
  <thead>
    <tr>
      <th scope="col">Income</th>
      <th scope="col">Taxes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>$5.00</td>
      <td>$4.50</td>
    </tr>
  </tbody>
</table>
```

### HTML Quotation Marks ###
When quoting attributes values, use double quotation marks. Use double ("") rather than single quotation marks ('') around attribute values.

```
#!html

<!-- Not recommended -->
<a class='maia-button maia-button-secondary'>Sign in</a>

<!-- Recommended -->
<a class="maia-button maia-button-secondary">Sign in</a>
```
