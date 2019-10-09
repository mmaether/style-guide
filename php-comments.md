# PHP Comment Standards #
The PHP commenting standards is once again taken from [Drupal's documentation](https://www.drupal.org/node/1354). This document will contain the most important aspects of our comment standards, for a more in-depth look please take a look at [Drupal's documentation](https://www.drupal.org/node/1354). The comment standard is important for code maintainability and to quickly understand what the code does. It’s also useful for generating JAVADOC-type documentation.

## General Considerations for API Module Parsing ##
The API module parses documentation and code in PHP files, and it expects documentation to be in a format similar to other code/documentation parsing systems such as PHPDoc, JavaDoc, etc. It was originally based on Doxygen, but it has evolved into something that has its own set of tags and a lot of Drupal-specific functionality.

The API module parses documentation that is in special documentation blocks (known as "docblocks" in the rest of this document).

Syntax example:

```
#!php
<?php
/**
* Sample summary line.
*
* Next paragraph. Etc.
*/
(code being documented goes here, such as a function declaration, class, etc.)
?>
```

General notes on the API module and the documentation it will parse:

- The API module treats files with the following extensions as PHP: `.php`, `.module`, `.inc`, `.install`, `.engine.`, `.theme`, `.profile`, and `.test`.
- When parsing a PHP file, the API module parses both documentation and PHP code. PHP files with syntax errors could cause problems.
- The special documentation tags described in this document are only recognized within special PHP comment blocks that start with `/**`. These are known in this document as docblocks.
- In-code comment lines starting with `//` and comment blocks starting with `/*` are not recognized as docblocks.
- Docblocks normally have `*` at the beginning of each line, and the API module strips these out when formatting the documentation (see example above).
- To make a paragraph break in a docblock, leave a blank line (see example above). Some tags also trigger paragraph breaks: `@param`, `@return`, `@see`, `@var`. The API module does not support blank lines within a single tag's documentation (for example, within a single parameter's documentation with @param.
- The API module itself does not care about line length (Drupal documentation standards do, however, see below).
- The first paragraph of a docblock is known as the summary.
- To document a function, class, etc., the docblock must appear directly before the item being documented, with no blank line in between (see example above). There are also a few "free-standing" docblocks (for documenting files and making topics -- see tag reference for details).
- The API module automatically turns the names of functions, classes, etc. that it recognizes in documentation text into links to the documentation of those items.

## Drupal API Documentation Standards (General) ##
In the individual tag references, there are notes about Drupal standards that pertain to that particular tag. This section contains standards related to all API docblocks.

Syntax example:

```
#!php
<?php
/**
* Sample summary line.
*
* Next paragraph. Etc.
*/
(code being documented goes here, such as a function declaration, class, etc.)
?>
```

Drupal standards notes:

- All documentation and comments should form proper sentences, use proper grammar and punctuation, and generally follow the same style guidelines as [drupal.org content](http://drupal.org/style-guide/content).
- Document the current version of code, not differences from past code or future plans (except for `@todo` and `@deprecated` tags).
- Sentences should be separated by single spaces.
- Comments and variable names should be in English, and use US English spelling (e.g., "color" not "colour").
- To refer to a module or theme, use wording and capitalization like `the Foo Bar module` (in other words, the module/theme name is a proper noun, not including the word "module" or "theme" in the proper noun).
- All caps are used in comments only when referencing constants, for example `TRUE`.
- Docblocks must have `*` at the beginning of each line (see sample above).
- Lines containing comments (including docblocks) must wrap as close to 80 characters as possible without going over, with a few exceptions (noted in the Tag Reference below).
- Lines after tags such as `@param`, `@return`, etc. that contain documentation connected with that tag are indented two spaces. See syntax examples for each tag.
- Every function, constant, class, interface, class member (function, property, constant), and file must be documented, even private class members.
- All summaries (first lines of docblocks) must be under 80 characters, start with a capital letter, and end with a period (`.`). They must provide a brief description of what a function does, what a class does, what a file contains, etc.
- When implementing a hook, use a short summary of the form `"Implements hook_menu()."` Omit the parameter and return value documentation in this case.

## Drupal Standards for In-Line Code Comments ##
Non-header or in-line comments are strongly encouraged. A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you need to comment it before you forget how it works. Comments should be on a separate line immediately before the code line or block they reference. For example:

```
#!php
<?php
// Unselect all other contact categories.
db_query(&#039;UPDATE {contact} SET selected = 0&#039;);
?>
```

If each line of a list needs a separate comment, the comments may be given on the same line and may be formatted to a uniform indent for readability.

C style comments `(/* */)` and standard C++ comments `(//) are both fine, though the former is discouraged within functions (even for multiple lines, repeat the `//` single-line comment). Use of Perl/shell style comments `(#)` is discouraged.

`@todo` statements in inline comments follow the same rules as using the `@todo` tag in docblocks. An example:

```
#!php

<?php
// Some other comment here.
// @todo Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
//   nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam 
//   erat, sed diam voluptua.
// We explicitly delete all comments.
comment_delete($cid);
?>
```

## Drupal API Documentation Standards for Functions ##

Documenting functions is somewhat more complex than documenting other types of PHP language constructs, so they have their own coding standards:

- Each parameter of a function must be documented with a `@param` tag (see exceptions below).
- If a function has a return value, it must be documented with a `@return` tag (see exceptions below).
- If there is no return value for a function, there must not be a `@return` tag.
- For most functions (see exceptions below), summary lines must start with a third person singular present tense verb, and they must explain what the function does. Example: `"Calculates the maximum weight for a list."`
- Functions that are easily described in one line may be documented by providing the function summary line only (omitting all parameters and return value).

In addition, there are standards for documenting specific special types of functions:

### Hook definition ###

The summary starts with an imperative verb that tells why a module would want to implement the hook. Example: `"Respond to node deletion."` Hook definitions are placed in a `.api.php` file, which is not directly loaded by Drupal. Each is a fake function whose name starts with "hook", and whose function body is a sample implementation. Parameters and return value that the implementer must use need to be documented.

### Hook implementation ###

Documentation uses a short format:

```
#!php

<?php
/**
* Implements hook_help().
*/
function blog_help($section) {
?>
```

If appropriate, you can add additional details pertinent to the particular hook implementation in a separate paragraph after the summary. Do not document parameters and return value though -- these are documented on the hook definition referenced in the summary line. And if the hook name has a variable portion:

```
#!php
<?php
/**
* Implements hook_form_FORM_ID_alter() for node_type_form().
*/
function mymodule_form_node_type_form_alter(&$form, &$form_state) {
?>
```

### Update functions (implementations of hook_update_N()) ###

These use a different syntax, because the documentation summary (first line) is displayed to someone running `update.php` to tell them which updates need to run. So they are documented like this:

```
#!php

<?php
/**
* Add several fields to the {node_revision} table.
*
* Longer description can go here, if necessary.
*/
function module_name_update_7002() {
?>
```

Note that the first line should start with an imperative verb, so it will make sense to people running `update.php`.

### Callbacks for built-in PHP functions ###

Many PHP functions, such as `uasort()` etc., require the caller to pass in a callback function. When documenting a function that is intended to be used as a callback in this way, start with a standard function summary line. After the summary, the next paragraph should indicate what function the callback is passed to, and which Drupal function makes use of it (which could be a list of functions). Example:


```
#!php

<?php
/**
* [standard first line]
*
* Callback for uasort() within system_themes_page().
?>
```

### Callback definitions and implementations ###

Like standard PHP functions, sometimes the Drupal API also needs callbacks to operate. In this context, a "callback" is a function that a module needs to define in conjunction with a hook implementation, batch set, plugins, or other Drupal functionality.

In order to document the parameters and return value of callbacks, make a callback function definition, with a documentation block, in a `*.api.php` file, similar to hook definition documentation (i.e., they are "dummy" functions in files that are not actually loaded, but they can be referenced in other documentation). Then, in the hook or other function that requires the implementer/caller to return a callback function name, refer to this callback definition function by name.

Notes:

- Callback definition function names in the `*.api.php` file should start with "callback_", followed by a descriptive name.
- Callback definition function bodies should be an example implementation (like hook definition function bodies).
- The one-line documentation summary should describe what the callback does. The summary starts with an imperative verb that tells why a module would want to define this callback.
- The next line should start with "Callback for ___:" (replace ___ with the name of the hook, function, or other component that uses the callback).
- They should be added to the "`@ingroup` callbacks" topic.
- The hook or other function that uses the callback should refer to the callback definition function by name in its documentation.
- An actual callback function in module code should have as its one-line documentation summary `"Implements callback_NAME()."`, so that its documentation links to the callback documentation.

### Form-generating functions ###

The form constructor, validation handler(s), and submission handler(s) form a set and are documented as in this sample:


```
#!php

<?php
/**
* Form constructor for the user login form.
*
* @param string $message
*   The message to display.
*
* @see user_login_form_validate()
* @see user_login_form_submit()
*
* @ingroup forms
*/
function user_login_form($form, &$form_state, $message = '') {
   ...
}

/**
* Form validation handler for user_login_form().
*
* @see user_login_form_submit()
*/
function user_login_form_validate($form, &$form_state) {
   ...
}

/**
* Form submission handler for user_login_form().
*
* @see user_login_form_validate()
*/
function user_login_form_submit($form, &$form_state) {
   ...
}
?>
```

Notes:

- Omit `@param` and `@return` documentation for the standard parameters and return value (such as `$form` and `$form_state`.
- Document parameters specific to this form constructor.

Please see the rest of [Drupal's documentation](https://www.drupal.org/node/1354#functions) for more examples.

## Drupal API Documentation Standards for Order of Documentation Sections ##

Docblocks for functions, classes, and other PHP items should be written in the following order to comply with Drupal coding standards. Omit sections that don't apply (for instance, `@param` only applies to functions). Separate different-type sections by a blank line (for instance, all the `@param` documentation goes together, with a blank line before the first parameter and a blank line after the last parameter before the `@return` section starts). Here's the order:

- One-line summary, ending in a period (.).
- Additional paragraph(s) of explanation.
- [@var](https://www.drupal.org/node/1354#var)
- [@param](https://www.drupal.org/node/1354#param)
- [@return](https://www.drupal.org/node/1354#return)
- [@throws](https://www.drupal.org/node/1354#throws)
- [@ingroup](https://www.drupal.org/node/1354#defgroup)
- [@deprecated](https://www.drupal.org/node/1354#deprecated)
- [@see](https://www.drupal.org/node/1354#see)
- [@todo](https://www.drupal.org/node/1354#todo)
- [@Plugin](https://www.drupal.org/node/1354#Plugin) and other annotations

## Making Lists in Documentation ##
The API module creates nested bullet lists from lines starting with `-` in docblocks.

Syntax example:

```
#!php
<?php
/**
 * Lists are usually preceded by a line ending in a colon:
 * - Item in the list.
 * - Another item.
 *   - key: Sub-list with keys, first item.
 *   - key2: (optional) Second item with a key.
 * - Back to the outer list. Sometimes list items are quite long, in which case
 *   you can wrap the text like this.
 * - Last item in the outer list.
 * Text that is outside of the list continues here.
 */
?>
```

Syntax notes:

- A hyphen as the first character on the line (except the *) indicates a list item.
- All list items at the same nesting level must be indented the same amount.
- There must not be blank lines between items in the same list.
- If a list has "keys" (for instance, a list describing an associative array, or the options for a parameter), use a colon between the key and its description. The API module will format keys with a strong tag.

Drupal standards notes:

- The line before a list or before a new level of nesting must end with a colon (:).
- The hyphen aligns with the start of the text containing that level of list, which means that each level of nesting indents by two spaces.
- Array keys are not put in quotes; keys describing literal strings (such as parameter values) should be put in quotes.
- Indicate optional list items with (optional) and default list items with (default). With keys, these words are placed just after the colon.
